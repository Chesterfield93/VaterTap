# ADR-0002: Messstrategie und Waegezellen

- Status: accepted (Revision 2, ersetzt 4x50-kg-Variante)

## Entscheidung

- Gravimetrisch, 3-Punkt-Lagerung
- 3x Edelstahl-Doppelbolzen-Waegezelle, 30 kg Nennlast, Gesamtnennlast 90 kg
- Sockel mit Fasszentrierung und Fuehrungsbolzen mit Gleitlagern
- Waage traegt nur das Fass
- Durchflussmessung verworfen (Gasanteil, Schaum, Reinigung)

## Begruendung

- 3 Punkte sind statisch bestimmt; jede Zelle traegt immer, kein Kippeln
- Maximal 30-l-Fass, Gesamtlast ca. 42-46 kg; 30 kg je Zelle ergibt ca. 50 %
  statische Auslastung mit Reserve
- 200-ml-Portion entspricht ca. 0,22 % der Nennlast statt 0,1 % bei 200 kg
- Einzelzellen ermoeglichen Diagnose und Ecklastkorrektur

## Konsequenzen

- Fuehrungsbolzen nur mit radialem Spiel, sonst Kraftnebenschluss
  (siehe `docs/hardware/scale-mechanics.md`)
- Ueberlastanschlaege konstruktiv vorbereiten, Einbau nach Datenblattlage
- Portion weiterhin rein differenziell ueber stabile Plateaus

## Verifikation

Hysteresetest, Ecklasttest, 20 Referenzentnahmen 200 ml, Nullpunkt nach Transport.
