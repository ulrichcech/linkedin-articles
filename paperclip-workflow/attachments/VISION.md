# VISION.md

## 1. Produktidee

ChronoX ist eine moderne Countdown-App für persönliche Ereignisse, Meilensteine und Ziele.

Die App hilft Nutzern dabei, wichtige Zeitpunkte sichtbar, emotional und motivierend zu verfolgen: Geburtstage, Reisen, Prüfungen, Projektstarts, persönliche Ziele oder besondere Lebensereignisse.

ChronoX soll nicht wie ein nüchterner Kalender wirken, sondern wie ein hochwertiger persönlicher Begleiter für Dinge, auf die man sich freut oder auf die man hinarbeitet.

## 2. Zielgruppe

### Primäre Zielgruppe

- Menschen, die persönliche Ereignisse bewusst verfolgen möchten
- Nutzer, die Vorfreude und Motivation visuell erleben möchten
- Schüler, Studierende und Berufstätige mit wichtigen Deadlines
- Familien, Paare und Einzelpersonen mit besonderen Meilensteinen

### Sekundäre Zielgruppe

- Menschen, die Ziele und Gewohnheiten emotionaler planen möchten
- Nutzer, die minimalistische, schöne Apps bevorzugen
- Personen, die keine komplexe Projektmanagement-App verwenden möchten

## 3. Problem

Viele Countdown-Apps wirken technisch, altmodisch oder überladen. Sie zeigen zwar eine verbleibende Zeit an, erzeugen aber wenig emotionale Bindung.

ChronoX soll diese Lücke schließen: Die App soll Countdowns nicht nur verwalten, sondern sichtbar, schön und motivierend machen.

## 4. Nutzenversprechen

ChronoX bietet dem Nutzer:

- eine schnelle Übersicht über wichtige Ereignisse,
- visuell starke Countdown-Karten,
- eine einfache Möglichkeit, neue Ereignisse anzulegen,
- Kategorien oder visuelle Marker für unterschiedliche Ereignistypen,
- eine lokale, zuverlässige Nutzung ohne Konto und ohne Backend,
- später optionale Widgets, Erinnerungen und Meilenstein-Funktionen.

## 5. Nicht-Ziele für V1

Die erste Version ist bewusst klein gehalten.

Nicht enthalten in V1:

- kein vollständiger Kalenderersatz,
- keine komplexe Projektmanagement-App,
- keine Team-Funktionen,
- kein Backend,
- keine Benutzerkonten,
- keine Cloud-Synchronisierung,
- keine Social-Media-Funktionen,
- keine aufwendige Gamification,
- keine Widgets,
- keine In-App-Käufe.

## 6. Kernfunktionen für V1

V1 enthält nur Funktionen, die nötig sind, um eine einfache, schöne und zuverlässige Countdown-App zu veröffentlichen.

### Muss-Funktionen

- Countdown-Liste
- Countdown-Karte für ein Ereignis
- Ereignis erstellen
- Ereignis bearbeiten
- Ereignis löschen
- Ziel-Datum
- optionale Ziel-Uhrzeit
- Kategorie oder visuelle Markierung
- lokale Speicherung
- ansprechendes App-Design für Android und iOS

### Sollte-Funktionen

- Live-Vorschau beim Erstellen eines Countdowns
- leere Zustände für die erste Nutzung
- verständliche Fehlerzustände bei ungültigem Datum
- einfache Kategorien, z. B. Geburtstag, Reise, Prüfung, Projekt, Sonstiges

### Spätere Ausbaustufen

- Widgets
- Erinnerungen
- Themes
- Templates
- Meilenstein-Ansichten
- Teilen von Countdowns
- Export-Funktionen
- optionale Synchronisierung

## 7. Qualitätskriterien

ChronoX ist gut, wenn:

- ein Countdown in wenigen Sekunden erstellt werden kann,
- die App sofort verständlich ist,
- die Countdown-Karten visuell hochwertig wirken,
- die wichtigsten Ereignisse schnell scanbar sind,
- die App auch ohne Backend zuverlässig funktioniert,
- die UI auf Android und iOS konsistent wirkt,
- die App nicht überladen wirkt,
- der Nutzer emotional versteht: „Das ist mein wichtiges Ereignis.“

## 8. UX-Prinzipien

- Emotional, aber nicht verspielt
- Visuell stark, aber nicht überladen
- Schnell erfassbar
- Wenige Eingaben
- Gute Lesbarkeit vor reiner Ästhetik
- V1 bewusst einfach halten
- Jede Interaktion soll klar und unmittelbar wirken

## 9. Technische Leitplanken

- App: Kotlin Multiplatform / Compose Multiplatform
- Zielplattformen: Android und iOS
- Datenhaltung: lokal auf dem Gerät
- Backend: keines für V1
- Architektur: modular, testbar, erweiterbar
- Design: hochwertige Cards, klare Typografie, moderne Gradients
- Ziel: später erweiterbar für Widgets, Erinnerungen und optionale Synchronisierung

## 10. Veröffentlichungsziel

Ziel ist eine erste öffentlich testbare App-Version, die Countdowns visuell attraktiv und zuverlässig abbildet.

V1 soll zeigen:

- Die Grundidee funktioniert.
- Die App fühlt sich hochwertig an.
- Das Erstellen und Verwalten von Countdowns ist einfach.
- Nutzer verstehen sofort, welchen Wert die App bietet.

## 11. Entscheidungsprinzipien

Wenn zwischen Einfachheit und Funktionsumfang entschieden werden muss, gewinnt für V1 die Einfachheit.

Wenn zwischen visueller Wirkung und Lesbarkeit entschieden werden muss, gewinnt die Lesbarkeit.

Wenn eine Änderung eine Backend-Abhängigkeit einführt, muss sie für V1 besonders begründet werden.

Wenn ein Feature eher „nice to have“ ist, wird es in eine spätere Version verschoben.

Wenn ein Agent unsicher ist, ob ein Feature noch V1 ist oder bereits V2, muss er `MODE: PROPOSE` verwenden oder eine `Human Input Required`-Rückfrage erzeugen.

## 12. Agentenrelevante Leitplanken

Agenten dürfen bei ChronoX nicht eigenmächtig:

- ein Backend einführen,
- Benutzerkonten einführen,
- Cloud-Synchronisierung implementieren,
- ein komplexes Designsystem ersetzen,
- große Architekturwechsel durchführen,
- nicht abgestimmte Premium-Funktionen bauen.

Agenten sollen stattdessen:

- kleine V1-taugliche Lösungen bevorzugen,
- offene Fragen explizit machen,
- Designentscheidungen dokumentieren,
- Implementierungen auf klar abgegrenzte Tasks begrenzen,
- nach Umsetzung Tests und Risiken nennen.
