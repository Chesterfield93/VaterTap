# ADR-0009: GPIO-Belegung

- Status: deferred

## Kontext

Festlegung bei Beginn der Board-Programmierung, je Knoten.

| Knoten | Bedarf |
|---|---|
| Brain | 2x I2C (4 Pins), 1x UART (2 Pins), Reserve |
| Display | SPI zum EE04 (vorgegeben), Taster, 1x UART |

## Bekannte Vorbelastung Display

Fruehe Notizen sahen GPIO5 doppelt vor; Batteriemesspfade werden durch Weglassen der
Batterie nicht automatisch frei.

## Vorgehen

Schaltplan pruefen, am Board messen, Boot-Strap-Pins ausschliessen, mit Reserve
dokumentieren, dann loeten.
