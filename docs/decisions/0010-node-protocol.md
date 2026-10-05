# ADR-0010: UART-Protokoll Brain/Display

- Status: accepted

## Entscheidung

- NDJSON: eine Nachricht pro Zeile, `*` plus CRC-16/CCITT als Hex, `\n`
- Pflichtfelder `v`, `t`, `seq`; max. 1 KB je Zeile
- Viewmodel Brain -> Display: vollstaendiger Zustand, 1 Hz + bei Aenderung, ohne ACK
- Tastenereignisse Display -> Brain: mit ACK, Retry, Dedup
- Heartbeat Display -> Brain: 1 Hz, enthaelt zuletzt gerenderte `seq`
- Watchdog beidseitig, 3 s

## Verworfene Optionen

- Laengenpraefix: schwer zu debuggen, schlechter Resync
- Binaerformat (CBOR, Struct): Groesse irrelevant, Debugbarkeit wichtiger
- Pixelframes: ca. 48 KB je Frame, koppelt Layout an das Brain
- ACK auf alles: wiederholt veraltete Zustaende

## Konsequenzen

Protokollcode liegt in `firmware/shared/protocol` und wird von beiden Knoten genutzt.
Inkompatible Versionen fuehren zu E10.
