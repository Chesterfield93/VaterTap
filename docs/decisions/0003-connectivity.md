# ADR-0003: Transport Backend

- Status: accepted

## Entscheidung

Nur das Brain spricht mit dem Backend, ausschliesslich per ausgehendem HTTPS an einen
oeffentlichen Reverse Proxy. Kein MQTT, kein WireGuard, keine HA-Entities im Datenpfad.

## Begruendung

MQTT ist nicht exponiert und haette unterwegs WireGuard erzwungen. HTTPS vermeidet
Tunnel-Handshake, MTU- und Roaming-Probleme und reduziert Firmwarekomplexitaet.

## Konsequenzen

- Geraeteauthentifizierung, Rate Limiting und Groessenlimits am Proxy
- Spaetere MQTT-Bruecke nur backendseitig
- Display hat keine Backend-Verbindung
