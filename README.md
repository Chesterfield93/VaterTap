# VaterTap

Smartes, mobiles Zapfanlagen-System auf einem Bollerwagen.

## Zielbild

- Restmenge des Fasses ueber eine 3-Punkt-Fasswaage erfassen
- Nutzer per NFC-Check-in identifizieren und Zapfmengen lokal zuordnen
- Nicht zuordenbare Mengen auf das Sammelkonto "Devil's Share" buchen
- Status, Fehler und persoenlichen QR-Code auf E-Paper anzeigen
- Persoenliche Statistik ueber tagesgueltigen, read-only Link
- Ohne Backend-Verbindung vollstaendig weiter messen und zuordnen

## Systemueberblick

| Teil | Rolle |
|---|---|
| **Brain** (ESP32-S3) | Sensorik, Zustandsautomat, Attribution, Journal, HTTPS zum Backend |
| **Display** (XIAO ESP32-S3 auf EE04) | reiner Renderer und Tasteneingabe, per UART am Brain |
| **Sensorbox** | I2C-Multiplexer + 3x NAU7802 unter dem Fasssockel |
| **Backend** (AppDaemon) | Event-Annahme, Aggregation, Webansicht, dateibasierte Persistenz |

## Projektstatus

**Konzeptphase.** Topologie, Transport, Messkette, Attribution, Persistenz,
Knotenprotokoll und Task-Modell sind entschieden. Offen: GPIO-Belegung, Firmware-
Toolchain, OTA-Signierung.

## Dokumentation

- [Anforderungen](docs/requirements.md)
- [Systemarchitektur](docs/architecture/system-architecture.md)
- [Brain: Task-Modell](docs/architecture/brain-task-model.md)
- [Knotenprotokoll Brain/Display](docs/architecture/node-protocol.md)
- [Attribution und Ereignisse](docs/architecture/attribution-and-events.md)
- [Backend-API](docs/architecture/api-contract.md)
- [Bedienung und Anzeige](docs/architecture/ui-concept.md)
- [Elektronik und Verkabelung](docs/hardware/electronics-and-wiring.md)
- [Waagenmechanik](docs/hardware/scale-mechanics.md)
- [Softwarekonzept](docs/software/software-concept.md)
- [Security und Datenschutz](docs/security/security-and-privacy.md)
- [Teststrategie](docs/testing/test-strategy.md)
- [Entscheidungen (ADR)](docs/decisions/README.md)
- [Risiken](docs/risks.md)
- [Beschaffung](hardware/bom.md)

## Leitplanken

1. Das Brain besitzt Messung, Zuordnung und Zustand. Display und Backend sind Adapter.
2. Jede gezapfte Menge landet auf genau einem Konto, im Zweifel auf Devil's Share.
3. Kein Zapfvorgang darf durch Netzausfall, Displayausfall oder Neustart verloren gehen.
4. Rohwert, abgeleitete Menge und Qualitaetsbewertung bleiben getrennt.
5. Keine Secrets, echten NFC-UIDs oder personenbezogenen Exporte im Repository.
6. Hardwareannahmen werden erst nach Bench-Test zu Entscheidungen.
