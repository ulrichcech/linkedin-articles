# Effektiv arbeiten mit Paperclip

Ein praktisches Tutorial für produktive Multi-Agent-Workflows — anhand des fiktiven Beispielprojekts **ChronoX**, einer Countdown-App für persönliche Ereignisse.

## Was ist das?

Wer mit [Paperclip](https://paperclip.ing/) anfängt, merkt schnell: möglichst viele Agenten anzulegen löst das Problem nicht. Es löst sich erst, wenn die Agenten **Vision, Rollen, Modi, Übergaben und Reviews** vor sich haben — die gleichen Dinge, die ein kleines Team auch braucht.

Dieses Repo enthält:

- ein **Tutorial** (deutsch, ca. 60 Seiten), das den Aufbau Schritt für Schritt zeigt
- **fertige Templates** zum Kopieren: `VISION.md`, `AGENTS.md`, `WORKFLOW.md`, `TASKS.md`, `HEARTBEAT.md`, `SECURITY.md`
- einen **LinkedIn-Begleittext**, falls Du das Tutorial in Deinem Netzwerk teilen möchtest

## Inhalt

| Datei                                                              | Zweck                                                       |
|--------------------------------------------------------------------|-------------------------------------------------------------|
| [`paperclip-workflow-tutorial.md`](paperclip-workflow-tutorial.md) | Das vollständige Tutorial                                   |
| [`linkedin-article.md`](linkedin-article.md)                       | Kurz-Begleittext für LinkedIn                               |
| [`attachments/VISION.md`](attachments/VISION.md)                   | Beispiel-Produktvision für ChronoX                          |
| [`attachments/AGENTS.md`](attachments/AGENTS.md)                   | Rollen, Regeln, Eskalation                                  |
| [`attachments/WORKFLOW.md`](attachments/WORKFLOW.md)               | Standardprozess von Idee bis Review, inkl. Status-Modell    |
| [`attachments/TASKS.md`](attachments/TASKS.md)                     | Kuratiertes Projekt-Narrativ (kein Mirror der Paperclip-UI) |
| [`attachments/HEARTBEAT.md`](attachments/HEARTBEAT.md)             | Taktung, Budgets und Pause-Regeln                           |
| [`attachments/SECURITY.md`](attachments/SECURITY.md)               | Threat-Modell und Sicherheitsregeln                         |

## Wie nutze ich das?

1. **Tutorial lesen** — als Einführung in das Konzept und die Begriffe.
2. **Templates kopieren** — die Dateien aus `attachments/` in Dein eigenes Projekt-Repo übernehmen und auf Dein Setup anpassen.
3. **Schrittweise einführen** — mit `VISION.md` und `AGENTS.md` starten, die übrigen Dateien ergänzen, sobald das Projekt sie braucht.

Mein Vorgehen für ein neues Projekt:

```text
1. VISION.md ausfüllen (strategische Richtung)
2. AGENTS.md an das eigene Agenten-Setup anpassen
3. WORKFLOW.md als verbindlichen Prozess übernehmen
4. TASKS.md als kuratiertes Narrativ pflegen (nicht als Status-Mirror)
5. HEARTBEAT.md anlegen, sobald Du mehr als 2-3 Agenten hast
6. SECURITY.md anlegen, sobald externe Dependencies oder Daten ins Spiel kommen
```

## PDF erzeugen

Das Tutorial ist als PDF gut lesbar. Mit [Pandoc](https://pandoc.org/) und dem [Eisvogel-Template](https://github.com/Wandmalfarbe/pandoc-latex-template):

```bash
# einmaliges Setup
brew install pandoc
brew install --cask basictex
sudo tlmgr update --self
sudo tlmgr install collection-fontsrecommended titling

mkdir -p ~/.pandoc/templates
curl -L -o ~/.pandoc/templates/eisvogel.latex \
  https://raw.githubusercontent.com/Wandmalfarbe/pandoc-latex-template/master/eisvogel.latex

# konvertieren
pandoc paperclip-workflow-tutorial.md \
  -o paperclip-tutorial.pdf \
  --template eisvogel \
  --pdf-engine=xelatex \
  --toc --toc-depth=2 \
  --listings
```

Für ein Setup ohne LaTeX funktioniert auch:

```bash
pandoc paperclip-workflow-tutorial.md \
  -o paperclip-tutorial.pdf \
  --pdf-engine=weasyprint \
  --toc --toc-depth=2
```

## Stand und Erfahrungsbasis

Das Tutorial entstand auf Basis von **zwei Wochen aktiver Arbeit mit Paperclip**. Es ist bewusst kein „Best Practice nach Jahren Erfahrung", sondern eine erste, ehrliche Bestandsaufnahme — gedacht als Startpunkt, nicht als Endpunkt. Wer es nutzt und Verbesserungen findet: Issues und Pull Requests sind willkommen.

Geprüft gegen den Paperclip-Stand vom **05.05.2026**. Paperclip entwickelt sich aktiv weiter — manche Konventionen können in späteren Versionen anders aussehen.

## Verhältnis zu anderen Konventionen

- Das offene [`agents.md`](https://agents.md/)-Format definiert `AGENTS.md` enger (Build-/Test-Befehle, Coding-Style). Die hier vorgeschlagene Struktur ist breiter (zusätzlich Rollen, Modi, Eskalation) — beides ist kompatibel.
- `HEARTBEAT.md` als Dateiname kommt ursprünglich aus [OpenClaw](https://docs.openclaw.ai/gateway/heartbeat) (dort als kurze Heartbeat-Checkliste für Agenten). Die hier vorgeschlagene Verwendung erweitert das Konzept zu einem Setup-Dokument für menschliche Leser.

## Lizenz

[Creative Commons Attribution 4.0 International](LICENSE) — Du darfst das Material kopieren, weiterverteilen, anpassen und auch kommerziell nutzen, solange Du den Autor nennst (`Ulrich Cech`) und Änderungen kennzeichnest.

## Autor

Ulrich Cech — [LinkedIn](https://www.linkedin.com/in/ulrichcech)
