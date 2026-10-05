# Brain: Task-Modell

## Grundsatz

Die Architektur schneidet nach Verantwortung (Module, Ports). Die Zuordnung zu Kernen
ist eine Deployment-Entscheidung und darf sich aendern, ohne Module umzubauen.

## Logische Struktur

```text
        Sensor-Port              Display-Port
   (3 Zellen, NFC, IMU)    (Viewmodel raus, Tasten rein)
               \                    /
                [      DOMAIN      ]   <- einziger Besitzer des Fachzustands
               /                    \
        Journal-Port             Backend-Port
     (Event-Outbox, Logs)   (Events, Health, Logs, Roster, Config)
```

Das Journal ist die Outbox: Die Domain schreibt ein Event und ist fertig. Der Sync
liest unabhaengig. Die Domain wartet nie auf das Netz.

## Tasks

| Task | Kern | Prioritaet | Takt | Aufgabe |
|---|---|---|---|---|
| `acquisition` | 1 | hoechste | 10 Hz Waage, ~5-10 Hz NFC | 3 Zellen ueber Mux lesen, Summe, Filter, Stabilitaet, NFC-Poll |
| `domain` | 1 | hoch | ereignisgetrieben | Zustandsautomat, Sitzung, Attribution, Viewmodel |
| `journal` | 0 | mittel | ereignisgetrieben | Events und Logs persistieren, Outbox fuehren |
| `link` | 0 | mittel | 1 Hz + bei Aenderung | UART zum Display, Watchdog |
| `sync` | 0 | niedrig | Outbox-getrieben | HTTPS: Events, Logs, Health, Roster, Config, Sessions |
| `supervisor` | 0 | niedrig | periodisch | Task-Watchdogs, Heap, Log-Deduplizierung |

## Regeln

- Kommunikation ausschliesslich ueber Queues. Keine geteilten Variablen mit Mutex.
- `domain` ist der einzige Schreiber des Fachzustands.
- Sensorschicht liefert normierte, zeitgestempelte Werte in Gramm plus generische
  Signalinfos (gefiltert, stabil). Die Interpretation (Zapfbeginn, Zapfstopp) liegt in
  `domain`.
- `domain` und `acquisition` blockieren nie auf I/O ausser ihrem eigenen Bus.
- Jeder Task meldet sich zyklisch beim `supervisor`; ein haengender Task fuehrt zu
  Log-Eintrag und kontrolliertem Neustart.

## Kernzuordnung, Begruendung

- WLAN-Stack und TLS laufen standardmaessig auf Kern 0 und belasten ihn spuerbar. Netz-
  und Persistenzarbeit liegen deshalb dort.
- Flash-Schreibzugriffe halten kurz beide Kerne an. Der NAU7802 puffert den Messwert im
  Register und signalisiert Bereitschaft; ein verspaetetes Auslesen kostet daher keinen
  Messwert. Eine IRAM-Pflicht fuer die Sensorroutine entfaellt.

## Busaufteilung

| Bus | Teilnehmer | Begruendung |
|---|---|---|
| I2C 0 | Multiplexer, 3x NAU7802 (Adresse fest 0x2A) | Waage exklusiv, kein Fremdverkehr |
| I2C 1 | PN532, spaeter IMU | NFC-Poll blockiert die Waage nicht |
| UART | Display | eigener Hardware-UART |
