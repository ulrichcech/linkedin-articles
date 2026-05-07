# WORKFLOW.md

## 1. Zweck

Dieses Dokument beschreibt, wie Mensch und Agenten im ChronoX-Projekt zusammenarbeiten.

ChronoX ist eine moderne Countdown-App für persönliche Ereignisse, Meilensteine und Ziele. V1 soll klein, hochwertig und backendfrei bleiben.

Der Workflow soll verhindern, dass Agenten zu früh implementieren, Anforderungen erfinden oder unkontrolliert große Änderungen erzeugen.

## 2. Grundprinzip

Paperclip wird nicht als Chat-System verwendet, sondern als strukturierter Arbeitsprozess.

Kommunikation findet über diese Arbeitsobjekte statt:

- Tasks
- Kommentare
- `@mentions`
- Handoffs
- Review Requests
- Approval Requests
- Work Products
- Projektdokumente

Jede relevante Entscheidung muss sichtbar und nachvollziehbar dokumentiert werden.

## 3. Standard-Task-Struktur

Jeder Task sollte möglichst mit einem expliziten Modus beginnen:

```md
MODE: [PROPOSE | ITERATIVE | EXECUTE]

TASK:
[Was soll der Agent tun?]

KONTEXT:
[Warum ist die Aufgabe wichtig? Was ist bereits entschieden?]

ERWARTETE AUSGABE:
[Was soll konkret zurückkommen?]

EINSCHRÄNKUNGEN:
[Was darf nicht getan werden?]

RELEVANTE DATEIEN:
[Welche Dateien soll der Agent beachten?]

ABSCHLUSS:
[Wie soll der Agent stoppen oder berichten?]
```

## 4. Task-Modes

### 4.1 MODE: PROPOSE

Nutzen, wenn:

- ein Feature noch unklar ist,
- Designvarianten benötigt werden,
- Architekturentscheidungen vorbereitet werden,
- der Mensch eine Auswahl treffen soll.

Der Agent:

- erstellt 2-3 Optionen,
- nennt Vorteile und Nachteile,
- gibt eine Empfehlung,
- ändert keine Dateien,
- stoppt mit `AWAITING_SELECTION`.

Pflichtabschluss:

```text
AWAITING_SELECTION: Bitte wähle A, B oder C oder kombiniere Teile der Optionen.
```

### 4.2 MODE: ITERATIVE

Nutzen, wenn:

- eine Richtung gewählt wurde,
- Details aber noch im Ping-Pong ausgearbeitet werden,
- UI/UX, Texte oder Spezifikationen iterativ entstehen sollen.

Der Agent:

- liefert eine erste Version,
- wartet auf Feedback,
- zählt Iterationen,
- eskaliert nach maximal 5 Iterationen,
- stoppt nach jeder Runde mit `ITERATION_[N] COMPLETE. Feedback?`.

### 4.3 MODE: EXECUTE

Nutzen, wenn:

- Ziel und Scope klar sind,
- Akzeptanzkriterien vorhanden sind,
- keine offenen Produkt- oder Architekturentscheidungen bestehen.

Der Agent:

- führt direkt aus,
- bleibt im Scope,
- dokumentiert Änderungen,
- führt relevante Tests aus,
- erzeugt bei Blockern `Human Input Required`.

## 5. Standard-Workflow: Feature von Idee bis Review

> **Hinweis zur zeitlichen Abfolge:** Die Schritte unten sind logisch, nicht synchron. Jeder Agent reagiert beim **nächsten Heartbeat** (siehe `HEARTBEAT.md`) — zwischen „Mensch erstellt Task" und „PO-Agent antwortet" können je nach Taktung wenige Minuten bis eine Stunde liegen. Wer den Ablauf liest und sich wundert, warum nicht sofort etwas passiert: das ist gewollt und kostensparend.

### Schritt 1: Idee formulieren

Der Mensch beschreibt die Idee, aber noch keine Umsetzung.

Beispiel:

```md
Ich möchte, dass Nutzer in ChronoX ein neues Countdown-Ereignis anlegen können.
Ich bin mir aber noch nicht sicher, wie reduziert oder emotional der Create-Flow aussehen soll.
```

### Schritt 2: Product Owner Agent arbeitet im PROPOSE-Modus

```md
MODE: PROPOSE

TASK:
Analysiere die Feature-Idee „Create Countdown“.

ERWARTETE AUSGABE:
- 3 Produktvarianten
- Nutzen je Variante
- Aufwand je Variante
- Risiken
- Empfehlung für V1

EINSCHRÄNKUNGEN:
- kein Code
- keine finale Entscheidung
```

