# ADR-0008: Stromversorgung

- Status: accepted

## Entscheidung

Versorgung ueber eine Powerbank.

## Bekannter Fallstrick

Viele Powerbanks schalten bei geringer Stromaufnahme nach etwa 30 Sekunden ab. Ob das
System dauerhaft ueber der Abschaltschwelle bleibt, darf nicht dem Zufall ueberlassen
werden.

## Auflagen

- Modell mit Always-On- oder Passthrough-Funktion, oder definierte Grundlast
- Reale Laufzeit und Stromaufnahme messen, nicht schaetzen
- Spannungseinbruch bei WLAN- und Displayspitzen pruefen
- Brownout-Erkennung aktiv, Journal gegen Abbruch abgesichert
- Kabel mit ausreichendem Querschnitt, moeglichst kurz

## Konsequenz

Ein unerwarteter Neustart ist ein **erwarteter** Betriebsfall. Die Firmware muss ihn
ohne Datenverlust und ohne Phantomereignis ueberstehen.
