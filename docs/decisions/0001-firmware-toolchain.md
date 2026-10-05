# ADR-0001: Firmware-Toolchain

- Status: deferred

## Kontext

Entscheidung bei Beginn der Board-Programmierung. Gilt jetzt fuer zwei Firmwares
(Brain, Display) plus gemeinsames Protokollmodul.

## Begriffsklaerung

PlatformIO ist Build- und Abhaengigkeitswerkzeug. Framework ist Arduino-Core oder
ESP-IDF. ESPHome ersetzt eigene Firmware weitgehend und scheidet fuer das Brain aus.

## Optionen

1. PlatformIO + Arduino-Core
2. PlatformIO + ESP-IDF
3. Natives ESP-IDF

## Kriterien

Treiber fuer NAU7802, PN532, E-Paper; FreeRTOS-Kontrolle (Task-Modell, ADR-0011);
OTA mit Rollback und spaeterer Signierung; Host-Tests; ein Build fuer zwei Targets
mit gemeinsamem Modul.
