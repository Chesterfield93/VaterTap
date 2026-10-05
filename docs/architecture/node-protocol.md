# Knotenprotokoll Brain <-> Display

## Transport

- Hardware-UART, 115200 Baud (Startwert, nach Test anpassbar)
- Rahmen: eine JSON-Nachricht pro Zeile, CRC als Hex-Suffix, Zeilenende `\n`

```text
{"v":1,"t":"vm","seq":1042,...}*3F1A
```

- CRC: CRC-16/CCITT ueber die JSON-Bytes vor dem `*`
- Maximale Zeilenlaenge 1 KB; laengere Zeilen werden verworfen und gezaehlt
- Kaputte Zeile (CRC falsch, kein JSON) wird verworfen; Resync am naechsten `\n`

Warum Zeilenende statt Laengenpraefix: im seriellen Monitor direkt lesbar, triviale
Resynchronisation, kein Risiko, da JSON Zeilenumbrueche in Strings immer escaped.

## Pflichtfelder jeder Nachricht

| Feld | Bedeutung |
|---|---|
| `v` | Protokollversion |
| `t` | Nachrichtentyp |
| `seq` | Sequenznummer je Sender, monoton |

Bei unbekannter Protokollversion zeigt das Display einen Versionsfehler statt zu raten.
Grund: Beide Knoten werden unabhaengig per OTA aktualisiert.

## Nachrichtentypen

| Typ | Richtung | Takt | ACK |
|---|---|---|---|
| `vm` Viewmodel | Brain -> Display | 1 Hz + sofort bei Aenderung | nein |
| `btn` Tastenereignis | Display -> Brain | ereignisgetrieben | **ja**, Retry, Dedup ueber `seq` |
| `hb` Heartbeat | Display -> Brain | 1 Hz | nein |
| `ack` | Brain -> Display | auf `btn` | - |

## Warum nicht alles bestaetigt wird

Das Viewmodel ist ein Zustand, der eine Sekunde spaeter vollstaendig neu kommt. Ein
ACK mit Retry wuerde veraltete Daten wiederholen. Tastenereignisse sind dagegen
einmalig; ihr Verlust waere verlorene Bedienung.

## Viewmodel

Immer vollstaendiger Zustand, nie Deltas. Das Display ermittelt Aenderungen selbst und
waehlt den Refresh. Inhalt sind fertige Anzeigewerte, keine Rohdaten:

- Bildschirmmodus: `idle`, `user_active`, `pouring`, `qr`, `error`, `menu`, `diag`
- Restvolumen in ml, Fuellstand in Prozent
- Anzeigename des aktiven Nutzers (nie Tag-UID)
- Letzte Zapfmenge in ml
- Fehlercode
- Statusflags: Netz, Backend, Journalfuellstand
- QR-Payload als URL-String plus Anzeigedauer in s
- Menuezustand (Menue wird im Brain gefuehrt)

## Heartbeat Display

- zuletzt gerenderte Viewmodel-`seq` (impliziter Empfangsnachweis)
- Firmwareversion, Protokollversion
- Refresh-Zaehler, Fehlerzaehler (CRC, Ueberlauf)

## Watchdog

| Seite | Ausloeser | Reaktion |
|---|---|---|
| Display | 3 s ohne gueltiges `vm` | Anzeige "Brain nicht erreichbar" |
| Brain | 3 s ohne `hb` | Log-Eintrag Fehler, Status ans Backend; Betrieb laeuft weiter |

## Tasten

Das Display meldet nur Rohereignisse (`btn1_short`, `btn2_long`, ...). Die Bedeutung
legt ausschliesslich das Brain fest.
