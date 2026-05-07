# SECURITY.md

## 1. Zweck

Diese Datei hält die sicherheitsrelevanten Spielregeln für das ChronoX-Projekt fest. Sie ist die einzige Quelle der Wahrheit für Sicherheits- und Datenschutzthemen — andere Dateien (insbesondere `AGENTS.md`) verweisen darauf, anstatt Regeln zu duplizieren.

ChronoX ist eine lokale Countdown-App ohne Backend in V1. Das Threat-Modell ist deshalb klein, aber nicht leer: es geht vor allem darum, **versehentliche Datenpreisgabe**, **eingeschleuste Telemetrie** und **unbedachte externe Abhängigkeiten** zu verhindern.

## 2. Threat-Modell für V1

- App läuft lokal auf Android und iOS, ohne Login, ohne Backend.
- Daten (Countdown-Ereignisse) liegen lokal auf dem Gerät.
- Es gibt keine Nutzer-Authentifizierung und keine Übermittlung an externe Server.

Daraus folgen drei Hauptrisiken:

1. **Versehentliche Telemetrie oder Tracking** — z. B. wenn ein Agent eine Analytics-Bibliothek einführt, die Geräte-IDs oder Event-Titel sendet.
2. **Unbedachte externe Abhängigkeiten** — eine neue Library mit eigenem Netzwerkzugriff, die in V1 nichts zu suchen hat.
3. **Lecks lokaler Daten** — z. B. durch Logging von Event-Titeln, die persönliche Inhalte (Geburtstage, Prüfungstermine) enthalten können.

## 3. Was Agenten niemals tun dürfen

- Secrets, API-Keys oder Tokens in Dateien, Logs oder Commit-Nachrichten schreiben.
- Authentifizierung umgehen (auch nicht „nur für lokale Tests").
- Produktive Daten löschen.
- Security-Checks oder Lints entfernen, um einen Build grün zu bekommen.
- Logs mit personenbezogenen oder potenziell sensiblen Inhalten erzeugen (Event-Titel, Notizen, Standortinformationen).
- Ohne menschliche Freigabe externe Dienste integrieren — auch keine vermeintlich harmlosen Analytics, Crash-Reporter oder Feature-Flag-Services.
- Ohne Freigabe Telemetrie oder Tracking einbauen.

## 4. Was menschliche Freigabe braucht

Ein Agent darf erst nach explizitem `Approval Request` und Freigabe loslegen, wenn:

- eine neue externe Bibliothek eingeführt werden soll (besonders mit Netzwerkzugriff oder Persistenz),
- eine bestehende Bibliothek auf eine Major-Version aktualisiert wird,
- Datenmodell-Änderungen produktive Daten betreffen (Migrationen),
- eine Funktion Daten ans Gerät externer Stellen übermittelt (Sharing, Export, Cloud),
- eine Berechtigung im Manifest oder `Info.plist` ergänzt werden soll,
- Telemetrie, Crash-Reporting oder ähnliches eingeführt werden soll.

## 5. Logging-Regeln

- Keine Event-Titel, Beschreibungen oder Notizen in Logs.
- Keine Datums-/Zeitangaben, die ein bestimmtes Ereignis identifizieren könnten, in produktiven Logs.
- IDs sind in Ordnung, sofern sie keine Rückschlüsse auf Inhalte erlauben.
- Bei Crashlogs: vor dem Versand prüfen, dass keine Nutzerdaten enthalten sind. (V1 versendet ohnehin keine Crashlogs — sobald sich das ändert, gilt §4.)

## 6. Eskalationsweg bei Sicherheitsverdacht

Wenn ein Agent einen Sicherheitsverdacht hat — z. B. eine verdächtige Library, eine ungewöhnliche Berechtigungsanforderung, ein potenzielles Datenleck:

1. **Sofort stoppen** — keine weitere Umsetzung.
2. **Status auf `Blocked` setzen** und einen `Human Input Required`-Block erzeugen.
3. **Im Block beschreiben**: Was wurde beobachtet, welche Dateien sind betroffen, welche Risiken vermutet, was ist die empfohlene Maßnahme.
4. **Niemand außer dem Menschen darf den Block aufheben.**

## 7. Reviewzyklus dieser Datei

- Bei jeder neuen externen Abhängigkeit: SECURITY.md prüfen und ggf. ergänzen.
- Vor jedem Release: kurze Durchsicht, ob die Threat-Modell-Annahmen noch stimmen.
- Sobald V1 ein Backend, Konten oder Sync bekommt: vollständige Überarbeitung — die heutigen Annahmen gelten dann nicht mehr.
