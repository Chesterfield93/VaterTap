# ADR-0004: Knoten-Topologie Brain/Display

- Status: accepted

## Entscheidung

Zwei ESP32-S3-Knoten:

| Knoten | Rolle |
|---|---|
| **Brain** | Sensorik, Domain, Zustand, Journal, Backend-Sync, Display-Ueberwachung |
| **Display** (XIAO auf EE04) | Renderer und Tasteneingabe |

Kopplung per UART (ADR-0010).

## Begruendung

- Trennt Fachlogik von UI, ein vollwertiger ESP32 fuer Logik und Connectivity
- Loest GPIO-Knappheit des EE04
- Displayausfall beeintraechtigt Messung und Buchung nicht
- Journal und Backend-Sync liegen beim Messknoten, keine zweite Fehlerquelle fuer
  dieselbe Wahrheit

## Konsequenzen

- Zwei Firmwares, gemeinsames Protokollmodul
- Versionskompatibilitaet muss geprueft werden
- Taster liegen am Display, ihre Bedeutung im Brain
- Gemeinsame Masse beider Knoten zwingend
