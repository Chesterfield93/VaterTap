# Waagenmechanik

## Aufbau

- 3-Punkt-Lagerung: statisch bestimmt, kein Kippeln, jede Zelle traegt immer
- 3x Edelstahl-Doppelbolzen-Waegezelle, 30 kg Nennlast
- 3D-gedruckter Sockel mit Fasszentrierung
- Waage traegt ausschliesslich das Fass

## Lastrechnung (Schaetzung, nachwiegen)

| Posten | Masse |
|---|---|
| Bier, 30-l-Fass | ca. 30 kg |
| Leeres Fass | ca. 10-13 kg |
| Sockel/Plattform | ca. 1-2 kg |
| **Summe** | **ca. 42-46 kg** |
| Je Zelle statisch | ca. 15 kg = ca. 50 % Nennlast |
| Gesamtnennlast | 90 kg |
| 200-ml-Portion | ca. 0,22 % der Gesamtnennlast |

Gegenueber der verworfenen 4x50-kg-Loesung (200 kg, 0,1 %) ist die Portion mehr als
doppelt so gut aufgeloest.

## Ueberlast und Stoesse

Die Zellen sind laut Angebot robust gegen Ueberlast und Schlag. Die konkreten Werte
(sichere Ueberlast, Bruchlast) muessen aus dem Datenblatt der gelieferten Zellen
uebernommen werden; ohne Datenblatt gilt die Robustheit als unbelegt.

Abschaetzung: Hartes Absetzen eines vollen Fasses oder Bordsteinkanten koennen
kurzzeitig ein Mehrfaches der statischen Last erzeugen. Bei 50 % statischer Auslastung
genuegt bereits ein Faktor 2 bis 3, um die Nennlast zu ueberschreiten.

Entscheidung: zunaechst ohne Ueberlastanschlaege, aber konstruktiv so vorbereiten,
dass Anschlaege (Spalt ca. 0,3-0,5 mm unter der Plattform) nachgeruestet werden
koennen. Nachruestung, falls das Datenblatt weniger als 150 % sichere Ueberlast nennt
oder Nullpunktverschiebungen nach Transport auftreten.

## Krafteinleitung

Doppelbolzen-Kraftaufnehmer messen nur korrekt bei Krafteinleitung frei von Querkraft
und Biegemoment. Mechanische Robustheit gegen Seitenkraefte heisst Ueberleben, nicht
richtig messen.

- Oben je Zelle Druckstueck mit Kalotte oder Kugelkopf, keine starre Verschraubung
- Gewinde nur auf einer Seite fest verschrauben
- Zapf- und Gasschlauch kraftfrei fuehren

## Fuehrungsbolzen mit Gleitlagern

Fuehrungsbolzen nehmen Seitenkraefte auf und halten die Plattform zentriert.

Kritischer Punkt: Jede Reibung in vertikaler Richtung bildet einen **Kraftnebenschluss**.
Ein Teil der Gewichtskraft geht dann ueber die Fuehrung statt ueber die Zellen. Folgen
sind Hysterese und Stick-Slip, also genau Fehler in der Groessenordnung einer Portion.

Konstruktionsregeln:

- Radiales Spiel, sodass die Bolzen im Ruhezustand **nicht anliegen**
  und nur bei Seitenstoss Kontakt haben
- Reibungsarme Paarung (z. B. PTFE- oder Polymer-Gleitlager auf Edelstahl)
- Keine Vorspannung, keine Dichtungen mit Reibung
- Fuehrung wirkt nur radial; vertikal muss die Plattform frei sein

Verifikation: Hysteresetest (Last auf, Last ab, Differenz) mit und ohne Seitenkontakt.
Liegt die Hysterese ueber 20 g, ist die Fuehrung zu straff.

## Sockel aus 3D-Druck

- Auflagepunkte mit Metalleinlegern, damit Kunststoffkriechen nicht ins Messergebnis geht
- Steife Plattform, damit sich die Lastverteilung bei Fassverschiebung stetig aendert
- Sensorbox geschuetzt unter oder neben dem Sockel, Kabel nicht im Kraftfluss
