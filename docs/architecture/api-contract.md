# API-Vertrag, konzeptionell

Transport ist ausschliesslich ausgehendes HTTPS. Basis-Pfad `/vatertap/api/v1`.

## Endpunkte

| Zweck | Methode | Pfad | Anmerkung |
|---|---|---|---|
| Ereignisse liefern | POST | `/events` | Batch, idempotent ueber `event_id` |
| Roster laden | GET | `/roster` | Tag-UID zu Nutzer-ID, mit ETag |
| Konfiguration laden | GET | `/config` | Schwellen, Timeouts, Kalibrierhinweise |
| Tagestoken anfordern | POST | `/sessions` | liefert Token und Gueltigkeit |
| Gesundheitsmeldung | POST | `/health` | Firmwarestand, Journalfuellstand, Fehler |
| Persoenliche Statistik | GET | `/me` | read-only, Token-authentifiziert |

## Regeln

- Geraeteauthentifizierung ueber geraetespezifisches Secret oder Zertifikat, getrennt
  vom Nutzer-Token.
- Zustellung ist at-least-once. Das Backend muss doppelte `event_id` folgenlos
  verwerfen.
- Ein Ereignis darf erst nach bestaetigter Uebernahme aus dem Sendepuffer entfernt
  werden.
- Das Backend veraendert keine Attribution.
- Breaking Changes nur ueber neue Pfadversion.

## Offline-Verhalten

- Ohne Netz laeuft die Messung vollstaendig weiter.
- Der Roster-Cache bleibt gueltig; abgelaufene Roster erzeugen eine Warnung, aber
  keinen Betriebsstopp.
- Kann kein Tagestoken erzeugt werden, wird gezapft und gebucht, aber kein QR
  angezeigt. Die Statistik ist spaeter erreichbar.
