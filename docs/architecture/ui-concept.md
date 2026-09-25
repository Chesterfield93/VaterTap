# Bedienung und Anzeige

## Randbedingung E-Paper

Vollrefresh dauert mehrere Sekunden und flackert. Teilrefresh ist schnell, hinterlaesst
aber Ghosting. Das Bedienkonzept muss beides beruecksichtigen.

## Bildschirmbereiche

| Bereich | Inhalt | Refresh |
|---|---|---|
| Kopf | Restmenge, Fassstatus | Teilrefresh bei Aenderung |
| Mitte | Aktueller Nutzer, letzte Zapfmenge | Teilrefresh |
| QR-Feld | Persoenlicher Statistik-Link | Teilrefresh, danach Vollrefresh |
| Fuss | Systemstatus, Netz, Fehlercode | Teilrefresh |

## Fehleranzeige

Fehler erscheinen als kurzer Code plus Klartext, damit im Feld ohne Laptop
diagnostiziert werden kann.

| Code | Bedeutung |
|---|---|
| `E01` | Waage liefert keine Daten |
| `E02` | Kalibrierung fehlt oder ungueltig |
| `E03` | NFC-Leser nicht erreichbar |
| `E04` | Journal voll |
| `E05` | Backend nicht erreichbar, Puffer aktiv |
| `E06` | Zeit nicht synchronisiert |
| `E07` | Messwert instabil, Buchung ausgesetzt |

## Tastenbelegung, Vorschlag

Flache Struktur, maximal eine Ebene, da jeder Menuewechsel einen Refresh kostet.

| Taste | Kurz | Lang |
|---|---|---|
| 1 | Ansicht wechseln, Statistik und Diagnose | Tara |
| 2 | Devil's-Share-Korrektur auf letzten Nutzer | Fasswechsel bestaetigen |
| 3 | optional, entfaellt falls GPIO benoetigt | - |

Die endgueltige Belegung haengt von der spaeteren GPIO-Entscheidung ab. Tara und
Fasswechsel liegen bewusst auf Langdruck, da beide die Messbasis veraendern.

## QR und Ghosting

Nach jeder QR-Anzeige ist ein Vollrefresh verpflichtend. Ein geisterhaft sichtbarer
QR-Code bleibt oft scanbar und wuerde das Token laenger exponieren als vorgesehen.
