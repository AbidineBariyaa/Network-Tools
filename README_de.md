# Netline — Netzwerk-Tools Dashboard

Ein kleines, in sich geschlossenes Netzwerk-Diagnose-Dashboard, gebaut mit **HTML, CSS und reinem JavaScript**. Kein Framework, kein Build-Prozess — einfach `netline_de.html` im Browser öffnen und es funktioniert.

## Funktionen

| Werkzeug | Was es macht | Datenquelle |
|---|---|---|
| **Verbindungsinfo** | Zeigt öffentliche IP, Provider/Organisation, Stadt, Land und Zeitzone | [ipapi.co](https://ipapi.co) (öffentliche REST-API) |
| **DNS-Auflösung** | Löst A-, AAAA-, MX-, TXT-, NS- oder CNAME-Einträge für eine beliebige Domain auf | [Cloudflare DNS-over-HTTPS](https://developers.cloudflare.com/1.1.1.1/encryption/dns-over-https/) (JSON-API) |
| **Latenz-Test** | Misst die tatsächliche Roundtrip-Zeit zu einer beliebigen URL über 3 Anfragen (Durchschnitt / Min / Max / Jitter) | Clientseitige `fetch()`-Zeitmessung |
| **Subnetz-/CIDR-Rechner** | Berechnet Netzwerkadresse, Broadcast, Subnetzmaske, Wildcard-Maske und nutzbaren Hostbereich aus IP + Präfix | Reine JS-Bit-Operationen — keine API nötig |

## Erste Schritte

1. `netline_de.html` herunterladen.
2. Direkt in einem modernen Browser öffnen (Chrome, Firefox, Edge, Safari).
3. Fertig — kein Server, kein `npm install`, keine Abhängigkeiten zum Bauen.

> Werkzeuge 1–3 benötigen eine Internetverbindung, da sie öffentliche APIs aufrufen. Werkzeug 4 (Subnetz-Rechner) funktioniert komplett offline.

## Wie es aufgebaut ist

- **HTML** — Eine einzelne Seite: Hero-Bereich + ein 2×2-Werkzeug-Raster.
- **CSS** — Eigene CSS-Variablen (`:root`) für Farben/Abstände, kein externes CSS-Framework.
- **JavaScript** — Pro Werkzeug eine `async`/`await`-Funktion, `fetch()` für Netzwerk-Aufrufe, Template-Literale zur Darstellung der Ergebnisse, grundlegende Eingabevalidierung und durchgängiges `try/catch`-Fehlerhandling.

## Bekannte Einschränkungen

- Der **Latenz-Test** nutzt `mode: 'no-cors'`. Dadurch kann er die Zeit zu fast jedem Host messen, aber der Browser blockiert das Auslesen des echten HTTP-Statuscodes oder des Response-Bodys bei Cross-Origin-Anfragen — man bekommt nur "hat geantwortet" + Zeitmessung, keinen echten HTTP-Status.
- **DNS-Auflösung** und **Verbindungsinfo** nutzen kostenlose öffentliche APIs (Cloudflare, ipapi.co), die bei intensiver Nutzung eine Ratenbegrenzung haben können.
- Dies ist ein **Diagnose-/Demo-Tool**, kein Ersatz für CLI-Tools wie `ping`, `dig` oder `traceroute` — Browser haben keinen Zugriff auf Raw Sockets, daher ist alles, was ICMP oder TCP-Zugriff auf niedriger Ebene braucht (echtes Ping, echtes Port-Scanning), aus clientseitigem JS heraus nicht möglich. Dieses Projekt täuscht das auch nicht vor.

## Dateistruktur

```
netline_de.html   → die gesamte App (HTML + CSS + JS in einer Datei)
README.md         → diese Datei
```

## Mögliche nächste Schritte

- WHOIS-Abfrage für Domains
- HTTP-Response-Header-Inspektor
- Traceroute-artige Visualisierung über eine öffentliche API
- Aufteilung in separate Dateien `index.html` / `style.css` / `script.js`
