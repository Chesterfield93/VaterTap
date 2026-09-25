# GitHub Copilot Instructions for VaterTap

## Project context

VaterTap is an offline-capable IoT system for a mobile beer tap on a handcart. An
ESP32-S3 with an e-paper display reads a keg scale and an NFC reader, detects pours,
attributes them to users, and reports them to a Python backend running inside AppDaemon.

## Current phase

Concept phase. Do not generate production firmware or a complete backend unless an issue
explicitly asks for it. Prefer interfaces, tests, small spikes and documentation.

## Decided, do not silently change

- Transport is outbound HTTPS only. No MQTT, no WireGuard, no Home Assistant entities in
  the data path. AppDaemon is only a Python runtime.
- Attribution happens on the device, never in the backend.
- Unattributable volume is booked to the `devils_share` account.
- Persistence is local files. `events.jsonl` is append-only and is the source of truth.
- Measurement is gravimetric only. Flow meters are rejected.
- The scale carries the keg only, not the cart.
- Daily read-only tokens are delivered as a QR code and may appear in the URL.

## Deferred, do not invent

- GPIO pin mapping
- Firmware toolchain and framework
- One-node versus two-node topology

If code needs these, use a clearly marked placeholder and reference the ADR.

## Architecture rules

- `domain` and `app` layers must be hardware-free and testable on the host.
- Hardware libraries go behind project-owned interfaces.
- Pour detection is an explicit state machine, not scattered callbacks.
- Keep display refresh concerns out of domain logic.
- Every event carries `event_id`, `device_id`, `sequence`, timestamps, quality flags and
  `payload_version`.
- Delivery is at-least-once; consumers must be idempotent.
- Never replace invalid sensor data with plausible defaults. Raise an error state.
- Use physical units in identifiers, for example `mass_g`, `volume_ml`, `timeout_s`.

## Measurement rules

- A pour volume is always a difference between two stable plateaus, never an
  instantaneous reading.
- Keep display debounce and booking settle window as two separate parameters.
- A weight increase must never produce a negative pour.
- Do not book anything without valid calibration.

## Security rules

- Never commit secrets, tokens, real NFC UIDs, personal data or private URLs.
- Log only a token prefix, never the full token.
- Force a full display refresh after showing a QR code.
- Device credentials are separate from user tokens.
- No unauthenticated OTA, debug, config or admin endpoints.

## Python and AppDaemon

- Type hints, small modules, dependency injection, testable adapters.
- No business logic inside HTTP handlers or callbacks.
- Validate incoming payloads and schema versions.
- File writes must be atomic via temp file plus rename.

## Testing

- Add tests with every behavior change.
- Cover attribution, devil's share fallback, timeout, restart during pour, duplicate
  events and journal recovery.
- Prefer recorded weight traces as fixtures over timing-dependent tests.

## Documentation

- Update requirements and affected ADRs when behavior or boundaries change.
- Mark unresolved topics as `deferred`, never as fact.
- Keep documentation concise and technical. No filler, no repeated explanations.
