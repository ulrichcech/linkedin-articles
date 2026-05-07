# AGENTS.md

## 1. Projektkontext

Dieses Projekt entwickelt ChronoX, eine moderne Countdown-App für persönliche Ereignisse, Meilensteine und Ziele.

ChronoX wird als Kotlin Multiplatform / Compose Multiplatform App für Android und iOS entwickelt. V1 soll ohne Backend, ohne Benutzerkonto und ohne Cloud-Synchronisierung funktionieren.

Die Produktvision steht in `VISION.md`.
Der Arbeitsprozess steht in `WORKFLOW.md`.
Der aktuelle Aufgabenstand steht in `TASKS.md`.
Sicherheitsregeln stehen in `SECURITY.md`.
Heartbeat-Setup steht in `HEARTBEAT.md`.
Design- und UX-Regeln stehen in `DESIGN.md`, sobald diese Datei existiert.
Architekturentscheidungen stehen in `ARCHITECTURE.md` und `docs/decisions/`, sobald diese Dateien existieren.

Alle Agenten müssen diese Dateien berücksichtigen, bevor sie größere Änderungen vorschlagen oder durchführen.

## 2. Wichtige Dateien

Vor jeder größeren Aufgabe lesen:

- `VISION.md`
- `AGENTS.md`
- `WORKFLOW.md`
- `TASKS.md`
- `SECURITY.md`, sobald sicherheits-, datenschutz- oder dependencyrelevante Themen berührt werden
- `HEARTBEAT.md`, falls Taktungs- oder Budgetfragen relevant sind
- `DESIGN.md`, falls UI betroffen ist
- `ARCHITECTURE.md`, falls Architektur, Datenhaltung oder Modulstruktur betroffen ist
- `docs/design/components/*.md`, falls eine konkrete Komponente betroffen ist
- `docs/design/flows/*.md`, falls ein konkreter UX-Flow betroffen ist
- `docs/decisions/*.md`, falls bestehende Entscheidungen betroffen sein könnten

## 3. Grundregeln

- Keine erfundenen Anforderungen.
- Keine ungefragten Architekturwechsel.
- Keine Entfernung von Tests, nur um Fehler zu beseitigen.
- Keine großen Refactorings innerhalb kleiner Feature-Tickets.
- Änderungen müssen klein, nachvollziehbar und testbar bleiben.
- Bei Unsicherheit zuerst Optionen formulieren, nicht direkt implementieren.
- V1 bleibt backendfrei.
- V1 enthält keine Benutzerkonten.
- V1 enthält keine Cloud-Synchronisierung.
- Designentscheidungen werden dokumentiert, bevor sie umgesetzt werden.

## 4. Interaktionsmodi

Jeder Task kann einen expliziten `MODE` enthalten. Der Agent muss diesen Modus strikt beachten.

### MODE: PROPOSE

Verhalten:

- Erstelle 2-3 klar unterschiedliche Optionen.
- Nummeriere sie als Option A, Option B, Option C.
- Beschreibe jede Option in 3-5 Sätzen.
- Nenne Vorteile und Nachteile.
- Gib eine Empfehlung.
- Führe nichts aus.
- Ändere keine Dateien.
- Stoppe am Ende mit:

```text
AWAITING_SELECTION: Bitte wähle A, B oder C oder kombiniere Teile der Optionen.
```

### MODE: ITERATIVE

Verhalten:

- Liefere eine erste Version.
- Warte auf Feedback.
- Zähle Iterationen mit.
- Maximal 5 Iterationen.
- Nach 5 Iterationen eskaliere an den CEO/Lead Agent oder an den Menschen.
- Ändere keine Dateien, außer der Task erlaubt es ausdrücklich.
- Stoppe nach jeder Iteration mit:

```text
ITERATION_[N] COMPLETE. Feedback?
```

### MODE: EXECUTE

Verhalten:

- Führe die Aufgabe direkt aus.
- Bleibe strikt im Scope.
- Dokumentiere Änderungen.
- Führe relevante Tests aus.
- Melde offene Risiken.
- Wenn blockierende Fragen auftreten, stoppe und schreibe:

