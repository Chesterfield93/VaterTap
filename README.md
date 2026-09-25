# VaterTap

Smartes, mobiles Zapfanlagen-System auf Basis eines ESP32-S3-E-Paper-Boards.

## Zielbild

- Restmenge des Fasses ueber eine Fasswaage erfassen
- Nutzer per NFC-Check-in identifizieren und Zapfmengen zuordnen
- Nicht zuordenbare Mengen auf einen Sammelnutzer "Devil's Share" buchen
- Status, Fehler und persoenlicher QR-Code lokal auf E-Paper anzeigen
- Persoenliche Statistik ueber eine tagesgueltige, read-only Webansicht
- Ohne Backend-Verbindung vollstaendig weiter messen und zuordnen

## Projektstatus

**Konzeptphase.** Transport, Messstrategie, Attribution, Persistenz und Zugriffsmodell
sind entschieden. GPIO-Belegung, Firmware-Toolchain und Knoten-Topologie sind bewusst
vertagt, bis die Board-Programmierung beginnt.

## Dokumentation

- [Anforderungen](docs/requirements.md)
- [Systemarchitektur](docs/architecture/system-architecture.md)
- [Attribution und Ereignisse](docs/architecture/attribution-and-events.md)
- [API-Vertrag](docs/architecture/api-contract.md)
- [Bedienung und Anzeige](docs/architecture/ui-concept.md)
- [Elektronik und Verkabelung](docs/hardware/electronics-and-wiring.md)
- [Softwarekonzept](docs/software/software-concept.md)
- [Security und Datenschutz](docs/security/security-and-privacy.md)
- [Teststrategie](docs/testing/test-strategy.md)
- [Offene und getroffene Entscheidungen](docs/decisions/README.md)
- [Risiken](docs/risks.md)
- [Beschaffung](hardware/bom.md)

## Leitplanken

1. Der Knoten, der misst, besitzt auch die Zuordnung. Keine Attribution im Backend.
2. Jede gezapfte Menge landet auf genau einem Konto. Im Zweifel auf Devil's Share.
3. Kein Zapfvorgang darf durch Netzausfall oder Neustart verloren gehen.
4. Rohwert, abgeleitete Menge und Qualitaetsbewertung bleiben getrennt gespeichert.
5. Keine Secrets, echten NFC-UIDs oder personenbezogenen Exporte im Repository.
6. Hardwareannahmen werden erst nach Bench-Test zu Entscheidungen.
