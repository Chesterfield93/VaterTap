# Attribution, Zapferkennung und Ereignisse

## Sitzungsmodell

| Uebergang | Ausloeser |
|---|---|
| Sitzung startet | Registrierter Tag wird gelesen |
| Sitzung endet | Check-in eines anderen Tags |
| Sitzung endet | Keine Entnahme fuer `checkout_timeout_s` (Standard 2 s) |
| Sitzung endet | Fehlerzustand oder Neustart |

Der Tag muss waehrend des Zapfens nicht aufliegen.

## Devil's Share

Sammelkonto fuer alles ohne aktive Sitzung: Zapfen ohne Check-in, nach Timeout,
Schaum, Probezapfen, Verlust.

Zugleich **Bilanzkontrolle**: Summe aller Nutzerbuchungen plus Devil's Share muss der
gemessenen Gesamtentnahme entsprechen. Ein unerklaert wachsender Devil's Share deutet
auf einen Messkettenfehler hin.

Menuefunktion: letzte Devil's-Share-Position auf den zuletzt aktiven Nutzer umbuchen.

## Zustandsautomat

```text
IDLE           -> USER_ACTIVE     Check-in
USER_ACTIVE    -> IDLE            Timeout ohne Entnahme
USER_ACTIVE|IDLE -> POURING       Massenabnahme > Startschwelle
POURING        -> SETTLING        Masse stabil fuer stop_debounce_s
SETTLING       -> EVENT_READY     settle_window_s abgelaufen
EVENT_READY    -> USER_ACTIVE|IDLE Event geschrieben
```

Fehlerzustaende: `SENSOR_FAULT`, `CALIBRATION_REQUIRED`, `STORAGE_FULL`, `DEGRADED`.

## Zwei Zeitkonstanten

| Parameter | Zweck | Richtwert |
|---|---|---|
| `stop_debounce_s` | Zapfstopp fuer die **Anzeige** | 1-2 s |
| `settle_window_s` | Messwert fuer die **Buchung** | 4-6 s, aus Kriechprotokoll |

Die Zellen kriechen direkt nach Lastwechsel. Sofortiges Buchen wuerde Kriechen als
Bier zaehlen. Die Anzeige reagiert schnell, die Buchung folgt spaeter.

Der Buchungswert ist die Differenz zweier **stabiler Plateaus** der Summenmasse.

## Zellendiagnose

Die drei Einzelwerte werden zusaetzlich zur Summe ausgewertet:

- Lastverteilung bei Stillstand vergleichen mit Referenz nach Kalibrierung
- Starke Verschiebung bei konstanter Summe: Fass verrutscht oder Fuehrung klemmt
- Eine Zelle ohne Aenderung bei Entnahme: Zelle oder Kanal defekt
- Plausibilitaetsverletzung fuehrt zu `quality`-Flag oder `SENSOR_FAULT`

## Eventformat

| Feld | Bedeutung |
|---|---|
| `event_id` | eindeutig, im Brain erzeugt |
| `device_id` | Geraetekennung |
| `sequence` | monoton, lueckenlos je Geraet |
| `ts_utc` | Wanduhrzeit, ggf. unsynchronisiert |
| `ts_monotonic_ms` | Laufzeit, immer verlaesslich |
| `user_id` | Pseudonym oder `devils_share` |
| `mass_before_g` / `mass_after_g` | Plateauwerte der Summe |
| `mass_delta_g` | Differenz |
| `volume_ml` | abgeleitet |
| `cell_delta_g` | Differenz je Zelle, fuer Diagnose |
| `quality` | Flags: `unstable`, `motion`, `short_pour`, `clock_unsynced`, `cell_imbalance` |
| `payload_version` | Schemaversion |

Typen: `pour`, `keg_change`, `tare`, `correction`, `diagnostic`.

## Plausibilisierung

- Entnahmen unter Mindestmenge werden akkumuliert, nicht einzeln gebucht.
- Massenzunahme erzeugt nie eine negative Zapfung.
- Ohne gueltige Kalibrierung wird nicht gebucht.
