# ADR-0007: Dateibasierte Persistenz

- Status: accepted

## Entscheidung

Backend-Persistenz vollstaendig ueber lokale Dateien, analog zum bekannten
plex_porter-Ansatz. Keine Datenbank, keine Home-Assistant-Recorder-Abhaengigkeit.

## Format

- `events.jsonl` als append-only Quelle der Wahrheit
- `roster.json`, `sessions.json`, `state.json` als abgeleitete oder statische Dateien
- Aggregate sind jederzeit aus `events.jsonl` rekonstruierbar

## Regeln

- Atomare Schreibvorgaenge ueber temporaere Datei und Rename
- Rotation und Aufbewahrungsfrist konfigurierbar
- Defekte Zeilen werden gezaehlt und gemeldet, nicht still verworfen

## Begruendung

Die Datenmenge ist klein und der Zugriff nahezu ausschliesslich anhaengend. Das
append-only-Format ueberlebt abgebrochene Schreibvorgaenge, was bei Powerbank-Betrieb
der realistischste Fehlerfall ist.

## Konsequenz

Auswertungen ueber sehr lange Zeitraeume werden langsamer. Bei Bedarf kommt eine
Verdichtung je Veranstaltungstag hinzu, ohne die Rohdaten zu veraendern.
