# Teststrategie

## Ebenen

- **Unit, Host:** Zustandsautomat, Attribution, Devil's Share, Plateauberechnung,
  Zellendiagnose, Protokoll-Parser und CRC, Idempotenz
- **Aufgezeichnete Verlaeufe:** reale Massekurven je Zelle als Fixtures
- **Hardware-in-the-loop:** Sensorbox, NFC, UART-Strecke, Display
- **System:** Brain, Display, Backend, Offline-Nachlieferung
- **Feld:** Bewegung, Temperatur, schwaches Netz, Stoesse

## Pflichtszenarien

| Szenario | Erwartung |
|---|---|
| Zapfen ohne Check-in | Devil's Share |
| Check-in, Timeout, Zapfen | Devil's Share |
| Check-in B waehrend Sitzung A | A endet, B aktiv |
| Zapfung unter Mindestmenge | akkumuliert |
| Neustart Brain waehrend `POURING` | kein Phantomevent, kein Verlust |
| Doppelt gesendetes Event | Backend zaehlt einmal |
| Fass nachgefuellt ohne Bestaetigung | keine Buchung, Hinweis |
| Fass seitlich verschoben | Summe konstant, ggf. E08, keine Buchung |
| Eine Zelle abgezogen | E01 oder E08, keine Buchung |
| Schlag auf den Sockel | keine Buchung |
| Display stromlos | Messung und Buchung laufen weiter, Brain loggt Fehler |
| Brain stromlos | Display zeigt E09 nach spaetestens 3 s |
| UART-Stoerung, kaputte Zeilen | verworfen, gezaehlt, Resync |
| Tastendruck, ACK geht verloren | Retry, Brain verarbeitet genau einmal |
| Versionen Brain/Display inkompatibel | E10 |
| Fehlersturm im Log | Journal unberuehrt, Logs dedupliziert |
| Netzausfall ueber Stunden | Journal laeuft, vollstaendige Nachlieferung |
| QR angezeigt | danach kein scanbarer Geistercode |

## Messtechnische Abnahme

Protokolle unter `hardware/validation/`:

- Rauschband je Zelle und Summe, 10 min Ruhe
- Kriechverlauf nach Lastwechsel, ergibt `settle_window_s`
- Drift ueber Einsatzdauer
- Ecklast: Referenzgewicht an drei Positionen
- Hysterese mit Fuehrungsbolzen, Grenzwert 20 g
- Wiederholgenauigkeit 200 ml, mindestens 20 Entnahmen (NFR-001)
- Nullpunkt vor und nach Transportfahrt
- Strombudget beider Knoten, Powerbank-Verhalten