### Schritt 3: Mensch entscheidet

Der Mensch wählt eine Richtung oder kombiniert Optionen.

Beispiel:

```md
Entscheidung: Variante 2, aber ohne Templates.
Für V1 reicht:
- Titel
- Ziel-Datum
- optionale Ziel-Uhrzeit
- Kategorie
- Vorschau der Countdown-Karte
```

### Schritt 4: UX/UI Agent arbeitet im ITERATIVE-Modus

```md
MODE: ITERATIVE

TASK:
Konkretisiere den Create-Countdown-Flow.

ERWARTETE AUSGABE:
- Screen-Struktur
- Eingabefelder
- Zustände
- Fehlerfälle
- Vorschauverhalten
- Akzeptanzkriterien

EINSCHRÄNKUNGEN:
- kein Code
- nach jeder Iteration Feedback abwarten
```

### Schritt 5: Architect Agent prüft im PROPOSE-Modus

```md
MODE: PROPOSE

TASK:
Prüfe technische Optionen für lokale Speicherung in Kotlin Multiplatform.

ERWARTETE AUSGABE:
- 2 technische Optionen
- Vor- und Nachteile
- Migrationsrisiken
- spätere Sync-Fähigkeit
- V1-Empfehlung

EINSCHRÄNKUNGEN:
- kein Code
- kein Backend vorschlagen, außer als spätere Option
```

### Schritt 6: Product Owner oder Lead Agent erstellt Ticket

```md
MODE: EXECUTE

TASK:
Erstelle aus UX-Konzept und Architekturentscheidung ein implementierbares Ticket.

ERWARTETE AUSGABE:
- Titel
- Ziel
- Nutzerwert
- Akzeptanzkriterien
- Nicht-Ziele
- technische Hinweise
- Testfälle
- Definition of Done
```

### Schritt 7: Engineer Agent implementiert

```md
MODE: EXECUTE

TASK:
Setze das Ticket um.

REGELN:
- Scope nicht erweitern
- Design-Spezifikation beachten
- Architektur beachten
- Tests ergänzen
- bei offenen Entscheidungen stoppen
```

### Schritt 8: Review Agent prüft

```md
MODE: EXECUTE

TASK:
Reviewe die Änderungen.

PRÜFE:
- Akzeptanzkriterien
- Architektur
- Tests
- UX-Konformität
- Wartbarkeit
- unnötige Komplexität
```

### Schritt 9: Mensch entscheidet final

Der Mensch entscheidet über Freigabe, Korrektur oder Folgetickets.

## 6. Direkte Agentenzuweisung vs. Lead-Delegation

### Direkte Agentenzuweisung

Geeignet für:

- isolierte Exploration,
- Designvarianten,
- kleine Spezifikationen,
- Fragen an einen Spezialisten.

Beispiel:

```text
Mensch -> UX/UI Agent
MODE: PROPOSE
TASK: Erstelle 3 visuelle Richtungen für die Countdown-Karte.
```

### Delegation über CEO/Lead Agent

Geeignet für:

- Features mit mehreren Agenten,
- Umsetzung nach abgestimmter Spezifikation,
- Review- und QA-Prozesse,
- Architekturentscheidungen,
- Arbeit mit Abhängigkeiten.

Beispiel:

```text
Mensch -> CEO/Lead Agent
MODE: EXECUTE
TASK: Setze die abgestimmte Countdown Card um.
INPUT: docs/design/components/countdown-card.md
```

## 7. Human-Agent Communication

Menschen kommunizieren mit Agenten durch:

- neue Tasks,
- Kommentare,
- Antworten auf `Human Input Required`,
- Approval oder Rejection,
- Priorisierung,
- Statusentscheidungen.

Menschen sollten vermeiden:

- „looks good?“
- „mach es schöner“
- „entscheide du“
- „bau das mal schnell“

Besser:

- „Choose option B.“
- „Proceed with implementation.“
- „Do not implement yet. Create a smaller V1 proposal.“
- „Approved with changes: ...“
- „Rejected because: ...“

## 8. Agent-Agent Communication

Agenten kommunizieren durch:

- Task-Zuweisung,
- `@mentions`,
- Handoff-Kommentare,
- Review Requests,
- Work Products,
- Updates an Projektdokumenten.

Agenten müssen immer genug Kontext liefern, damit der empfangende Agent ohne Raten weiterarbeiten kann.

## 9. Blocking Questions

