# Risiken

| Risiko | Wirkung | Gegenmassnahme |
|---|---|---|
| Reibung in Fuehrungsbolzen | Kraftnebenschluss, Hysterese, Fehlbuchung | Radiales Spiel, Hysteresetest |
| Ueberlast durch Stoss | Nullpunktverschiebung, Zellenschaden | Datenblatt pruefen, Anschlaege nachruestbar |
| Querkraft auf Zellen | Messfehler | Kalotten, einseitige Verschraubung |
| Kriechen der Zellen | Kriechen als Bier gebucht | Settle-Fenster |
| Zug am Zapfschlauch | Fehlbuchung | kraftfreie Fuehrung, Zellendiagnose |
| Lange I2C-Leitung zur Sensorbox | Busfehler | kurz halten, Takt senken, ggf. Extender |
| Versionsdrift Brain/Display | Falschanzeige | Protokollversion, E10 |
| Fehlende gemeinsame Masse | UART-Fehler | Masse explizit fuehren |
| Powerbank schaltet ab | Ausfall | Always-On, neustartfestes Journal |
| Fehlersturm | Speicher voll | Logs getrennt, Ringpuffer, Dedup |
| Kein Netz beim Check-in | kein QR | Buchung laeuft weiter |
| QR-Ghosting | Token laenger sichtbar | Vollrefresh |
| Metall daempft NFC | schlechte Erkennung | Position testen |
| Unsignierte OTA | Manipulation | Wartungsmodus bis Signierung |
