# 4. Lösungsstrategie

Kurzer Überblick über die grundlegenden Entscheidungen, die Entwurf und
Implementierung des Tierlist Makers prägen, sowie deren Begründung.

## Technologieentscheidungen

| Entscheidung | Begründung |
|---|---|
| Backend: Java mit Spring Boot | Festgelegte Randbedingung (Kap. 2). Spring Boot bietet mit vertretbarem Aufwand REST-Endpunkte, HTTP-Client für IGDB-Aufrufe und ist als Solo-Entwickler gut beherrschbar. |
| Frontend: Angular oder React (Entscheidung offen) | Beide erlauben ein reaktives, komponentenbasiertes UI, das für Drag & Drop (Tier-Stufen) gut geeignet ist. Die konkrete Wahl wird bewusst nicht architekturrelevant gemacht (siehe Top-Level-Zerlegung unten), damit die Entscheidung ohne Auswirkung auf das Backend nachgeholt werden kann. |
| Kommunikation via REST/JSON über HTTPS | Einfaches, weit verbreitetes, gut dokumentiertes Muster; passt zur Backend/Frontend-Trennung und ist ausreichend für die überschaubare Interaktionslast eines Hobbyprojekts. |
| Kein eigenes Speichern von Spiele-Assets | Bilder werden nicht heruntergeladen/gehostet, sondern per Bild-URL von IGDB referenziert. Reduziert Speicherbedarf, Wartungsaufwand und Rechtsfragen (Kap. 2). |

## Top-Level-Zerlegung

Das System wird als **Client-Server-Architektur** umgesetzt. Das
Backend ist ein **einziges deploybares Artefakt** (eine Spring-Boot-
Anwendung, ein Jar/eine ausführbare Datei), intern jedoch nach dem
Prinzip einer **Service-orientierten Architektur (SOA)** in mehrere
fachlich klar abgegrenzte Services gegliedert:

- **Frontend** (SPA, Angular oder React): Darstellung, Drag & Drop der
  Tier-Stufen, Aufruf der Backend-REST-API.
- **Tierlist-Service** (innerhalb des Backends): verwaltet das
  Anlegen, Bearbeiten und Speichern von Tierlisten sowie deren
  Tier-Stufen und Einträgen.
- **IGDB-Integration-Service** (innerhalb des Backends): kapselt die
  gesamte Kommunikation mit der IGDB/Twitch-API, inkl.
  Authentifizierung (Client-ID/Secret) und optionalem Caching von
  Suchergebnissen. Bietet den anderen Services eine eigene, von IGDB
  unabhängige Such-Schnittstelle (internes Interface, kein Netzwerk-
  Aufruf) an.

Die Services laufen im selben Prozess und kommunizieren über normale
Java-Schnittstellen (kein separates Deployment, kein API-Gateway, kein
Netzwerk-Overhead zwischen den Services) – der Betriebsaufwand bleibt
damit so gering wie bei einem Monolithen. Die fachliche Trennung
bringt trotzdem den Wartbarkeitsvorteil: Die beiden Kernaufgaben –
**Tierlisten-Verwaltung** und **externe Datenbeschaffung über IGDB** –
haben klar unterschiedliche Verantwortlichkeiten, Änderungsraten und
Fehlerquellen (z.B. soll ein Ausfall/Rate-Limit von IGDB die
Tierlisten-Verwaltung nicht beeinträchtigen, siehe Qualitätsziel
"Zuverlässigkeit"). Sollte das Projekt später wachsen, können einzelne
Services bei Bedarf auch separat deployt werden, ohne die fachlichen
Schnittstellen neu zu entwerfen.

## Entscheidungen zur Erreichung der Qualitätsziele

| Qualitätsziel (aus Kap. 1) | Lösungsansatz |
|---|---|
| Benutzerfreundlichkeit | Klare Trennung von Suche (IGDB) und Ranking (Drag & Drop) im UI; Fokus auf minimalen Bedienaufwand statt Funktionsvielfalt im MVP. |
| Performance der Suche | Backend fungiert als Proxy zu IGDB und kann Suchergebnisse cachen (z.B. In-Memory- oder einfacher DB-Cache), um wiederholte Anfragen zu beschleunigen und IGDB-Rate-Limits zu schonen. |
| Zuverlässigkeit der externen Anbindung | Fehler bei IGDB-Aufrufen werden im Backend abgefangen und als definierte Fehlerantwort an das Frontend weitergegeben, statt das System abstürzen zu lassen; bereits gespeicherte Tierlisten sind unabhängig von der IGDB-Erreichbarkeit nutzbar. |
| Wartbarkeit | Saubere Trennung Frontend/Backend über REST-API sowie fachliche Trennung im Backend selbst in eigenständige Services (Tierlist-Service, IGDB-Integration-Service). Eine spätere Erweiterung um weitere Kategorien (z.B. Filme) oder ein Wechsel der Datenquelle lässt sich als zusätzlicher, unabhängiger Service ergänzen, ohne bestehende Services zu ändern. |
| Portabilität / Zugänglichkeit | Frontend als Single-Page-Application im Browser, keine Installation nötig; responsives Layout für Desktop und Mobile. |

## Organisatorische Entscheidungen

- Entwicklung erfolgt allein, iterativ und ohne festen Zeitplan
  (Kap. 2). Es wird bewusst auf ein volles, striktes
  Vorgehensmodell verzichtet; stattdessen wird arc42 begleitend und in
  kleinen Schritten gepflegt, statt einmalig "fertig" geschrieben.
- Priorität liegt zunächst auf einem funktionierenden MVP
  (Suche + Ranking + Speichern), bevor Zusatzfunktionen (Teilen, Export,
  weitere Kategorien) angegangen werden.

*(TODO: sobald die Frontend-Entscheidung (Angular/React) gefallen ist,
hier ergänzen und ggf. als ADR/Kurzbegründung nachtragen.)*