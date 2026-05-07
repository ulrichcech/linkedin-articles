# TASKS.md

## 1. Zweck

Diese Datei ist **kein Mirror der Paperclip-Task-Liste**. Paperclip selbst ist die Quelle der Wahrheit für Status, Assignee, Blocker-Dependencies und Kommentarverläufe — diese Datei daneben zu pflegen würde nur Doppelarbeit erzeugen, und beide Quellen würden auseinanderlaufen.

`TASKS.md` enthält stattdessen nur das, was Paperclip **nicht** liefert: ein menschenlesbares Narrativ über das Projekt — was läuft gerade, was kommt als Nächstes, was wurde bewusst zurückgestellt und welche Entscheidungen sind in der jüngeren Vergangenheit gefallen.

## 2. Aktueller Fokus

ChronoX V1 — eine einfache, hochwertige Countdown-App, mit der Nutzer lokale Countdown-Ereignisse erstellen, anzeigen, bearbeiten und löschen können. Aktueller Schwerpunkt: Countdown-Karte (Design + Implementierung) und Create-Flow.

## 3. V1-Nicht-Ziele (Erinnerung)

Spiegelt die Vision — bewusst nicht in V1, damit der Scope klein bleibt:

- kein Backend, keine Benutzerkonten, keine Cloud-Synchronisierung
- keine Widgets, keine Erinnerungen, kein Sharing
- keine komplexe Gamification, keine In-App-Käufe

Wenn ein Agent vorschlägt, eines davon zu bauen: Eskalation gemäß `WORKFLOW.md`.

## 4. Nächste empfohlene Schritte

Kuratierte Reihenfolge — die Status der einzelnen Tasks stehen in Paperclip:

1. Countdown Card Designvarianten erstellen (UX/UI Agent, `MODE: PROPOSE`).
2. Mensch wählt Richtung oder kombiniert Optionen.
3. Konkrete Countdown Card Spezifikation ausarbeiten (UX/UI Agent, `MODE: ITERATIVE`).
4. Lokale Speicherung architektonisch bewerten (Architect Agent, `MODE: PROPOSE`).
5. Implementierungstickets schneiden für Card, Formatter und Create-Flow (Product Owner Agent).

## 5. Zurückgestellt (Post-V1)

Bewusst nicht Teil von V1, mit Begründung — verhindert das immer wiederkehrende „können wir das nicht doch noch …?":

| Thema | Grund |
|---|---|
| Cloud-Synchronisierung | Backend und Konten wären nötig |
| Widgets | V1 soll zuerst App-Kern validieren |
| Erinnerungen | zusätzlicher Plattformaufwand |
| Themes | erst nach stabiler UI-Basis sinnvoll |
| Sharing | nicht notwendig für V1 |
| Premium-Modell | erst nach Produktvalidierung |

## 6. Entscheidungslog (Kurzform)

Knapper Verlauf als schnelle Übersicht. Längere Begründungen liegen in `docs/decisions/YYYY-MM-DD-*.md`:

| Datum | Entscheidung |
|---|---|
| 2026-04-30 | ChronoX bleibt in V1 backendfrei |
| 2026-04-30 | Task-Modes (`PROPOSE`/`ITERATIVE`/`EXECUTE`) werden verbindlich genutzt |
| 2026-04-30 | Design-Spezifikationen liegen unter `docs/design/` |

## 7. Pflegehinweis

Diese Datei wird **nicht bei jedem Task-Update angefasst**. Sinnvolle Anlässe:

- Wenn der aktuelle Fokus wechselt (typischerweise alle ein bis zwei Wochen).
- Wenn eine größere Entscheidung gefallen ist (Eintrag in §6).
- Wenn ein neues Post-V1-Thema dazukommt (Eintrag in §5).
- Wenn die Reihenfolge der nächsten Schritte sich ändert (§4).

Pro Heartbeat-Zyklus genügt es, hier nichts zu tun. Wer sucht, was *gerade* in Arbeit ist, schaut in Paperclip.
