# Elektronik- und Verkabelungskonzept

## Komponenten

- TRMNL 7.5" OG DIY Kit mit XIAO ESP32-S3 Plus und EE04-Treiberboard
- 7.5" E-Paper, 800 x 480
- 4 x 50 kg Halbbrueckenzellen, zu einer Vollbruecke verschaltet, Vollskala 200 kg
- HX711 Waegewandler
- NFC-Leser PN532
- Powerbank als Energiequelle
- Optional spaeter: IMU, GNSS

## Mechanik, der entscheidende Teil

Der 3D-gedruckte Fasssockel traegt **ausschliesslich das Fass**. Damit bleiben
Wagenstruktur, angelehnte Personen und abgelegte Jacken ausserhalb der Messkette. Diese
Entscheidung ist messtechnisch wichtiger als jede Filterung in Software.

Anforderungen an den Sockel:

- Vier definierte Krafteinleitungspunkte, je eine Zelle pro Ecke
- Zellen duerfen sich frei durchbiegen, kein Verspannen im Druckteil
- Ueberlastanschlag, der ueber Vollskala mechanisch abfaengt
- Rutschsicherung fuer das Fass, ohne die Zellen seitlich zu belasten
- Zapfschlauch und Gasleitung **kraftfrei** gefuehrt, keine Zugkraft auf das Fass
- Sockel steht auf ebener, steifer Flaeche, nicht direkt auf federnden Wagenlatten
- Schichtrichtung und Wandstaerke so gewaehlt, dass Kriechen des Kunststoffs minimal
  bleibt, idealerweise Metalleinleger an den Auflagepunkten

Der Schlauchzug ist erfahrungsgemaess die haeufigste Fehlerquelle: Er wirkt wie eine
variable Zusatzlast und ist von echter Entnahme nicht unterscheidbar.

## Verschaltung der Waegezellen

Die vier Halbbruecken werden zu einer Wheatstone-Vollbruecke kombiniert und auf Kanal A
des HX711 gefuehrt. Kanal A bietet die hoehere Verstaerkung und damit die bessere
Aufloesung.

```text
Zelle 1..4 -> Vollbruecke -> kurze verdrillte Leitung -> HX711 (Kanal A)
HX711 -> zwei Digitalsignale -> ESP32
```

Praxisregeln:

- HX711 so nah wie moeglich an den Zellen, Digitalleitung lieber lang als Analogleitung
- Zellenkabel nicht verlaengern, wenn vermeidbar
- Abstand zu DC/DC-Wandler, WLAN-Antenne und Powerbank
- Schirm einseitig auf Masse, keine Masseschleife
- Sternfoermige Masse nahe der Versorgung

## Stromversorgung

Powerbank als Quelle. Der bekannte Fallstrick: viele Powerbanks schalten bei geringer
Last nach etwa 30 Sekunden ab.

Vorgaben:

- Modell mit Always-On- oder Passthrough-Funktion verwenden, oder
- definierte Grundlast vorsehen, falls kein solches Modell verfuegbar ist
- Kabel mit ausreichendem Querschnitt, USB-Spannungseinbruch beim WLAN-Peak messen
- Brownout-Erkennung aktiv, Journalschreibvorgaenge gegen Einbruch absichern
- Laufzeitbudget mit realem WLAN-, NFC- und Displaybetrieb messen, nicht schaetzen

## Kalibrierung

- Nullpunkt mit leerem Sockel, danach Tara persistent speichern
- Referenzgewicht moeglichst nahe der Betriebslast, nicht 1 kg bei 60 kg Nutzlast
- Ecklastpruefung an allen vier Positionen
- Kriech- und Driftprotokoll ueber mindestens die geplante Veranstaltungsdauer
- Kalibrierdaten versioniert und im Diagnosemenue sichtbar

## Vertagt

Die GPIO-Belegung wird erst bei Beginn der Board-Programmierung festgelegt. Die frueher
notierte Zuordnung ist nicht freigabefaehig, da GPIO5 doppelt belegt war und die
Batteriemesspfade nicht automatisch durch Weglassen der Batterie elektrisch frei
werden. Vor dem Loeten ist ein Pin-Audit am realen Board Pflicht.
