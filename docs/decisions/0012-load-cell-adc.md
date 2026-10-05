# ADR-0012: Waege-ADC und I2C-Topologie

- Status: accepted

## Entscheidung

- 3x NAU7802 (24 Bit, I2C), je Zelle ein ADC, 10 SPS
- I2C-Multiplexer (TCA9548A oder PCA9546A), da Adresse fest 0x2A
- Multiplexer und ADCs in einer Sensorbox unter dem Sockel
- I2C-Bus 0 exklusiv fuer die Waage, Bus 1 fuer NFC und spaeter IMU

## Begruendung gegen HX711

- Kein Bit-Banging, kein Power-Down durch Timing-Fehler
- Wert wird im Register gepuffert; Flash-Writes kosten keine Messwerte
- Interner LDO stabilisiert die Brueckenspeisung gegen Powerbank-Schwankungen

## Begruendung gegen Summierbox

Einzelkanaele ermoeglichen Kalibrierung je Zelle, Ecklastkorrektur, Erkennung von
Verrutschen, Klemmen und Zellendefekt.

## Verworfen

- Summierbox mit einem ADC: keine Diagnose, Ecklastfehler bei Billigzellen
- Zweiter Kanal je NAU7802: Kanalumschaltung halbiert Rate, kostet Einschwingzeit
- ADS1220: messtechnisch besser, aber die Zellen begrenzen, nicht der ADC
