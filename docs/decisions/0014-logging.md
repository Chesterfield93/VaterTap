# ADR-0014: Logging und Journal-Trennung

- Status: accepted

## Entscheidung

- Event-Journal und Logs getrennt
- Journal: eigene Partition ca. 1 MB, nie ueberschreiben, voll fuehrt zu E04
- Logs: Ringpuffer ca. 64 KB im Flash, aelteste zuerst ueberschrieben
- Remote ab Warnung, Info per Konfiguration; Info nur im RAM
- Deduplizierung gleicher Meldungen
- Keine Unterscheidung Heim-/Mobilnetz
- Sync-Prioritaet: Events vor Logs

## Begruendung

Ein Fehlersturm darf keinen Speicher fuer Zapfdaten belegen. Flash-Ringpuffer
uebersteht Neustarts und liefert Diagnose nach Brownout.
