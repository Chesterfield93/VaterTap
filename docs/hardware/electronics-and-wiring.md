# Elektronik und Verkabelung

## Komponenten

| Knoten | Komponenten |
|---|---|
| Brain | ESP32-S3-Board (Modell offen), 2x I2C, 1x UART |
| Display | XIAO ESP32-S3 Plus auf EE04, 7,5" E-Paper 800x480, Taster |
| Sensorbox | I2C-Multiplexer (TCA9548A oder PCA9546A), 3x NAU7802-Breakout |
| Waage | 3x Edelstahl-Doppelbolzen-Waegezelle, 30 kg Nennlast |
| NFC | PN532 an der Check-in-Position |
| Versorgung | Powerbank mit Always-On |

## Blockverdrahtung

```text
Powerbank 5 V
  +--> Brain (USB-C oder 5V-Pin)
  +--> Display/EE04 (USB-C)

Brain I2C 0 ===== 4-adrig (3V3, GND, SDA, SCL) =====> Sensorbox
                                                        Mux Kanal 0 -> NAU7802 #1 -> Zelle 1
                                                        Mux Kanal 1 -> NAU7802 #2 -> Zelle 2
                                                        Mux Kanal 2 -> NAU7802 #3 -> Zelle 3
Brain I2C 1 ===== 4-adrig =====> PN532   (spaeter zusaetzlich IMU)
Brain UART  ===== 3-adrig (TX, RX, GND) =====> Display (gekreuzt)
```

## Adresskonflikt NAU7802

Der NAU7802 hat die feste I2C-Adresse 0x2A. Drei Chips an einem Bus sind nur ueber
einen Multiplexer moeglich. Der Multiplexer schaltet je Lesezyklus einen Kanal frei.

Pruefen bei Beschaffung:

- Breakouts haben eigene Pull-ups. Pro Mux-Kanal ist das unkritisch, am Haupt-Bus
  genau ein Pull-up-Paar sicherstellen.
- Versorgungsspannung aller Breakouts = Logikpegel des Brain (3,3 V).
- DRDY-Pins: fuer v1 nicht verdrahtet, Polling ueber Statusregister genuegt.

## Sensorbox

- Unter dem Sockel, kurze Zellenkabel, Zellenkabel nicht verlaengern
- Zugentlastung fuer alle drei Zellenkabel und das Buskabel
- Spritzwassergeschuetzt, Kabelverschraubungen, belueftet gegen Kondensat
- Buskabel zum Brain verdrillt (SDA mit GND, SCL mit GND), so kurz wie moeglich
- Bei Buslaengen ueber ca. 1 m: Bustakt reduzieren oder I2C-Bus-Extender vorsehen

## UART Brain <-> Display

- TX/RX gekreuzt, gemeinsame Masse zwingend
- Beide 3,3-V-Pegel, kein Pegelwandler
- Steckverbinder verriegelnd

## Stromversorgung

Powerbanks schalten bei geringer Last oft nach ca. 30 s ab. Modell mit Always-On oder
definierte Grundlast vorsehen. Gemeinsame Masse beider Knoten ueber die Versorgung
sicherstellen, sonst ist der UART-Bezug undefiniert.

- Reale Stromaufnahme beider Knoten messen
- Spannungseinbruch bei WLAN- und Refresh-Spitzen pruefen
- Brownout-Erkennung aktiv, Journal gegen Abbruch gesichert

## Kalibrierung

- Nullpunkt mit leerem Sockel je Zelle
- Referenzgewicht nahe der Betriebslast, an drei Positionen
- Kalibrierfaktor je Zelle, nicht nur fuer die Summe
- Kriech- und Driftprotokoll ueber geplante Einsatzdauer
- Kalibrierdaten versioniert, im Diagnosemenue sichtbar

## Vertagt

GPIO-Belegung beider Knoten (ADR-0009). Durch den Split ist der Druck stark gesunken:
Das Display braucht nur SPI, Taster und UART; das Brain zwei I2C-Busse und einen UART.
