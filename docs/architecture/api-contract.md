# Backend-API, konzeptionell

Ausschliesslich ausgehendes HTTPS vom Brain. Basis `/vatertap/api/v1`.

| Zweck | Methode | Pfad | Anmerkung |
|---|---|---|---|
| Events | POST | `/events` | Batch, idempotent ueber `event_id` |
| Logs | POST | `/logs` | Warnung und Fehler, dedupliziert |
| Health | POST | `/health` | Firmware beider Knoten, Journal, Displaystatus |
| Roster | GET | `/roster` | Tag-UID zu Nutzer-ID, mit ETag |
| Konfiguration | GET | `/config` | Schwellen, Timeouts, Log-Level |
| Tagestoken | POST | `/sessions` | Token plus Gueltigkeit |
| Statistik | GET | `/me` | read-only, Token in URL |

## Regeln

- Geraeteauthentifizierung getrennt vom Nutzer-Token.
- At-least-once; doppelte `event_id` wird folgenlos verworfen.
- Brain loescht ein Event erst nach bestaetigter Uebernahme.
- Backend veraendert keine Attribution.
- Sync-Reihenfolge: Events vor Logs vor Health.
- Breaking Changes nur ueber neue Pfadversion.

## Offline

- Messung und Buchung laufen weiter.
- Abgelaufener Roster-Cache erzeugt Warnung, keinen Stopp.
- Ohne Tagestoken wird gebucht, aber kein QR angezeigt.
