# ADR-0005: Attribution und Devil's Share

- Status: accepted

## Entscheidung

Zuordnung im Brain mit lokalem Roster-Cache. Nicht zuordenbare Mengen auf
`devils_share`.

## Sitzungsregeln

- Check-in startet Messung und Sitzung
- Neuer Check-in beendet die vorige Sitzung
- Timeout `checkout_timeout_s`, Standard 2 s
- Tag muss nicht aufliegen

## Zeitkonstanten

`stop_debounce_s` fuer die Anzeige, `settle_window_s` fuer die Buchung. Ohne Trennung
wird Zellenkriechen als Bier gebucht.

## Konsequenzen

- Backend darf Attribution nicht aendern
- Devil's Share dient als Bilanzkontrolle
- Korrekturfunktion fuer letzte Devil's-Share-Position
