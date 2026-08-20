# 2. Randbedingungen

Randbedingungen und Vorgaben, die die Freiheiten bezüglich Entwurf,
Implementierung und Entwicklungsprozess des Tierlist Makers einschränken.

## Technische Randbedingungen

| Randbedingung | Erläuterung |
|---|---|
| Backend: Java mit Spring (Boot) | Festgelegte Backend-Technologie. Beeinflusst u.a. Projektstruktur, Build-Tool (Maven/Gradle), REST-API-Design. |
| Frontend: Angular *oder* React (noch offen) | Entscheidung steht noch aus. Solange offen, sollte das Backend über eine klar getrennte REST-API kommunizieren, damit die Wahl des Frontend-Frameworks die Architektur nicht beeinflusst. *(TODO: Entscheidung nachtragen, sobald getroffen – idealerweise früh, da sie z.B. State-Management und Component-Struktur festlegt.)* |
| Abhängigkeit von der IGDB-API (Twitch-API) | Spielsuche und Bildmaterial werden extern über [IGDB](https://api-docs.igdb.com/) bezogen. Erfordert eine Twitch-Developer-App zur Authentifizierung (Client-ID/Secret), unterliegt den Rate-Limits und Nutzungsbedingungen von IGDB/Twitch. Das System ist damit von der Verfügbarkeit dieses Drittanbieters abhängig. |
| Keine eigene Spiele-Datenbank (zunächst) | Spieledaten werden nicht dauerhaft repliziert, sondern bei Bedarf live über IGDB abgefragt bzw. nur die für eine Tierlist nötigen Daten (Titel, Bild-URL, IGDB-ID) gespeichert. |
| Hosting noch nicht entschieden | Es gibt noch keine Festlegung auf einen konkreten Hosting-Anbieter oder eigenen Server. *(TODO: nachtragen, sobald entschieden – beeinflusst z.B. ob Backend "always-on" laufen kann oder mit Cold-Starts umgehen muss.)* |

## Organisatorische Randbedingungen

| Randbedingung | Erläuterung |
|---|---|
| Ein-Personen-Hobbyprojekt | Entwicklung erfolgt allein, nebenbei, ohne festes Team oder festen Zeitplan. Das begrenzt verfügbare Zeit und spricht für pragmatische, einfach zu wartende Lösungen statt Over-Engineering. |
| Kein fester Termin / Release-Druck | Es gibt keine externe Deadline. Prioritäten können sich im Projektverlauf verschieben. |
| Nutzung kostenloser bzw. kostengünstiger Dienste | Als Hobbyprojekt sollten wo möglich Free-Tier-Angebote genutzt werden (Hosting, ggf. Datenbank), um laufende Kosten gering zu halten. |

## Politische / rechtliche Randbedingungen

| Randbedingung | Erläuterung |
|---|---|
| Einhaltung der IGDB/Twitch API-Nutzungsbedingungen | Insbesondere Attribution der Datenquelle sowie Einhaltung der Rate-Limits sind Pflicht, nicht optional. |
| Urheberrecht an Spiele-Assets | Cover-Bilder/Artworks stammen von Drittanbietern (Publishern) und werden nur über die IGDB-API referenziert/angezeigt, nicht selbst erstellt oder frei weiterverwendet. |

## Konventionen

*(TODO: sobald festgelegt, hier ergänzen, z.B. Code-Style-Guide,
Commit-Message-Konvention, Branching-Modell – bei einem Solo-Projekt
reicht oft eine einfache, für dich selbst nachvollziehbare Konvention.)*