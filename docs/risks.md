# Risiken

| Risiko | Wirkung | Gegenmassnahme |
|---|---|---|
| Portionsaufloesung bei 200 kg Vollskala | 0,2-l-Portion nahe der Fehlergrenze | Differenzmessung, Plateauvergleich, Settle-Fenster, Nachweis per Messreihe |
| Kriechen der Waegezellen | Kriechen wird als Bier gebucht | Buchung erst nach Settle, Anzeige entkoppelt |
| Zug am Zapfschlauch | Fehlbuchung ohne Entnahme | kraftfreie Schlauchfuehrung, Stabilitaetspruefung |
| Sockel verspannt Zellen | Nichtlinearitaet, Ecklastfehler | Konstruktion mit freier Durchbiegung, Ecklasttest |
| Powerbank schaltet ab | Systemausfall im Betrieb | Always-On-Modell oder Grundlast, Journal neustartfest |
| Kein Netz beim Check-in | Kein Tagestoken, kein QR | Zapfen und Buchen laeuft weiter, QR entfaellt |
| Roster-Cache veraltet | Neue Tags unbekannt | Warnung, manuelle Nachregistrierung, Devil's Share als Auffang |
| QR-Ghosting auf E-Paper | Token laenger sichtbar als gedacht | Vollrefresh nach Anzeige |
| Fassnachfuellung als Zapfung | Bilanz kaputt | Zunahme nie negativ buchen, Bestaetigung per Taste |
| Metall daempft NFC | schlechte Erkennung | Antennenposition experimentell bestimmen |
| Devil's Share waechst unerklaert | versteckter Messfehler | Bilanzkontrolle als Diagnosewert anzeigen |
