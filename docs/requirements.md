# Anforderungen

## Funktional

| ID | Anforderung | Abnahmekriterium |
|---|---|---|
| FR-001 | Fassrestmenge erfassen | Kalibrierte Summenmasse und Restvolumen werden angezeigt. |
| FR-002 | NFC-Check-in | Registrierter Tag startet eine Nutzersitzung. |
| FR-003 | Check-out | Neuer Check-in oder Inaktivitaet (`checkout_timeout_s`) beendet die Sitzung. |
| FR-004 | Zapfmenge zuordnen | Entnahme waehrend aktiver Sitzung wird dem Nutzer gebucht. |
| FR-005 | Devil's Share | Entnahme ohne aktive Sitzung wird dem Sammelkonto gebucht. |
| FR-006 | Lokale Attribution | Zuordnung erfolgt im Brain, auch ohne Backend. |
| FR-007 | Offline-Journal | Events werden lokal gepuffert und idempotent nachgeliefert. |
| FR-008 | Tagestoken | Check-in erzeugt einen read-only Token mit Tagesgueltigkeit. |
| FR-009 | QR-Anzeige | Nach dem Zapfen wird der Statistik-Link als QR angezeigt. |
| FR-010 | Fasswechsel | Gewichtszunahme wird nur nach Tastenbestaetigung als Fasswechsel gebucht. |
| FR-011 | Fehleranzeige | Fehler erscheinen als Code plus Klartext auf dem E-Paper. |
| FR-012 | Menue | Taster bieten Ansicht, Tara, Fasswechsel, Diagnose, Devil's-Share-Korrektur. |
| FR-013 | Knotenueberwachung | Brain und Display erkennen gegenseitigen Ausfall und melden ihn. |
| FR-014 | Zellendiagnose | Einzelwerte der drei Zellen sind sichtbar; Auffaelligkeiten werden gemeldet. |
| FR-015 | Remote-Logs | Warnungen und Fehler werden ans Backend gemeldet; Info-Level optional. |
| FR-016 | OTA | Brain und Display sind per OTA aktualisierbar. |

## Qualitaet

| ID | Anforderung | Zielwert |
|---|---|---|
| NFR-001 | Portionsgenauigkeit, differenziell | +/- 50 g ueber ein Zapffenster von ca. 10 s |
| NFR-002 | Restmenge, absolut | +/- 300 g ueber einen Betriebstag |
| NFR-003 | Check-out-Timeout | Standard 2 s, parametrierbar |
| NFR-004 | Reaktionszeit Anzeige | < 2 s nach Zapfstopp |
| NFR-005 | Neustartsicherheit | Bestaetigte Events weder verloren noch doppelt |
| NFR-006 | Fail-safe Sensorik | Sensorfehler fuehren zu Fehlerzustand, nie zu Ersatzwerten |
| NFR-007 | Datensparsamkeit | Zugriff auf persoenliche Daten nur per Tagestoken |
| NFR-008 | Ausfallerkennung | Knotenausfall nach spaetestens 3 s angezeigt |
| NFR-009 | Offline-Kapazitaet | Event-Journal fuer mindestens 3 Betriebstage |

## Warum NFR-001 und NFR-002 getrennt sind

Drift und Kriechen der Zellen wirken langsam und damit auf den Absolutwert. Eine
Zapfung ist eine Differenz ueber wenige Sekunden, in denen Drift vernachlaessigbar
bleibt. Ein gemeinsamer Zielwert waere physikalisch nicht haltbar.

## Noch zu quantifizieren

- Leergewicht der verwendeten Fasstypen und Sockelmasse (wiegen)
- `settle_window_s` aus dem Kriechprotokoll
- Temperaturbereich ueber einen Veranstaltungstag
