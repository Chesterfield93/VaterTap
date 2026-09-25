# Attribution, Zapferkennung und Ereignisse

## Sitzungsmodell

| Uebergang | Ausloeser |
|---|---|
| Sitzung startet | Registrierter NFC-Tag wird gelesen |
| Sitzung endet | Neuer Check-in eines anderen Tags |
| Sitzung endet | Keine Entnahme fuer `checkout_timeout_s`, Standard 2 s |
| Sitzung endet | Fehlerzustand oder Neustart |

Der Tag muss **nicht** waehrend des Zapfens aufliegen. Der Check-in ist der Startpunkt
der Messung.

## Devil's Share

Ein fest definiertes Sammelkonto fuer alles, was keinem Nutzer zurechenbar ist:

- Entnahme ohne aktive Sitzung
- Entnahme nach Timeout
- Schaum, Leerlauf, Reinigung, Probezapfen
- Verlust durch Umkippen oder Verschuetten, sofern als Entnahme erkannt

Das Konto ist damit gleichzeitig die **Bilanzkontrolle**: Summe aller Nutzerbuchungen
plus Devil's Share muss der gemessenen Gesamtentnahme entsprechen. Weicht das ab, ist
die Messkette defekt.

Eine Korrekturfunktion im Menue erlaubt die nachtraegliche Umbuchung einer letzten
Devil's-Share-Position auf den zuletzt aktiven Nutzer.

## Zustandsautomat

```text
IDLE
  -> USER_ACTIVE        (Check-in)
USER_ACTIVE | IDLE
  -> POURING            (Gewichtsabnahme > Startschwelle)
POURING
  -> SETTLING           (Gewicht stabil fuer stop_debounce_s)
SETTLING
  -> EVENT_READY        (Settle-Fenster abgelaufen)
EVENT_READY
  -> USER_ACTIVE | IDLE (Ereignis geschrieben)
```

Fehlerzustaende: `SENSOR_FAULT`, `CALIBRATION_REQUIRED`, `STORAGE_FULL`, `DEGRADED`.

## Zwei Zeitkonstanten, bewusst getrennt

| Parameter | Zweck | Richtwert |
|---|---|---|
| `stop_debounce_s` | Zapfstopp fuer die **Anzeige** erkennen | 1 bis 2 s |
| `settle_window_s` | Messwert fuer die **Buchung** stabilisieren | 4 bis 6 s, empirisch |

Begruendung: Die Waegezellen kriechen unmittelbar nach einer Lastaenderung. Wird sofort
nach dem Debounce gebucht, wandert ein Teil des Kriechens als Biermenge in die
Statistik. Die Anzeige darf deshalb schnell reagieren, die Buchung nicht. Der
Vatertags-Flow bleibt erhalten, weil der Nutzer die schnelle Reaktion sieht und die
Nachkorrektur nicht bemerkt.

Der finale Wert wird als Differenz zweier **stabiler** Plateaus gebildet, nicht als
Momentanwert.

## Ereignisformat

Jedes Ereignis ist unveraenderlich und enthaelt mindestens:

| Feld | Bedeutung |
|---|---|
| `event_id` | Eindeutige ID, geraeteseitig erzeugt |
| `device_id` | Geraetekennung |
| `sequence` | Monoton steigend, luekenlos je Geraet |
| `ts_utc` | Wanduhrzeit, kann bei fehlender Zeitsynchronisation unsicher sein |
| `ts_monotonic_ms` | Geraetelaufzeit, immer verlaesslich |
| `user_id` | Pseudonym oder `devils_share` |
| `mass_delta_g` | Gemessene Massedifferenz |
| `volume_ml` | Abgeleitetes Volumen |
| `mass_before_g` / `mass_after_g` | Plateauwerte |
| `quality` | Flags wie `unstable`, `motion`, `short_pour`, `clock_unsynced` |
| `payload_version` | Schemaversion |

Ereignistypen: `pour`, `keg_change`, `tare`, `correction`, `diagnostic`.

## Plausibilisierung

- Entnahmen unterhalb der Mindestmenge werden akkumuliert, nicht einzeln gebucht.
- Gewichtszunahme erzeugt **nie** eine negative Zapfung.
- Unplausibel grosse Spruenge werden als `quality.unstable` markiert und angezeigt.
- Ohne gueltige Kalibrierung wird nicht gebucht.
