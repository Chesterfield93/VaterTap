# ADR-0011: Task-Modell Brain

- Status: accepted

## Entscheidung

Sechs FreeRTOS-Tasks: `acquisition`, `domain` (Kern 1), `journal`, `link`, `sync`,
`supervisor` (Kern 0). Kommunikation nur ueber Queues; `domain` einziger Schreiber des
Fachzustands. Intern hexagonal mit Ports fuer Sensorik, Display, Journal, Backend.

## Begruendung

- Netz und TLS belasten Kern 0, Messung und Logik bleiben ungestoert
- Einzeln ueberwachbar und priorisierbar
- Klar abgegrenzte Module fuer Copilot-gestuetzte Umsetzung
- Kernzuordnung bleibt Deployment-Detail und ist aenderbar

## Verworfen

Zwei Grob-Tasks (Realtime/Comms): weniger Aufwand, aber schlechtere Diagnose und
unklarere Modulgrenzen.

Details: `docs/architecture/brain-task-model.md`.
