# ADR-0009: GPIO-Belegung

- Status: deferred

## Kontext

Die Belegung wird erst bei Beginn der Board-Programmierung festgelegt.

## Bekannte Vorbelastung

Die frueh notierte Zuordnung ist nicht uebernehmbar:

- GPIO5 war gleichzeitig als Taster und als I2C-Takt vorgesehen.
- Die Pins der Batteriemessung werden durch Weglassen der Batterie nicht automatisch
  elektrisch frei.

## Vorgehen bei der Entscheidung

1. Schaltplan und Boardrevision abgleichen
2. Durchgang und Vorbelastung am realen Board messen
3. Boot-Strap-Pins ausschliessen
4. Belegung mit Reserve festlegen und dokumentieren
5. Erst danach loeten

Das Ergebnis entscheidet zugleich ADR-0004.
