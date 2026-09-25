# ADR-0003: Transport und Konnektivitaet

- Status: accepted

## Entscheidung

Ausschliesslich ausgehendes HTTPS vom Geraet zu einem oeffentlich erreichbaren
Reverse-Proxy-Endpunkt. Kein MQTT, kein WireGuard, keine Home-Assistant-Integration im
Datenpfad fuer v1. AppDaemon dient nur als Python-Laufzeitumgebung.

## Begruendung

MQTT ist nicht ins Internet exponiert und haette unterwegs zwingend WireGuard
erfordert. Damit entfielen mit HTTPS mehrere Fehlerquellen gleichzeitig:

- kein WireGuard-Handshake, der unterwegs scheitern kann
- keine MTU-Probleme bei Tunnelbetrieb
- kein Roaming- und Endpoint-Problem am wechselnden Hotspot
- keine Abhaengigkeit von Zeitsynchronisation ausserhalb des Tunnels
- deutlich weniger Firmwarekomplexitaet

## Konsequenzen

- Der Endpunkt muss oeffentlich erreichbar und TLS-gesichert sein.
- Geraeteauthentifizierung ist verpflichtend, da der Endpunkt erreichbar ist.
- Rate Limiting und Request-Groessenbegrenzung am Proxy erforderlich.
- Sensorwerte stehen zunaechst nicht in Home Assistant zur Verfuegung.

## Spaetere Option

Benoetigt Home Assistant spaeter Sensorwerte, veroeffentlicht das **Backend** sie per
MQTT. Das Geraet bleibt unveraendert bei HTTPS.
