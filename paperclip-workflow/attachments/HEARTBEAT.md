# HEARTBEAT.md

## 1. Zweck

Diese Datei dokumentiert das Heartbeat-Setup für das ChronoX-Projekt. Sie ergänzt `AGENTS.md` (Rollen) und `WORKFLOW.md` (Prozess) um die zeitliche Dimension: wann arbeiten welche Agenten, wie oft, und unter welchen Stop-Bedingungen.

Heartbeats sind ein Paperclip-Primitiv. Den Dateinamen `HEARTBEAT.md` selbst hat **OpenClaw** geprägt — dort als kurze, in den Agent-Prompt geladene Checkliste (siehe [docs.openclaw.ai/gateway/heartbeat](https://docs.openclaw.ai/gateway/heartbeat)). Das hier vorliegende Dokument geht darüber hinaus: es ist kein Prompt-Schnipsel für den Agenten, sondern ein **Setup- und Koordinationsdokument für menschliche Leser** — Taktung, Budgets, Pause-Regeln, Notbremsen. Beide Verwendungen können parallel existieren; bei OpenClaw-Nutzung würde diese Datei z. B. nach `docs/HEARTBEAT-setup.md` wandern und die OpenClaw-Stil-Checkliste den ursprünglichen Namen behalten.

## 2. Globale Regeln

- Heartbeats laufen Mo-Fr, 8:00-20:00 Europe/Berlin.
- Wochenende und Nachtzeit: alle Agenten pausiert (außer manueller Trigger).
- Bei zwei aufeinanderfolgenden Fehlläufen eines Agenten: automatischer Pause-Status, Notification an den Menschen.
- Vor jedem Release: Taktung kurzfristig erhöhbar, aber nur nach Eintrag in `DECISIONS.md`.

## 3. Taktung pro Agent

| Agent | Taktung | Aufgaben pro Heartbeat |
|---|---|---|
| Product Owner Agent | 30 Min | Inbox prüfen, neue Ideen-Tasks aufnehmen, offene Klärungen weiterführen |
| Architect Agent | 60 Min | Anstehende Architektur-Reviews, ADR-Entwürfe, technische Eskalationen |
| Engineer Agent | 10 Min | In-Progress-Tasks fortsetzen, neue EXECUTE-Tasks aufnehmen |
| Review Agent | 30 Min (versetzt zu Engineer) | Review Requests prüfen, Freigaben empfehlen |
| QA Agent | 60 Min | Neue Akzeptanzkriterien, Regressionsfälle, Edge-Case-Tests |
| UX/UI Agent | 30 Min | UX-Iterationen, offene Designvarianten, Spezifikationsupdates |
| Documentation Agent | 10:00, 13:00, 16:00, 19:00 | DECISIONS.md, README, Changelog, neue ADRs |

## 4. Was beim Heartbeat passiert

Bei jedem Heartbeat führt jeder Agent diese Schritte in fester Reihenfolge aus (Status-Werte siehe `WORKFLOW.md` §13):

1. **Inbox prüfen** — neue Zuweisungen, @mentions, Kommentare auf eigenen Tasks.
2. **`Waiting for Human` ignorieren** — der Mensch ist am Zug, kein Eingriff.
3. **`Blocked` prüfen** — Blocker noch aktiv? Ggf. erneut versuchen, sonst Status halten.
4. **`In Progress` fortsetzen** — eigenen Task weiterbearbeiten, falls Fortschritt möglich.
5. **Neue `Ready`-Tasks aufnehmen** — sofern Kapazität (Budget) und Priorität es erlauben.
6. **Stop-Bericht** — wenn nichts zu tun ist: kurzer Statuskommentar im zugewiesenen Inbox-Kanal, kein leerer Lauf.

## 5. Reihenfolge und Abhängigkeiten

- Reviewer-Heartbeat ist um 5 Minuten gegen den Engineer versetzt, damit Review Requests beim nächsten Reviewer-Lauf sicher vorhanden sind.
- Documentation Agent läuft erst nach Engineer und Reviewer, damit nichts dokumentiert wird, was noch nicht durchgereicht ist.
- Bei Architekturänderungen wartet der Engineer auf den Architect-Lauf, bevor er `EXECUTE` aufnimmt.

## 6. Kostenbezug

Monatliche Budgets pro Agent sind in der Paperclip-Konfiguration gesetzt. Die folgenden Zahlen sind **illustrative Beispielwerte** für ein kleines Solo-Projekt wie ChronoX und nicht aus mehreren Monaten Produktiverfahrung kalibriert — eigene Werte können stark abweichen, je nach Modellauswahl, Agent-Backend und Aufgabenkomplexität.

| Agent | Monatsbudget (Beispiel) | Erwartete Auslastung |
|---|---|---|
| Product Owner Agent | $30 | ~50 % |
| Architect Agent | $40 | ~40 % |
| Engineer Agent | $120 | ~70 % |
| Review Agent | $50 | ~50 % |
| QA Agent | $30 | ~30 % |
| UX/UI Agent | $40 | ~40 % |
| Documentation Agent | $20 | ~20 % |

Die obige Taktung wäre so gewählt, dass alle Agenten im Schnitt unter 70 % ihres Budgets bleiben — mit Puffer für Release-Spitzen. Bevor Du Dich auf konkrete Budgets festlegst: erste 1-2 Wochen ohne harte Limits beobachten und dann nachjustieren.

## 7. Pause-Regeln

Was Paperclip von Haus aus mitbringt, was Wunsch / Konvention ist, klar getrennt:

**Out-of-the-box (Paperclip):**

- **Manuell pausierbar** pro Agent über die Paperclip-UI.
- **Auto-Stop am Cost-Limit**: Erreicht ein Agent sein Monatsbudget (100 %), pausiert er und queued Work wird abgebrochen.

**Projekt-Konventionen für ChronoX (zusätzliche Wünsche, die ich über Skripte oder Hooks abbilde):**

- **Frühe Warnung bei 90 % des Monatsbudgets** mit Notification an den Menschen — damit die Wahl bleibt zwischen „Taktung runter" und „Limit erhöhen", bevor der Agent komplett stillsteht.
- **Globaler Pause-Schalter im Repo**: eine Datei wie `.paperclip/pause` (Vorschlag — eigene Konvention, kein Paperclip-Standard). Wenn vorhanden, deaktivieren wir Heartbeats über ein kleines Wrapper-Skript.
- **Auto-Pause bei zwei aufeinanderfolgenden Fehlläufen**: ebenfalls eine eigene Wrapper-Logik. „Fehllauf" definiere ich als Lauf, der mit nicht-null Exit-Code oder mit `HUMAN_INPUT_REQUIRED` ohne neuen Inhalt endet.

Wenn Du Paperclip ohne diese Erweiterungen verwendest, gilt nur der erste Block.

## 8. Notbremse: Schleifen-Erkennung

Auch dieser Mechanismus ist eine **Projekt-Konvention** — Paperclip selbst hat keine eingebaute Schleifen-Erkennung. Bei mir läuft sie als kleines Wrapper-Skript um die Agent-Aufrufe.

Idee: Wenn ein Agent dreimal hintereinander denselben Task ohne Fortschritt anfasst (gleicher Output-Hash bzw. derselbe Statuskommentar), wird:

1. der Task automatisch auf `Blocked` gesetzt,
2. ein `Human Input Required`-Block erzeugt,
3. der Agent für 24 Stunden von diesem Task ausgeschlossen,
4. ein Eintrag in `TASKS.md` unter „blockiert" erzeugt.

So vermeide ich endlose Iterationen, die nur Budget verbrennen, ohne dass etwas vorangeht. Wer die Logik nicht selbst implementieren möchte, ersetzt sie durch eine konservative Taktung plus regelmäßigen Cost-Check.

## 9. Reviewzyklus dieser Datei

- Bei jeder Taktungsänderung: Eintrag in `DECISIONS.md` mit Datum und Begründung.
- Einmal pro Quartal: Abgleich mit den Cost-Reports der Vorquartale, ggf. Anpassung.
- Nach jedem größeren Release: kurze Retro, ob die Taktung noch passt.

## 10. Geltungsbereich

Diese Datei beschreibt nur das Setup für ChronoX. Andere Paperclip-Projekte im Org-Account haben eigene `HEARTBEAT.md`-Dateien mit potenziell anderen Taktungen und Budgets.