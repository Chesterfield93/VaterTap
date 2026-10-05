# ADR-0006: Tagestoken und QR

- Status: accepted

## Entscheidung

Read-only Token, erzeugt beim Check-in, Tagesgueltigkeit, als QR in der URL.

## Begruendung

QR muss alle Information selbst tragen. Risiko begrenzt: nur lesend, ein Tag, nur
eigene Statistik, Erzeugung nur mit physischem Tag.

## Auflagen

Vollrefresh nach QR, Token-Praefix-Logging, `no-referrer`, `no-store`, Rate Limit,
taegliche Loeschung. Ohne Netz: Buchung ja, QR nein.
