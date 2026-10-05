# Security und Datenschutz

## Zugriffsmodell Statistik

Token wird beim Check-in erzeugt, ist read-only und laeuft am Tagesende ab. Er steht in
der URL, da ein QR-Code die Information vollstaendig tragen muss.

Auflagen:

- Vollrefresh nach jeder QR-Anzeige (Ghosting haelt den Code sonst scanbar)
- Serverseitig nur Token-Praefix loggen
- `Referrer-Policy: no-referrer`, `Cache-Control: no-store`
- Rate Limiting je Token und IP
- Abgelaufene Tokens am Folgetag loeschen
- Token ohne Schreib- und Adminrechte

## Geraete und Transport

- Nur ausgehendes HTTPS vom Brain
- Geraeteauthentifizierung getrennt vom Nutzer-Token
- Display hat keinen Backend-Zugang und keine Credentials ausser Heim-WLAN
- Secrets in NVS bzw. Backend-Konfiguration, nie im Repository
- Logs ohne vollstaendige UIDs und Tokens

## OTA

- Beide Knoten per OTA, Zwei-Partitionen-Schema mit Rollback
- Display nur ueber Heimnetz
- Signaturpruefung auf dem Geraet ist das Sicherheitsmerkmal, nicht die Quelle
  (Backend oder GitHub Releases sind beide zulaessig, sobald signiert wird)
- Bis zur Signierung: OTA nur im bewusst aktivierten Wartungsmodus
- Details und Bundle-Logik: ADR-0013

## UART

Der UART ist physisch intern und unverschluesselt. Annahme: Wer am Wagen Kabel
umstecken kann, hat ohnehin physischen Zugriff. Die CRC dient der Integritaet gegen
Stoerungen, nicht gegen Manipulation.

## Datenschutz

- Pseudonyme statt Klarnamen in Events
- Aufbewahrungsfrist konfigurierbar
- Teilnahme freiwillig: ohne Check-in laeuft alles auf Devil's Share
- NFC-UID ist kopierbar und kein Sicherheitsmerkmal
