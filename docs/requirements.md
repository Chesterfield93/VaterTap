# Anforderungen

## Funktionale Anforderungen

| ID | Anforderung | Abnahmekriterium |
|---|---|---|
| FR-001 | Fassrestmenge erfassen | Kalibrierte Masse und abgeleitetes Restvolumen werden angezeigt. |
| FR-002 | NFC-Check-in | Aufgelegter registrierter Tag startet eine Nutzersitzung. |
| FR-003 | Check-out | Neuer Check-in oder Inaktivitaets-Timeout beendet die Sitzung. |
| FR-004 | Zapfmenge zuordnen | Waehrend einer aktiven Sitzung erkannte Entnahme wird dem Nutzer gebucht. |
| FR-005 | Devil's Share | Jede Entnahme ohne aktive Sitzung wird dem Sammelkonto gebucht. |
| FR-006 | Lokale Attribution | Zuordnung erfolgt auf dem Geraet, auch ohne Backend-Verbindung. |
| FR-007 | Offline-Journal | Ereignisse werden lokal gepuffert und idempotent nachgeliefert. |
| FR-008 | Tagestoken | Beim Check-in wird ein read-only Token mit Tagesgueltigkeit erzeugt. |
| FR-009 | QR-Anzeige | Nach dem Zapfen wird der persoenliche Statistik-Link als QR angezeigt. |
| FR-010 | Fasswechsel | Gewichtszunahme wird erst nach Tastenbestaetigung als Fasswechsel verbucht. |
| FR-011 | Fehleranzeige | Sensor-, Speicher- und Netzfehler erscheinen als Fehlercode auf dem E-Paper. |
| FR-012 | Menuefunktion | Restliche Taster bieten Tara, Fasswechsel, Diagnose und Devil's-Share-Korrektur. |

## Qualitaetsanforderungen

| ID | Anforderung | Zielwert |
|---|---|---|
| NFR-001 | Portionsaufloesung, differenziell | +/- 50 g ueber ein Zapffenster von ca. 10 s |
| NFR-002 | Restmengenangabe, absolut | +/- 300 g ueber einen Betriebstag |
| NFR-003 | Check-out-Timeout | Standard 2 s, parametrierbar |
| NFR-004 | Buchungslatenz | UI-Reaktion < 2 s, finale Buchung nach Settle-Fenster |
| NFR-005 | Neustartsicherheit | Bestaetigte Ereignisse gehen weder verloren noch werden sie doppelt gezaehlt. |
| NFR-006 | Fail-safe Sensorik | Sensorfehler erzeugen keine plausiblen Fantasiewerte, sondern Fehlerzustaende. |
| NFR-007 | Datensparsamkeit | Zugriff auf persoenliche Daten nur ueber tagesgueltiges Token. |

## Bewusste Trennung von NFR-001 und NFR-002

Die eingesetzten Waegezellen driften und kriechen. Diese Fehler wirken **langsam** und
damit fast vollstaendig auf den Absolutwert. Eine Zapfung ist dagegen eine Differenz
ueber wenige Sekunden, in denen Drift vernachlaessigbar bleibt. Deshalb ist die
Portionsgenauigkeit strenger spezifiziert als die Restmengengenauigkeit. Ein einzelner
gemeinsamer Zielwert waere technisch nicht haltbar.

## Noch zu quantifizieren

- Fassgroesse, Leergewicht und Sockelmasse
- Settle-Fenster bis zur finalen Buchung, empirisch
- Maximale Offline-Dauer und Journalgroesse
- Temperaturbereich und erwartete Drift ueber einen Veranstaltungstag
