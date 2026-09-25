# ADR-0005: Attribution und Devil's Share

- Status: accepted

## Entscheidung

Die Zuordnung von Zapfmengen zu Nutzern erfolgt **auf dem ESP32**, nicht im Backend.
Nicht zuordenbare Mengen werden auf das Sammelkonto `devils_share` gebucht.

## Sitzungsregeln

- NFC-Check-in startet die Messung und die Nutzersitzung.
- Ein neuer Check-in beendet die vorherige Sitzung automatisch.
- Ohne Entnahme endet die Sitzung nach `checkout_timeout_s`, Standard 2 s.
- Der Tag muss waehrend des Zapfens nicht aufliegen.

## Begruendung fuer lokale Attribution

Netzunabhaengigkeit, sofortige Anzeige und eine einzige Entscheidungsstelle. Das Backend
haette dieselbe Entscheidung spaeter und mit weniger Kontext treffen muessen. Es darf
Attribution daher nicht nachtraeglich veraendern.

Voraussetzung: lokaler Roster-Cache mit Tag-UID zu Nutzer-ID.

## Zwei Zeitkonstanten

`stop_debounce_s` steuert die Anzeige und bleibt kurz, damit der Ablauf fluessig bleibt.
`settle_window_s` steuert die Buchung und ist laenger, weil die Zellen unmittelbar nach
Lastwechsel kriechen. Ohne diese Trennung wuerde Kriechen als Biermenge gebucht.

## Devil's Share als Bilanzkontrolle

Summe der Nutzerbuchungen plus Devil's Share muss der Gesamtentnahme entsprechen. Ein
unerwartet schnell wachsender Devil's Share ist ein Frueherkennungsmerkmal fuer
Messfehler, nicht nur eine Spassfunktion.

## Konsequenzen

- Roster muss vor dem Einsatz aktuell sein.
- Unbekannte Tags erzeugen keine Sitzung.
- Korrekturfunktion im Menue fuer die letzte Devil's-Share-Position.
