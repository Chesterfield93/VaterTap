# Security und Datenschutz

## Zugriffsmodell

Der Zugriff auf persoenliche Statistiken erfolgt ueber ein Token, das beim NFC-Check-in
erzeugt wird und am Tagesende ablaeuft. Der Token wird als QR-Code auf dem E-Paper
angezeigt und liegt damit in der URL.

Bewertung: fuer diesen Anwendungsfall vertretbar.

- read-only, keine schreibenden Operationen
- Gueltigkeit auf einen Tag begrenzt
- Erzeugung erfordert physischen Besitz des personalisierten Tags
- Offenlegung betrifft ausschliesslich die eigene Trinkstatistik

Auflagen:

- Nach jeder QR-Anzeige ein Vollrefresh des Displays, da Ghosting den Code weiterhin
  scanbar halten kann.
- Serverseitig nur ein Token-Praefix protokollieren, nie den vollstaendigen Token.
- `Referrer-Policy: no-referrer` und `Cache-Control: no-store` auf der Statistikseite.
- Rate Limiting je Token und je IP.
- Tokens am Folgetag serverseitig loeschen, nicht nur als abgelaufen markieren.
- Token traegt keinerlei Adminrechte; Konfiguration laeuft ueber einen getrennten Pfad.

## Weitere Vorgaben

- Ausschliesslich ausgehende TLS-Verbindungen vom Geraet.
- Geraeteauthentifizierung getrennt von Nutzer-Tokens.
- Secrets in NVS beziehungsweise Backend-Konfiguration, niemals im Repository.
- Kein unauthentifizierter OTA-, Debug- oder Konfigurationsendpunkt.
- OTA nur im bewussten Wartungsmodus, moeglichst signiert, mit Rollback.
- NFC-UID ist kein Sicherheitsmerkmal. Sie ist kopierbar und dient nur der Zuordnung.
- Logs ohne vollstaendige UIDs und ohne vollstaendige Tokens.

## Datenschutz

Die kurze Token-Lebensdauer begrenzt den Zugriff, nicht die Speicherung. Zusaetzlich:

- Ereignisse speichern Pseudonyme, nicht Klarnamen.
- Aufbewahrungsfrist fuer Rohereignisse konfigurierbar.
- Teilnahme bleibt freiwillig; ohne Check-in laeuft alles auf Devil's Share, und das
  System funktioniert weiterhin vollstaendig.
