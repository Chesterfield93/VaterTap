# Systemarchitektur

## Zielbild

```text
ON-VEHICLE
                                      +---------------------------+
 [Waegezelle x3] -analog-> [Sensorbox] -I2C Bus 0-> |                           |
                    (TCA/PCA-Mux + 3x NAU7802)       |  BRAIN  (ESP32-S3)        |
 [NFC PN532] ---------------I2C Bus 1-------------> |  Fachlogik, Zustand,      | -- HTTPS -->  BACKEND
 [IMU, spaeter] ------------I2C Bus 1-------------> |  Journal, Backend-Sync    |
                                                    +---------------------------+
                                                         ^            |
                                    Tasten-Events (ACK)  |  UART      | Viewmodel 1 Hz
                                                         |            v
                                                    +---------------------------+
 [Taster] ----GPIO----------------------------------> |  DISPLAY (XIAO auf EE04)  |
 [E-Paper 7,5"] <---SPI/FPC------------------------- |  Renderer, QR, Watchdog   | -- WLAN nur OTA, zuhause
                                                    +---------------------------+

BACKEND
 [Reverse Proxy /vatertap/api/v1] -> [AppDaemon als Python-Laufzeit] -> [lokale Dateien]
                                               ^
                        [Smartphone] -- HTTPS, Tagestoken aus QR
```

Ein grafisches Diagramm folgt unter `diagrams/`.

## Knoten und Verantwortung

### Brain

Einziger Besitzer von Messung, Fachzustand und Backend-Kommunikation.

- Drei Zellen einzeln einlesen, summieren, plausibilisieren
- Zapfvorgang per Zustandsautomat erkennen
- Nutzersitzung fuehren und lokal attribuieren
- Events ins Journal schreiben (Outbox), per HTTPS nachliefern
- Viewmodel fuer das Display erzeugen, Tastenereignisse interpretieren
- Display ueberwachen, Warnungen und Fehler ans Backend melden
- Roster, Konfiguration und Tagestoken cachen

Aufbau intern: siehe [Task-Modell](brain-task-model.md).

### Display

Reine Darstellung und Eingabe, keine Fachlogik.

- Viewmodel empfangen und rendern, inklusive QR-Erzeugung
- Teil- oder Vollrefresh selbst waehlen, Vollrefresh nach QR erzwingen
- Rohe Tastenereignisse an das Brain melden
- Brain-Ausfall erkennen und anzeigen
- WLAN ausschliesslich fuer OTA im Heimnetz

Faellt das Display aus, laufen Messung und Buchung unveraendert weiter.

### Sensorbox

Eine gemeinsame Box unter dem Fasssockel mit Multiplexer und drei NAU7802. Analoge
Leitungen bleiben kurz; zum Brain fuehrt nur ein digitales I2C-Kabel. Der NFC-Leser
sitzt nicht in der Box, sondern an der Check-in-Position.

### Backend

AppDaemon dient nur als Python-Laufzeit. Keine Home-Assistant-Entities im Datenpfad.

- Events idempotent annehmen und dateibasiert ablegen
- Aggregation je Nutzer, Tag und Fass; keine Aenderung der Attribution
- Read-only Webansicht per Tagestoken
- Roster und Konfiguration ausliefern, Health und Logs annehmen

## Schnittstellen

| Von | Nach | Medium | Inhalt |
|---|---|---|---|
| Waegezellen | NAU7802 | analog, je Zelle eigene Bruecke | Brueckenspannung |
| Sensorbox | Brain | I2C Bus 0 ueber Multiplexer | Rohwerte je Zelle |
| PN532 | Brain | I2C Bus 1 | Tag-UID |
| Brain | Display | UART, NDJSON + CRC | Viewmodel, zyklisch |
| Display | Brain | UART, NDJSON + CRC | Tasten-Events, Heartbeat |
| Brain | Backend | HTTPS ausgehend | Events, Health, Logs, Roster, Config, Sessions |
| Smartphone | Backend | HTTPS | Statistik per Tagestoken |
| Brain, Display | OTA-Quelle | WLAN | Firmware-Images |

## Netze

| Knoten | WLAN-Liste | Zweck |
|---|---|---|
| Brain | Heimnetze + mobile Hotspots | Backend-Sync, OTA |
| Display | nur Heimnetze | ausschliesslich OTA |

## Architekturprinzipien

- Offline-first, idempotent, fail-safe
- Brain intern hexagonal: Domain im Zentrum, Sensorik, Display, Journal, Backend als Ports
- Zustand nur ueber Nachrichten teilen, keine geteilten Variablen
- Treiber hinter eigenen Interfaces
- Rohmessung, Ableitung und Praesentation strikt getrennt

## Offen

- GPIO-Belegung je Knoten (ADR-0009)
- Firmware-Toolchain (ADR-0001)
- OTA-Signierung und Bundle-Update (ADR-0013)
