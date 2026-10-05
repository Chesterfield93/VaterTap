# GitHub Copilot Instructions for VaterTap

## Project context

VaterTap is an offline-capable IoT system for a mobile beer tap on a handcart.

- **Brain** (ESP32-S3): reads 3 load cells via I2C mux + 3x NAU7802 and an NFC reader,
  detects pours, attributes them to users, journals events, syncs to the backend.
- **Display** (XIAO ESP32-S3 on EE04 with 7.5" e-paper): pure renderer and button
  input, connected to the Brain via UART.
- **Backend**: Python app inside AppDaemon, file-based persistence.

## Current phase

Concept phase. Do not generate production firmware or a full backend unless an issue
explicitly asks for it. Prefer interfaces, tests, small spikes and docs.

## Decided - do not silently change

- Two nodes. Brain owns all domain state, the journal and backend communication.
  Display has no business logic and no backend access.
- Brain <-> Display: UART, NDJSON, one message per line, `*` + CRC-16/CCITT hex suffix.
  Fields `v`, `t`, `seq` mandatory. Viewmodel is full state, cyclic, no ACK. Button
  events are ACKed with retry and dedup. Display heartbeat 1 Hz. 3 s watchdog both ways.
- Viewmodel carries display-ready values, never raw data or pixels. Display renders QR
  from a URL string and must full-refresh after showing a QR.
- Brain uses six FreeRTOS tasks: acquisition, domain, journal, link, sync, supervisor.
  Tasks talk only via queues. Domain is the only writer of domain state.
- Load cells: 3x 30 kg, each with its own NAU7802 behind an I2C mux (fixed address
  0x2A). I2C bus 0 is scale-only, bus 1 is NFC and later IMU.
- Backend transport: outbound HTTPS only. No MQTT, no WireGuard, no Home Assistant
  entities in the data path.
- Attribution happens on the Brain, never in the backend. Unattributable volume goes
  to `devils_share`.
- Event journal and logs are separate. Journal never overwrites; logs are a ring
  buffer. Remote logging from warning level up; info level optional and RAM only.
- Daily read-only tokens are delivered as QR and may appear in the URL.
- Backend persistence: local files, `events.jsonl` append-only is the source of truth.

## Deferred - do not invent

- GPIO pin mapping (ADR-0009)
- Firmware toolchain and framework (ADR-0001)
- OTA signing and bundle update (ADR-0013)

Use a clearly marked placeholder referencing the ADR when needed.

## Architecture rules

- Brain is hexagonal: `domain` and `app` are hardware-free and host-testable; hardware
  and network only via `ports/` and `adapters/`.
- Protocol code lives in `firmware/shared/protocol` and is used by both nodes.
- Pour detection is an explicit state machine.
- Every event has `event_id`, `device_id`, `sequence`, timestamps, quality flags,
  `payload_version`. Delivery is at-least-once; consumers are idempotent.
- Never replace invalid sensor data with plausible defaults; raise an error state.
- Physical units in identifiers: `mass_g`, `volume_ml`, `timeout_s`.

## Measurement rules

- Pour volume = difference between two stable plateaus of the summed mass.
- Display debounce and booking settle window are separate parameters.
- Mass increase never produces a negative pour.
- No booking without valid per-cell calibration.
- Evaluate per-cell values for diagnostics (shift, jam, dead cell).

## Security rules

- Never commit secrets, tokens, real NFC UIDs, personal data or private URLs.
- Log token prefixes only.
- Device credentials are separate from user tokens. Display holds no backend secrets.
- No unauthenticated OTA, debug, config or admin endpoints.

## Python / AppDaemon

- Type hints, small modules, dependency injection, testable adapters.
- No business logic in HTTP handlers or callbacks.
- Validate payloads and schema versions. Atomic file writes (temp + rename).

## Testing

- Tests with every behavior change.
- Cover attribution, devil's share, timeout, restart during pour, duplicate events,
  journal recovery, UART CRC and resync, ACK retry, version mismatch, node loss.
- Prefer recorded per-cell weight traces as fixtures over timing-dependent tests.

## Documentation

- Update requirements and affected ADRs when behavior or boundaries change.
- When editing a `.drawio` file, re-export the matching image in the same commit.
- Mark unresolved topics as `deferred`, never as fact.
- Keep docs concise and technical. No filler.
