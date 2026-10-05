# Bedienung und Anzeige

## Rollenverteilung

Das Brain entscheidet **was** angezeigt wird (Viewmodel, Menuezustand). Das Display
entscheidet **wie** (Layout, QR-Pixel, Refresh-Art).

## E-Paper-Randbedingung

Vollrefresh dauert Sekunden und flackert, Teilrefresh hinterlaesst Ghosting.

| Bereich | Inhalt | Refresh |
|---|---|---|
| Kopf | Restmenge, Fuellstand | Teil |
| Mitte | Nutzer, letzte Zapfmenge | Teil |
| QR-Feld | Statistik-Link | Teil, danach verpflichtend Voll |
| Fuss | Netz, Backend, Fehlercode | Teil |

## Fehlercodes

| Code | Bedeutung | Erkannt durch |
|---|---|---|
| `E01` | Waage liefert keine Daten | Brain |
| `E02` | Kalibrierung fehlt | Brain |
| `E03` | NFC nicht erreichbar | Brain |
| `E04` | Journal voll | Brain |
| `E05` | Backend nicht erreichbar, Puffer aktiv | Brain |
| `E06` | Zeit nicht synchronisiert | Brain |
| `E07` | Messwert instabil, Buchung ausgesetzt | Brain |
| `E08` | Zellenungleichgewicht oder Zellendefekt | Brain |
| `E09` | Brain nicht erreichbar | Display, lokal |
| `E10` | Protokollversion inkompatibel | Display, lokal |

`E09` und `E10` erzeugt das Display selbst, da in diesen Faellen kein Viewmodel kommt.

## Tasten

Flach, maximal eine Menueebene.

| Taste | Kurz | Lang |
|---|---|---|
| 1 | Ansicht wechseln (Status, Statistik, Diagnose) | Tara |
| 2 | Devil's-Share-Korrektur auf letzten Nutzer | Fasswechsel bestaetigen |
| 3 | optional | - |

Tara und Fasswechsel liegen auf Langdruck, da beide die Messbasis aendern.
