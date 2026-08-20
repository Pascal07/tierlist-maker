# 1. Einführung und Ziele

Beschreibt die wesentlichen Anforderungen und treibenden Kräfte, die bei
der Umsetzung der Softwarearchitektur und Entwicklung des Systems
berücksichtigt werden müssen.

## Aufgabenstellung

Tierlist Maker ist eine Webanwendung, mit der Nutzer einfach und schnell
Tierlisten (Rankings) zu beliebigen Themen erstellen können – z.B.
"Best Games of the Year", "Beste Konsolen aller Zeiten" o.ä.

Im Gegensatz zu bestehenden Tierlist-Tools müssen Nutzer keine Bilder
manuell suchen, screenshotten und hochladen. Stattdessen wird die
[IGDB](https://www.igdb.com/) (Internet Game Database) API angebunden:
Nutzer können Spiele direkt über eine Suche finden, und Titel sowie
Cover-/Artwork-Bild werden automatisch übernommen.

Kernfunktionen (MVP):

- Erstellen einer neuen Tierlist mit frei definierbaren Tier-Stufen
  (z.B. S, A, B, C, D)
- Suche nach Spielen über die IGDB-Datenbank (Titel, Cover-Bild,
  Release-Jahr)
- Hinzufügen gefundener Spiele per Drag & Drop in die gewünschte Stufe
- Speichern / Teilen der fertigen Tierlist

*(Optional/später: weitere Kategorien über IGDB hinaus, Export als
Bild, Teilen-Link, eigene Uploads als Fallback)*

**Motivation**: Der eigentliche Beweggrund ist, den größten
Reibungspunkt bestehender Tierlist-Maker zu beseitigen – das mühsame
manuelle Beschaffen von Bildmaterial. Durch die IGDB-Anbindung wird die
Erstellung einer Gaming-Tierlist auf wenige Klicks reduziert.

*(TODO: Verweis auf ggf. vorhandene weitere Anforderungsdokumente
ergänzen, falls vorhanden.)*

## Qualitätsziele

| Priorität | Qualitätsziel | Szenario |
|---|---|---|
| 1 | **Benutzerfreundlichkeit** | Ein neuer Nutzer kann ohne Anleitung innerhalb weniger Minuten eine erste Tierlist mit mind. 5 Spielen erstellen. |
| 2 | **Performance der Suche** | Eine IGDB-Suchanfrage liefert Ergebnisse (inkl. Bild) in unter 1 Sekunde bei normaler Internetverbindung. |
| 3 | **Zuverlässigkeit der externen Anbindung** | Bei Nichterreichbarkeit der IGDB-API erhält der Nutzer eine verständliche Fehlermeldung statt eines Absturzes; bestehende, bereits geladene Tierlisten bleiben nutzbar. |
| 4 | **Wartbarkeit** | Neue Kategorien (z.B. Filme, Musik über andere APIs) sollen sich mit überschaubarem Aufwand ergänzen lassen. |
| 5 | **Portabilität / Zugänglichkeit** | Die Anwendung funktioniert im Browser auf Desktop und mobil, ohne Installation. |

*(TODO: Prioritäten ggf. anpassen, sobald klar ist, was dir für den
ersten Release am wichtigsten ist – z.B. eher Speed of Development als
volle Robustheit, wenn es zunächst nur ein Hobbyprojekt für dich selbst
ist.)*

## Stakeholder

| Rolle | Kontakt | Erwartungshaltung |
|---|---|---|
| Entwickler (Pascal) | Projektinhaber | Klare, wartbare Architektur; Projekt soll trotz begrenzter Zeit voranschreiten; Lernziel arc42/Architekturdokumentation. |
| Nutzer / Gamer | – | Einfache, schnelle Erstellung von Tierlisten ohne manuellen Bildaufwand; intuitive Bedienung. |
| IGDB / Twitch API (externer Dienst) | [api-docs.igdb.com](https://api-docs.igdb.com/) | Einhaltung der API-Nutzungsbedingungen und Rate-Limits; korrekte Attribution der Datenquelle. |

*(TODO: weitere Stakeholder ergänzen, sobald relevant, z.B. falls das
Projekt später öffentlich betrieben wird – dann evtl. Hosting-Provider,
ggf. Mitentwickler.)*