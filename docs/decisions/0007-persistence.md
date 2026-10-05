# ADR-0007: Dateibasierte Persistenz Backend

- Status: accepted

## Entscheidung

Lokale Dateien analog plex_porter: `events.jsonl` append-only als Quelle der Wahrheit,
`logs.jsonl` rotiert, `roster.json`, `sessions.json`, `state.json` als Cache.

## Regeln

Atomare Writes (temp + rename), Rotation konfigurierbar, defekte Zeilen zaehlen und
melden, Aggregate jederzeit aus Events rekonstruierbar.
