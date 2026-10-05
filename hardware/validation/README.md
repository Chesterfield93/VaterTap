# Hardware-Validierung

Je Experiment ein Markdown-Bericht plus Rohdaten (nicht personenbezogen).

Vor dem ersten Einsatz:

- `noise-baseline.md` - Rauschen je Zelle und Summe
- `creep-profile.md` - Kriechen, ergibt `settle_window_s`
- `drift-profile.md` - Drift ueber Einsatzdauer
- `corner-load.md` - Referenzgewicht an drei Positionen
- `hysteresis-guides.md` - Hysterese mit Fuehrungsbolzen, Grenze 20 g
- `repeatability-200ml.md` - 20 Entnahmen, Nachweis NFR-001
- `transport-zero.md` - Nullpunkt vor/nach Fahrt
- `i2c-bus-length.md` - Busstabilitaet Brain zu Sensorbox
- `power-budget.md` - Stromaufnahme beider Knoten, Powerbank

Inhalt: Aufbau, Verdrahtungsrevision, Firmware-Commit, Messmittel, Ablauf, Rohwerte,
Ergebnis, Folgeaktion.