Wenn ein Agent nicht sicher weiterarbeiten kann, muss er:

1. Umsetzung stoppen,
2. Task-Status auf `Blocked` oder `Waiting for Human` setzen (siehe §13),
3. `Human Input Required` schreiben,
4. Optionen und Empfehlung liefern,
5. erst nach Antwort weiterarbeiten.

Format:

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

## 10. Review Workflow

Keine relevante Implementierung sollte ohne Review abgeschlossen werden.

Standardfluss:

```text
Engineer Agent -> Review Agent -> Menschliche Freigabe
```

Review Request:

```md
## Review Request

### Ticket
[Name]

### Umsetzung
[Kurze Zusammenfassung]

### Geänderte Dateien
- ...

### Ausgeführte Tests
- ...

### Bekannte Einschränkungen
- ...

### Bitte prüfen
- Akzeptanzkriterien
- Architektur
- Tests
- UX-Konformität
- Wartbarkeit

### Gewünschtes Ergebnis
Review mit:
- Blocker
- Should Fix
- Nice to Have
- Freigabeempfehlung
```

## 11. Dokumentationsregeln

Wichtige Ergebnisse sollen aus Kommentarverläufen in stabile Dateien überführt werden.

Beispiele:

- Designentscheidung -> `docs/design/components/countdown-card.md`
- Architekturentscheidung -> `docs/decisions/YYYY-MM-DD-title.md`
- Feature-Zuschnitt -> `TASKS.md`
- Workflow-Regel -> `WORKFLOW.md`

## 12. No Hidden Decisions

Agenten dürfen keine versteckten Entscheidungen treffen.

Dokumentationspflichtig sind insbesondere:

- Produktumfang,
- Architekturänderungen,
- Datenmodelländerungen,
- externe Bibliotheken,
- Datenschutz- und Security-Aspekte,
- UI/UX-Entscheidungen,
- Abweichungen von V1-Nicht-Zielen.

## 13. Task-Status

Paperclip selbst gibt kein verbindliches Status-Modell vor. Für ChronoX gilt deshalb folgende Konvention. **Diese Liste ist die einzige verbindliche Status-Quelle im Projekt** — andere Bezeichnungen oder Schreibweisen (z. B. UPPERCASE) sollen in Tasks, Kommentaren und in `TASKS.md` nicht verwendet werden.

- `Backlog` — bekannt, aber Voraussetzungen fehlen oder nicht priorisiert.
- `Ready` — ausreichend klar, kann aufgenommen werden.
- `In Progress` — wird aktiv bearbeitet (Agent oder Mensch).
- `Waiting for Human` — Agent hat geliefert oder eine Rückfrage gestellt; der Mensch ist am Zug.
- `Blocked` — kann nicht weiterlaufen (Abhängigkeit, fehlende Entscheidung, technisches Problem). Pflicht: `Human Input Required`-Block oder Verweis auf blockierenden Task.
- `In Review` — Umsetzung abgeschlossen, Review läuft.
- `Done` — Akzeptanzkriterien erfüllt, Review abgeschlossen, Mensch hat freigegeben.
- `Deferred` — bewusst auf später verschoben (z. B. Post-V1).

Übergänge:

```text
Backlog
  → Ready             (Voraussetzungen erfüllt, kann gestartet werden)
  → Deferred          (auf später verschoben)

Ready
  → In Progress       (Agent nimmt Arbeit auf)

In Progress
  → Waiting for Human (Agent liefert Vorschlag / stellt Frage)
  → Blocked           (technisches oder fachliches Hindernis)
  → In Review         (Umsetzung fertig, Review angefordert)

Waiting for Human
  → In Progress       (nach Antwort des Menschen)

Blocked
  → In Progress       (Blocker aufgelöst)

In Review
  → In Progress       (Blocker → zurück an Engineer)
  → Done              (freigegeben)
```

Wichtig: Der Status ist kein Selbstzweck — er muss immer dem aktuellen Zustand entsprechen. Wenn ein Agent eine Rückfrage stellt, gehört der Status auf `Waiting for Human`, sonst geht die Rückfrage in der Liste unter.

## 14. Definition of Done für Tasks

Ein Task ist nur fertig, wenn:

- Akzeptanzkriterien erfüllt sind,
- Nicht-Ziele eingehalten wurden,
- relevante Tests ergänzt oder angepasst wurden,
- Tests ausgeführt oder begründet nicht ausgeführt wurden,
- geänderte Dateien genannt wurden,
- offene Risiken genannt wurden,
- Review durchgeführt oder angefordert wurde.
