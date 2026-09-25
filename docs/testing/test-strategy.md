# Teststrategie

## Ebenen

- **Unit, hardwarefrei:** Zustandsautomat, Attribution, Devil's Share, Mengenberechnung,
  Idempotenz, Journalwiederherstellung
- **Aufgezeichnete Verlaeufe:** reale Gewichtskurven als Fixture, damit Zapferkennung
  ohne Hardware reproduzierbar testbar ist
- **Hardware-in-the-loop:** Waage, NFC, Display, Tastenlogik
- **System:** Geraet, Backend, Webansicht, Offline-Nachlieferung
- **Feld:** Veranstaltungstag mit Bewegung, Temperaturgang und schwachem Netz

## Pflichtszenarien

| Szenario | Erwartung |
|---|---|
| Zapfen ohne Check-in | Buchung auf Devil's Share |
| Check-in, dann Timeout, dann Zapfen | Devil's Share |
| Check-in B waehrend Sitzung A | A beendet, B aktiv |
| Sehr kurze Zapfung unter Mindestmenge | akkumuliert, nicht einzeln gebucht |
| Neustart waehrend `POURING` | kein Phantomereignis, kein Datenverlust |
| Doppelt gesendetes Ereignis | Backend zaehlt einmal |
| Fassnachfuellung ohne Bestaetigung | keine Buchung, Hinweis auf Display |
| Zug am Zapfschlauch | als instabil markiert, keine Fehlbuchung |
| Anstossen an den Sockel | keine Buchung |
| Netzausfall ueber Stunden | Journal laeuft, spaetere Nachlieferung vollstaendig |
| Powerbank-Abschaltung | definierter Neustart, Journal intakt |
| QR angezeigt, danach Display | kein scanbarer Geistercode |

## Messtechnische Abnahme

Vor dem ersten Einsatz zu protokollieren:

- Rauschband bei Ruhe ueber 10 Minuten
- Kriechverlauf nach Lastwechsel, daraus `settle_window_s` ableiten
- Drift ueber die geplante Einsatzdauer
- Ecklastfehler an vier Positionen
- Wiederholgenauigkeit einer 0,2-l-Referenzentnahme, mindestens 20 Wiederholungen
- Nachweis von NFR-001 und NFR-002 getrennt

Ergebnisse unter `hardware/validation/` ablegen.
