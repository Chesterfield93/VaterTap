# ADR-0004: Knoten-Topologie

- Status: deferred

## Kontext

Ein separater Sensorknoten wuerde die Umsetzung vereinfachen, da Waage und NFC nicht mit
dem Display um GPIOs und Bus-Timing konkurrieren.

## Architekturregel, falls aufgeteilt wird

Der Knoten mit den Sensoren besitzt die Fachlogik und fuehrt die Backend-Kommunikation.

| Knoten | Rolle |
|---|---|
| Sensorknoten | Waage, NFC, Sitzung, Zustandsautomat, Journal, HTTPS zum Backend |
| Displayknoten | reiner Renderer, zeigt ein fertiges Viewmodell an |

Kopplung ueber UART mit einem schlanken, versionierten Nachrichtenformat und
Bestaetigung. Kein zweites WLAN, kein ESP-NOW.

## Begruendung dieser Aufteilung

Laege das Journal beim Displayknoten, muesste jedes Ereignis eine zusaetzliche
Verbindung passieren, bevor es dauerhaft gespeichert ist. Damit entstuenden zwei
Fehlerquellen fuer dieselbe Wahrheit. Der Renderer darf ausfallen, ohne dass Messung
oder Buchung beeintraechtigt sind. Fehlt das Display, zeigt der Sensorknoten den Zustand
ueber Statusausgaben an.

## Offener Punkt

Der urspruengliche Treiber fuer die Aufteilung war GPIO-Knappheit durch den HX711. Ob
sie tatsaechlich noetig ist, entscheidet sich mit ADR-0009.

## Konsequenz fuer die Struktur

Die Firmware wird so geschnitten, dass `domain` und `app` unabhaengig von der
Knotenanzahl bleiben. Eine spaetere Aufteilung verschiebt dann nur Adapter, keine
Fachlogik.
