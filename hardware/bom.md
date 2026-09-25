# Bill of Materials

| Komponente | Auswahl | Status | Anmerkung |
|---|---|---|---|
| Controller und Display | TRMNL 7.5" OG DIY Kit, XIAO ESP32-S3 Plus auf EE04 | gesetzt | Boardrevision pruefen |
| Waegezellen | 4 x 50 kg Halbbruecke, Vollbruecke, 200 kg FS | gesetzt | siehe ADR-0002 |
| Waegewandler | HX711, Kanal A | gesetzt | nah an den Zellen platzieren |
| NFC | PN532 | gesetzt | Antennenlage neben Metall pruefen |
| Fasssockel | 3D-Druck, vier Krafteinleitungspunkte | zu konstruieren | Ueberlastanschlag vorsehen |
| Stromversorgung | Powerbank mit Always-On | zu beschaffen | siehe ADR-0008 |
| NFC-Tags | personalisiert je Nutzer | zu beschaffen | UID im Roster hinterlegen |
| Gehaeuse | spritzwassergeschuetzt, belueftet | offen | Kondensation beachten |
| IMU | spaeter | optional | Bewegungsflag fuer Messqualitaet |
| GNSS | spaeter | optional | ueber UART, nicht I2C |

Fuer jedes beschaffte Teil Datenblatt, Bezugsquelle, Revision und Betriebsspannung
ergaenzen.
