# ADR-0001: Firmware-Toolchain

- Status: deferred

## Kontext

Die Wahl wurde bewusst vertagt, bis die Board-Programmierung beginnt. Zuvor ist die
Begriffsklaerung wichtiger als die Entscheidung.

## Begriffsklaerung

PlatformIO ist ein Build- und Abhaengigkeitswerkzeug, kein Framework. Das Framework ist
Arduino-Core oder ESP-IDF. ESPHome ist eine Fertigloesung, die eigene Firmware weitgehend
ersetzt.

## Optionen

1. PlatformIO mit Arduino-Core
2. PlatformIO mit ESP-IDF
3. Natives ESP-IDF-Build
4. ESPHome

## Vorlaeufige Einschaetzung

ESPHome duerfte an Event-Journal, Sitzungslogik, QR-Anzeige und Devil's Share scheitern.
Die uebrigen Optionen bleiben offen.

## Entscheidungskriterien

Treiberqualitaet fuer E-Paper, PN532 und HX711; hostseitige Unit-Tests; OTA und
Rollback; reproduzierbarer Build; Flash- und RAM-Bedarf.
