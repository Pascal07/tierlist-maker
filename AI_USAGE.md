# KI-Einsatz in diesem Projekt

Dieses Dokument legt transparent offen, wie und wofür KI-Tools (z.B. LLMs
wie Claude/ChatGPT) im Rahmen dieses Projekts eingesetzt wurden.

## Kurzfassung

Dieses Projekt ist **nicht "vibe-coded"**. KI wurde gezielt und in einem
reflektierten Rahmen als Lern- und Diskussionswerkzeug eingesetzt –
nicht als Ersatz für eigenständiges Verständnis oder eigenständige
Implementierung.

## Wofür KI eingesetzt wurde

- **Lernunterstützung**: Erklärung von Konzepten, Patterns, Bibliotheken,
  die mir neu waren
- **Diskussion & Abwägung von Architekturentscheidungen**: z.B. Vergleich
  verschiedener Lösungsansätze (Caching-Strategien, API-Design,
  Datenbankmodellierung) – die finale Entscheidung sowie deren
  Begründung stammen von mir (siehe `docs/adr/`)
- **Code-Reviewing**: Gegenlesen von selbst geschriebenem Code auf
  potenzielle Bugs, Edge Cases, Lesbarkeit
- **Vereinzelte Unterstützung bei Boilerplate** (z.B. Config-Dateien,
  sich wiederholende Standard-Strukturen)

## Wofür KI nicht eingesetzt wurde

- Der **Großteil des Codes**, insbesondere **kritische und
  Backend-Logik**, wurde von mir selbst geschrieben.
- **Jede Zeile Code im Repository wurde von mir gelesen, verstanden und
  überprüft** – unabhängig davon, ob sie mit oder ohne KI-Unterstützung
  entstanden ist. Es gibt keinen Code, den ich nicht erklären kann.
- Architektur- und Designentscheidungen wurden von mir getroffen; KI
  diente hier als Sparringspartner, nicht als Entscheidungsinstanz.

## Warum diese Offenlegung

Mir ist es wichtig, dass Code in meinem Portfolio meine tatsächlichen
Fähigkeiten widerspiegelt. Ich nutze KI-Tools so, wie ich sie auch im
beruflichen Alltag einsetzen würde: als Werkzeug zur Beschleunigung von
Recherche, Diskussion und Review – nicht als Blackbox, die
unverstandenen Code produziert.
