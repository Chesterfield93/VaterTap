# ADR-0006: Tagestoken und QR-Zugriff

- Status: accepted

## Entscheidung

Beim NFC-Check-in wird ein read-only Token mit Tagesgueltigkeit erzeugt. Nach dem Zapfen
wird der persoenliche Statistik-Link als QR-Code auf dem E-Paper angezeigt. Der Token
steht dabei in der URL.

## Begruendung

Ein QR-Code muss die gesamte Information selbst tragen; Header oder Cookies sind vor dem
ersten Aufruf nicht setzbar. Das Restrisiko ist eng begrenzt: nur lesender Zugriff,
Gueltigkeit ein Tag, Erzeugung nur mit physischem Besitz des personalisierten Tags, und
betroffen ist ausschliesslich die eigene Trinkstatistik.

## Auflagen

- Vollrefresh nach jeder QR-Anzeige, da E-Paper-Ghosting den Code scanbar halten kann.
- Serverseitig nur Token-Praefix protokollieren.
- `Referrer-Policy: no-referrer`, `Cache-Control: no-store`.
- Rate Limiting je Token.
- Abgelaufene Tokens taeglich loeschen.
- Token gewaehrt keine Admin- oder Schreibrechte.

## Konsequenz ohne Netz

Kann kein Token erzeugt werden, laufen Messung und Buchung normal weiter. Es entfaellt
lediglich die QR-Anzeige.
