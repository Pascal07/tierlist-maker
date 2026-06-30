# ADR-0001: Spiele-Daten über externe API statt eigener Bild-Uploads

## Status
Beschlossen

## Kontext
Nutzer:innen sollen Spiele in eine Tierlist einsortieren können. Ein eigener
Bild-Upload würde Speicherverwaltung, Moderation (z.B. Urheberrecht, NSFW-Content)
und zusätzlichen Storage-Bedarf erfordern.

## Optionen
1. Nutzer laden eigene Bilder hoch
2. Anbindung an externe Spiele-Datenbank-API (z.B. IGDB, RAWG), die Name,
   Cover-Bild und Metadaten liefert

## Entscheidung
Option 2: Anbindung an eine externe Spiele-API.

## Konsequenzen
- (+) Kein eigener Bild-Storage nötig
- (+) Konsistente, korrekte Cover-Bilder
- (+) Geringerer Moderationsaufwand
- (-) Abhängigkeit von Verfügbarkeit/Rate-Limits der externen API
- (-) API-Key-Management notwendig
- Mitigation: Caching der Ergebnisse im eigenen Backend (siehe ADR-0002)