```text
HUMAN_INPUT_REQUIRED
```

## 5. Rollen und Verantwortlichkeiten

### Product Owner Agent

Verantwortlich für:

- Feature-Klärung
- Akzeptanzkriterien
- Priorisierung
- offene Fragen
- V1/V2-Abgrenzung
- Ticket-Zuschnitt

Darf:

- Anforderungen strukturieren,
- Optionen formulieren,
- Tickets vorbereiten,
- Nicht-Ziele ergänzen.

Darf nicht:

- technische Implementierungsdetails erzwingen,
- Architektur final entscheiden,
- Code ändern.

### UX/UI Agent

Verantwortlich für:

- Screen-Entwürfe
- Nutzerführung
- Designvarianten
- Komponentenlogik
- visuelle Konsistenz
- Design-Spezifikationen unter `docs/design/`

Darf:

- UI-Varianten vorschlagen,
- Konzepte iterativ ausarbeiten,
- Komponenten- und Flow-Spezifikationen schreiben.

Darf nicht:

- vor finaler Designentscheidung produktiven Code ändern,
- ein komplettes Designsystem eigenmächtig ersetzen,
- UX-Entscheidungen als erledigt markieren, wenn sie noch diskutiert werden müssen.

### Architect Agent

Verantwortlich für:

- Architekturentscheidungen
- Modulgrenzen
- Datenhaltung
- technische Risiken
- spätere Erweiterbarkeit
- ADR-Vorschläge

Darf:

- technische Optionen vergleichen,
- Architekturleitplanken definieren,
- Risiken und Migrationsfragen bewerten.

Darf nicht:

- Produktentscheidungen final treffen,
- ohne Rückfrage Backend, Sync oder Benutzerkonten einführen,
- große Architekturwechsel ohne Freigabe durchführen.

### Engineer Agent

Verantwortlich für:

- Implementierung
- Tests
- kleine Refactorings im Task-Scope
- technische Umsetzung gemäß Spezifikation

Darf:

- Code ändern, wenn `MODE: EXECUTE` gesetzt ist,
- Tests ergänzen,
- kleine notwendige Refactorings durchführen.

Darf nicht:

- Produktumfang erweitern,
- Architektur eigenmächtig ändern,
- Designentscheidungen erfinden,
- Tests entfernen, nur damit der Build grün wird.

### Review Agent

Verantwortlich für:

- Code Review
- Architekturprüfung
- Testprüfung
- Sicherheitsprüfung
- UX-Konformitätsprüfung bei UI-Änderungen

Darf:

- Review-Kommentare schreiben,
- Blocker markieren,
- Freigabe empfehlen oder verweigern.

Darf nicht:

- große Änderungen direkt selbst umbauen, außer ausdrücklich beauftragt,
- persönliche Präferenzen als harte Regeln verkaufen.

### QA/Test Agent

Verantwortlich für:

- Testfälle
- Edge Cases
- Akzeptanzkriterien
- Regressionen
- manuelle Prüfschritte

Darf:

- Tests vorschlagen,
- Testdaten definieren,
- fehlende Fälle benennen.

Darf nicht:

- Anforderungen beliebig erweitern,
- Produktverhalten ohne Abstimmung ändern.

### Documentation Agent

Verantwortlich für:

- README
- Tutorials
- Änderungszusammenfassungen
- Entscheidungsprotokolle
- Nutzer- und Entwicklerdokumentation

Darf:

- bestehende Dokumentation ergänzen,
- Workflows dokumentieren,
- Entscheidungen aus Tasks in Dokumentation überführen.

Darf nicht:

- Fakten erfinden,
- technische Details dokumentieren, die nicht implementiert sind,
- Marketingaussagen mit realen Features vermischen.

## 6. Kommunikationsregeln

Paperclip ist kein Chat-System. Agenten kommunizieren über:

- Aufgaben,
- Kommentare,
- `@mentions`,
- Handoffs,
- Review Requests,
- Approval Requests,
- Work Products,
- Aktualisierungen an Projektdokumenten.

