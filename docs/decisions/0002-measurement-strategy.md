# ADR-0002: Messstrategie

- Status: accepted

## Entscheidung

Gravimetrische Messung ueber vier 50-kg-Halbbrueckenzellen in Vollbruecke, HX711,
Vollskala 200 kg. Die Waage traegt ausschliesslich das Fass auf einem 3D-gedruckten
Sockel.

Durchflussmessung ist **verworfen**. Fuer karbonisiertes Bier sind gaengige
Durchflussmesser durch Gasanteil und Schaum unzuverlaessig, zusaetzlich entsteht
Reinigungsaufwand im Produktpfad.

## Begruendung fuer den Fasssockel

Wuerde ein Teil der Wagenstruktur mitgemessen, gingen angelehnte Personen und abgelegte
Gegenstaende in die Messung ein. Ein separater Sockel entkoppelt die Messkette
mechanisch und ist wirksamer als jede Softwarefilterung. Seitliches Verrutschen des
Fasses ist unkritisch, solange die Gesamtmasse auf den vier Punkten verbleibt.

## Konsequenzen

- 0,2-l-Portion entspricht etwa 0,1 Prozent der Vollskala. Absolutgenauigkeit reicht
  dafuer nicht aus; die Portion wird deshalb rein differenziell aus zwei stabilen
  Plateaus bestimmt.
- Portions- und Restmengengenauigkeit werden getrennt spezifiziert, siehe NFR-001 und
  NFR-002.
- Kriechen der Zellen erzwingt ein Settle-Fenster vor der Buchung.
- Schlauchfuehrung muss kraftfrei sein.

## Verifikation

Messreihe mit mindestens 20 Referenzentnahmen von 0,2 l, Kriechprotokoll und
Ecklasttest. Wird NFR-001 verfehlt, ist die Alternative ein Single-Point-Waegebalken mit
geringerer Vollskala.
