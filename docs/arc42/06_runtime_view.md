# 6. Laufzeitsicht

Diese Sicht beschreibt die aus Architektursicht relevantesten Abläufe
des Tierlist Makers anhand konkreter Szenarien.

## Spiel suchen und zur Tierlist hinzufügen

Zentraler Ablauf des Systems: Der Nutzer sucht ein Spiel über IGDB und
ordnet es einer Tier-Stufe zu.

```mermaid
sequenceDiagram
    actor Nutzer
    participant FE as Frontend
    participant C as REST-Controller
    participant IS as IGDB-Integration-Service
    participant IGDB as IGDB/Twitch API

    Nutzer->>FE: Suchbegriff eingeben (z.B. "Baldur's Gate 3")
    FE->>C: GET /api/games/search?q=...
    C->>IS: suche(query)
    IS->>IGDB: Suchanfrage (REST)
    IGDB-->>IS: Trefferliste (Titel, Cover-URL, Jahr)
    IS-->>C: normalisierte Trefferliste
    C-->>FE: JSON-Antwort
    FE-->>Nutzer: zeigt Ergebnisse mit Bild an
    Nutzer->>FE: Spiel per Drag & Drop in Tier-Stufe ablegen
    FE-->>Nutzer: aktualisiert Ansicht (lokal, noch nicht gespeichert)
```

**Besonderheiten**: Die Zuordnung per Drag & Drop wird zunächst nur im
Frontend-Zustand gehalten; ein Speichern-Aufruf (siehe nächstes
Szenario) ist ein separater, bewusster Schritt des Nutzers. Der
IGDB-Integration-Service liefert bereits normalisierte Daten
(einheitliches Format), sodass Controller und Frontend nicht wissen
müssen, wie die IGDB-Antwort intern aussieht.

## Tierlist speichern

```mermaid
sequenceDiagram
    actor Nutzer
    participant FE as Frontend
    participant C as REST-Controller
    participant TS as Tierlist-Service
    participant DB as Datenbank

    Nutzer->>FE: Klick auf "Speichern"
    FE->>C: POST /api/tierlists (Titel, Tier-Stufen, Einträge)
    C->>TS: speichereTierlist(daten)
    TS->>TS: validiere Daten (z.B. Pflichtfelder, gültige Stufen)
    TS->>DB: persistiere Tierlist
    DB-->>TS: OK (inkl. generierter ID)
    TS-->>C: gespeicherte Tierlist (mit ID)
    C-->>FE: JSON-Antwort (201 Created)
    FE-->>Nutzer: Bestätigung / Weiterleitung zur gespeicherten Ansicht
```

**Besonderheiten**: Die Validierung liegt bewusst im Tierlist-Service
(nicht im Controller oder Frontend), damit fachliche Regeln zentral an
einer Stelle im Backend geprüft werden, unabhängig davon, über welchen
Weg ein Speichern-Aufruf ausgelöst wird.

## Fehlerfall: IGDB-API nicht erreichbar

Adressiert das Qualitätsziel "Zuverlässigkeit der externen Anbindung"
aus Kapitel 1.

```mermaid
sequenceDiagram
    actor Nutzer
    participant FE as Frontend
    participant C as REST-Controller
    participant IS as IGDB-Integration-Service
    participant IGDB as IGDB/Twitch API

    Nutzer->>FE: Suchbegriff eingeben
    FE->>C: GET /api/games/search?q=...
    C->>IS: suche(query)
    IS->>IGDB: Suchanfrage (REST)
    IGDB--xIS: Timeout / Fehler (z.B. 503, Rate-Limit)
    IS-->>C: definierter Fehler (z.B. GameSearchUnavailableException)
    C-->>FE: HTTP 503 mit verständlicher Fehlermeldung
    FE-->>Nutzer: Hinweis "Spielsuche aktuell nicht verfügbar,\nbitte später erneut versuchen"
```

**Besonderheiten**: Der IGDB-Integration-Service fängt technische
Fehler (Timeout, HTTP-Fehlercodes, Rate-Limit-Überschreitung) ab und
übersetzt sie in eine definierte, fachliche Fehlerantwort. Dadurch
stürzt das Backend nicht ab, und bereits gespeicherte Tierlisten
bleiben unabhängig davon nutzbar, da der Tierlist-Service von diesem
Fehler nicht betroffen ist (Vorteil der Service-Trennung aus Kapitel 4
und 5).

*(TODO: sobald ein Caching im IGDB-Integration-Service umgesetzt ist,
hier ein weiteres Szenario ergänzen, das zeigt, wie bei einem
IGDB-Ausfall zumindest bereits gecachte Suchergebnisse weiter genutzt
werden können.)*