# Systemarchitektur

## Zielbild

```text
[NFC]     [Waage]     [Taster]     [E-Paper]
   |         |           |             |
   +---------+-----+-----+-------------+
                   |
          [ESP32-S3 Edge Node]
          - Messung und Filterung
          - Zapf-Zustandsautomat
          - Nutzersitzung und Attribution
          - Lokales Event-Journal
          - Token- und QR-Anzeige
                   |
             ausgehendes HTTPS
                   |
        [Reverse Proxy /vatertap/api/v1]
                   |
        [AppDaemon als Python-Backend]
          - Roster-Verwaltung
          - Aggregation und Historie
          - Read-only Webansicht
          - Dateibasierte Persistenz
```

## Verantwortungsgrenzen

### ESP32-S3

Der ESP ist die fachliche Wahrheit zum Zeitpunkt der Messung.

- Waage einlesen, filtern, Stabilitaet bewerten
- Zapfbeginn und Zapfstopp als Zustandsautomat erkennen
- Nutzersitzung fuehren und Menge lokal attribuieren
- Devil's Share bei fehlender Sitzung buchen
- Ereignisse mit Sequenznummer persistent puffern
- E-Paper inklusive Fehlercodes und QR bedienen
- Roster und Konfiguration lokal cachen

### AppDaemon

- Ereignisse idempotent annehmen und dateibasiert ablegen
- Aggregation je Nutzer, Tag und Fass
- Read-only Webansicht mit Tagestoken
- Roster und Konfiguration ausliefern
- Gerätestatus ueberwachen

### Home Assistant

Fuer v1 **nicht** Bestandteil des Datenpfads. AppDaemon dient ausschliesslich als
Python-Laufzeitumgebung. Eine spaetere MQTT-Bruecke bleibt backendseitig moeglich.

## Warum Attribution auf dem Geraet liegt

Eine Zuordnung im Backend waere netzabhaengig. Am Vatertag ist genau das der
unzuverlaessigste Teil des Systems. Der ESP kennt Sitzung, Zeitpunkt und Messung
ohnehin vollstaendig; das Backend wuerde dieselbe Entscheidung nur spaeter und mit
schlechteren Informationen erneut treffen. Das Backend darf Attribution daher
**nicht** korrigieren, sondern nur aggregieren.

Konsequenz: Der Roster (Tag-UID zu Nutzer-ID) muss lokal vorliegen. Er wird beim Start
und periodisch geladen und persistent gecacht. Ein unbekannter Tag erzeugt keine
Sitzung, sondern eine neutrale Anzeige.

## Datenfluss einer Zapfung

1. Tag aufgelegt, Sitzung startet, Tagestoken wird geholt oder aus Cache genutzt.
2. Gewichtsabnahme ueberschreitet Startschwelle, Zustand wechselt auf `POURING`.
3. Gewicht bleibt fuer die Stopp-Zeit stabil, Zustand wechselt auf `SETTLING`.
4. Nach Settle-Fenster wird die Menge final berechnet und als Ereignis geschrieben.
5. QR-Code mit persoenlichem Statistik-Link wird angezeigt, danach Vollrefresh.
6. Ereignis wird bei naechster Gelegenheit an das Backend gesendet und bestaetigt.

## Bewusst vertagte Themen

- GPIO-Belegung und Board-Rework
- Firmware-Toolchain
- Ein- oder Zwei-Knoten-Aufbau