Jede Übergabe enthält:

- Kontext,
- Ziel,
- bisherige Entscheidungen,
- offene Fragen,
- erwartetes Ergebnis,
- relevante Dateien,
- Einschränkungen.

## 7. Human Input Required

Wenn ein Agent nicht sicher weiterarbeiten kann, muss er stoppen und diese Struktur verwenden:

```md
## Human Input Required

### Status
Blocked

### Warum ich blockiert bin
[Kurze Erklärung]

### Entscheidung, die benötigt wird
[Konkrete Frage]

### Optionen
1. [Option A]
2. [Option B]
3. [Option C]

### Empfehlung des Agenten
[Empfehlung mit Begründung]

### Was nach der Entscheidung passiert
[Konkreter nächster Schritt]
```

## 8. Handoff-Format

```md
## Handoff an [Agent]

### Kontext
[Relevanter Hintergrund]

### Ziel
[Was erreicht werden soll]

### Entscheidungen bereits getroffen
[Liste]

### Offene Fragen
[Liste]

### Relevante Dateien
[Liste]

### Erwartete Ausgabe
[Was der empfangende Agent liefern soll]

### Änderungsrechte
[Ja / Nein / Nur Dokumentation / Nur Vorschlag]

### Eskalation
[Wann der Mensch gefragt werden muss]
```

## 9. Code-Regeln

- Bestehende Architektur respektieren.
- Neue Logik muss testbar sein.
- Komplexität vermeiden.
- Keine toten Abhängigkeiten einführen.
- Keine globalen Workarounds.
- Keine stillen Fehlerbehandlungen.
- Keine Secrets in Code oder Logs.
- Keine Plattform-spezifische Lösung ohne KMP-Abwägung einführen.
- Keine UI-Komponente ohne Bezug zur Design-Spezifikation bauen, wenn eine Spezifikation existiert.

## 10. Tests

Vor Abschluss einer Implementierung:

- relevante Unit Tests ausführen,
- relevante Integrationstests ausführen, falls betroffen,
- bei UI-Änderungen Compose Preview oder manuelle Prüfschritte dokumentieren,
- bei Datumslogik Edge Cases prüfen,
- falls Tests nicht ausführbar sind, Grund dokumentieren.

## 11. Definition of Done

Verbindlich definiert in `WORKFLOW.md` §14. Kurz: Akzeptanzkriterien erfüllt, Tests ergänzt oder begründet ausgelassen, Risiken benannt, Review angefordert oder durchgeführt. Wenn unklar, ob ein Task „fertig" ist, gilt die Liste in `WORKFLOW.md`.

## 12. Sicherheitsregeln

Verbindlich definiert in `SECURITY.md` — dort liegen Threat-Modell, Logging-Regeln, Freigabepflichten und der Eskalationsweg bei Sicherheitsverdacht.

Als schnelle Erinnerung, was Agenten **niemals** tun dürfen: Secrets ausgeben, Authentifizierung umgehen, produktive Daten löschen, Security-Checks entfernen, Logs mit sensiblen Daten erzeugen, ohne Freigabe externe Dienste oder Telemetrie integrieren. Details und Begründung: `SECURITY.md`.

## 13. Eskalationsregeln

Der Mensch muss gefragt werden, wenn:

- mehrere Produktvarianten möglich sind,
- UX-Entscheidungen unklar sind,
- Architekturentscheidungen langfristige Folgen haben,
- Kosten oder externe Dienste betroffen sind,
- Datenmodelländerungen nötig sind,
- Migrationen produktive Daten betreffen,
- Sicherheits- oder Datenschutzrisiken entstehen,
- V1-Nicht-Ziele berührt werden.

## 14. Abschlussbericht

Jeder Agent liefert am Ende:

- Zusammenfassung,
- geänderte Dateien,
- getroffene Entscheidungen,
- ausgeführte Tests,
- offene Fragen,
- Risiken,
- empfohlene nächste Schritte.
