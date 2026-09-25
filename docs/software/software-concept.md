# Softwarekonzept

## Firmware, Herangehensweise

Kein Code in dieser Phase. Festgelegt ist nur die Struktur.

```text
firmware/
  src/
    app/             # Zustandsautomat, Sitzung, Use Cases
    domain/          # Ereignisse, Einheiten, Attribution, Validierung
    drivers/         # Waage, NFC, E-Paper, Taster
    infrastructure/  # HTTPS-Client, Journal, Zeit, Konfiguration, OTA
    ui/              # Viewmodel, Rendering, Fehlercodes, QR
  test/              # hostnahe Unit-Tests ohne Hardware
```

Kernidee: `domain` und `app` sind hardwarefrei und damit auf dem Entwicklungsrechner
testbar. Zapferkennung, Attribution und Devil's-Share-Logik lassen sich so gegen
aufgezeichnete Gewichtsverlaeufe pruefen, ohne Bier zu verbrauchen.

## Zur Frage, was PlatformIO ist

PlatformIO ist **kein** Firmware-Framework, sondern ein Build- und
Abhaengigkeitswerkzeug mit VS-Code-Integration. Es laedt Toolchain und Bibliotheken,
erzeugt reproduzierbare Builds und kennt Unit-Test-Targets.

Das Framework liegt eine Ebene darunter und ist die eigentliche Wahl:

| Ebene | Optionen |
|---|---|
| Werkzeug | PlatformIO, Arduino IDE, natives ESP-IDF-Build |
| Framework | Arduino-Core, ESP-IDF |
| Fertigloesung | ESPHome, ersetzt eigene Firmware weitgehend |

"Raw C++" ist keine Alternative dazu. C++ ist die Sprache; SDK, HAL und Buildsystem
werden trotzdem benoetigt.

Diskussion und Entscheidung erfolgen bei Beginn der Board-Programmierung.

## Backend, Herangehensweise

AppDaemon dient als Python-Laufzeitumgebung. Keine Home-Assistant-Entities im
Datenpfad.

```text
appdaemon/apps/vatertap/
  domain/          # Nutzer, Buchungen, Aggregation
  application/     # Use Cases
  adapters/
    http/          # Event-Aufnahme, Roster, Sessions
    storage/       # dateibasierte Persistenz
  web/             # read-only Statistikansicht
  tests/
```

## Persistenz, dateibasiert

Analog zum bekannten plex_porter-Ansatz:

| Datei | Inhalt | Eigenschaft |
|---|---|---|
| `events.jsonl` | Rohereignisse | append-only, Quelle der Wahrheit |
| `roster.json` | Tag-UID zu Nutzer-ID | nur lesend im Betrieb |
| `sessions.json` | aktive Tagestoken | taegliche Bereinigung |
| `state.json` | abgeleitete Aggregate | jederzeit neu berechenbar |

Regeln:

- Schreibvorgaenge atomar ueber temporaere Datei und Rename
- Rotation und Aufbewahrungsfrist konfigurierbar
- Aggregate sind Cache, nie Wahrheit. Bei Zweifel aus `events.jsonl` neu aufbauen.
- Defekte Zeilen werden uebersprungen und gezaehlt, nicht stillschweigend ignoriert

Dateibasiert ist bei dieser Datenmenge voellig ausreichend. Die einzige reale Gefahr
ist ein abgebrochener Schreibvorgang, und genau die adressiert das append-only-Format.

## Entwicklungsstandards

### C++

- Physikalische Einheiten im Namen, etwa `mass_g`, `volume_ml`, `timeout_s`
- Keine dynamische Allokation im Messpfad
- Hardwarebibliotheken hinter eigenen Interfaces
- Fehler als expliziter Status, keine stillen Ersatzwerte

### Python

- Typannotationen, `ruff`, `pytest`
- Fachlogik nicht in HTTP-Handlern
- Eingehende Payloads und Schemaversionen validieren

### Gemeinsam

- Kleine Pull Requests mit ADR-Bezug bei Architekturaenderungen
- Tests fuer Zustandsuebergaenge, Attribution, Idempotenz und Neustart
- Konfigurationswerte nie im Code hart kodieren
