# Softwarekonzept

## Repositorystruktur, Zielbild

```text
firmware/
  brain/
    src/
      domain/          # Zustandsautomat, Attribution, Devil's Share, Mengen, Events
      app/             # Use Cases, Viewmodel-Erzeugung, Menuelogik
      ports/           # Interfaces: Sensor, Display, Journal, Backend
      adapters/
        sensors/       # NAU7802 ueber Mux, PN532
        display_link/  # UART-Protokoll
        journal/       # Outbox, Log-Ringpuffer
        backend/       # HTTPS-Client
      tasks/           # FreeRTOS-Tasks und Verdrahtung
    test/              # hostnahe Tests fuer domain und app
  display/
    src/
      render/          # Layout, QR, Refresh-Strategie
      link/            # UART-Protokoll, Watchdog
      input/           # Taster
    test/
  shared/
    protocol/          # Nachrichtenschema, CRC, Versionen - von beiden Knoten genutzt
appdaemon/
  apps/vatertap/
    domain/
    application/
    adapters/http/
    adapters/storage/
    web/
  tests/
docs/
hardware/
```

`shared/protocol` verhindert, dass Brain und Display das Protokoll unterschiedlich
implementieren.

## Firmware-Toolchain

Vertagt (ADR-0001). Begriffsklaerung: PlatformIO ist ein Build- und
Abhaengigkeitswerkzeug, kein Framework. Das Framework darunter ist Arduino-Core oder
ESP-IDF. Die Struktur oben funktioniert mit beiden.

## Persistenz Brain

| Bereich | Groesse | Verhalten bei voll |
|---|---|---|
| Event-Journal | eigene Partition, ca. 1 MB | `STORAGE_FULL`, E04, nie ueberschreiben |
| Log-Ringpuffer | ca. 64 KB im Flash | aelteste Eintraege werden ueberschrieben |
| Konfiguration, Roster, Kalibrierung | NVS | - |

Abschaetzung Journal: ca. 250 B je Event, ca. 300 Events pro Tag, also ca. 75 KB pro Tag.
1 MB reicht fuer mehr als 10 Tage offline. Bestaetigte Events werden segmentweise
geloescht.

Logs und Events liegen bewusst getrennt: Ein Fehlersturm darf nie Platz fuer
Zapfdaten belegen.

## Logging

- Remote: ab Warnung, Info-Level per Konfiguration zuschaltbar
- Deduplizierung: gleiche Meldung wird zu Code, Anzahl, erstem und letztem Zeitpunkt
- Info-Level nur im RAM gepuffert, nie im Flash
- Keine Unterscheidung zwischen Heim- und Mobilnetz
- Display-Logs laufen ueber den Heartbeat ins Brain und von dort ans Backend

## Persistenz Backend

| Datei | Inhalt |
|---|---|
| `events.jsonl` | append-only, Quelle der Wahrheit |
| `logs.jsonl` | Geraetelogs, rotiert |
| `roster.json` | Tag-UID zu Nutzer-ID |
| `sessions.json` | aktive Tagestoken |
| `state.json` | Aggregate, jederzeit rekonstruierbar |

Atomare Writes ueber temporaere Datei und Rename. Defekte Zeilen werden gezaehlt und
gemeldet.

## Standards

### C++

- Einheiten im Namen: `mass_g`, `volume_ml`, `timeout_s`
- Keine dynamische Allokation im Messpfad
- Fehler als expliziter Status, keine Ersatzwerte
- Hardware nur ueber `ports/`
- Formatierung per `clang-format`, statische Analyse in CI

### Python

- Typannotationen, `ruff`, `pytest`
- Keine Fachlogik in HTTP-Handlern
- Payload- und Schemavalidierung bei jedem Eingang

### Gemeinsam

- Kleine PRs, ADR-Bezug bei Architekturaenderung
- Tests fuer Zustandsuebergaenge, Attribution, Idempotenz, Neustart, Protokoll-CRC
- Konfiguration nie hart kodiert
