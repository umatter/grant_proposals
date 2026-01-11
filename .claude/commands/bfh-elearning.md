---
description: Draft a BFH E-Learning Förderprogramm proposal from context documents
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
  - Task
  - Skill
argument-hint: <context-files...>
---

# BFH E-Learning Förderprogramm Proposal Generator

Du bist ein Experte für die Erstellung von Förderanträgen für das E-Learning-Förderprogramm der Berner Fachhochschule (BFH).
Generiere einen vollständigen Förderantrag basierend auf den bereitgestellten Kontextdokumenten.

**WICHTIG:** Alle Ausgaben müssen auf Deutsch (Schweizer Rechtschreibung) verfasst sein. Verwende "ss" statt "ß".

## Input Processing

Der Benutzer hat folgende Kontextdateien bereitgestellt: $ARGUMENTS

### Schritt 1: Kontextdokumente laden und analysieren

1. Lies alle bereitgestellten Dateien mit geeigneten Tools
2. Extrahiere und kategorisiere folgende Informationen:
   - **Lehrveranstaltung**: Name, ECTS, Studiengang, Departement
   - **Projektidee**: Was soll entwickelt werden, welche digitalen Elemente
   - **Team**: Antragsteller*in, weitere Projektbeteiligte
   - **Budget**: Geschätzter Aufwand, beteiligte Rollen
   - **Didaktisches Konzept**: Lernziele, Methoden, Präsenz-/Online-Anteile

### Schritt 2: BFH E-Learning Skill aktivieren

Lade die bfh-elearning-guidelines Skill für Bewertungskriterien und Strukturvorgaben.
Lies die Bewertungskriterien und häufigen Fehler.

### Schritt 3: Spezialisierte Subagenten parallel starten

Verwende das Task-Tool, um diese Subagenten gleichzeitig zu starten:

1. **Didaktische Innovationsanalyse** (didactic-innovation-analyst)
   - Bewerte alle 6 Förderkriterien
   - Analysiere den Bezug zur BFH-Strategie "Lehren und Lernen im digitalen Zeitalter"
   - Beurteile didaktische Innovation und Qualitätsverbesserung
   - Evaluiere Nachhaltigkeit, Vernetzung und Transferpotenzial
   - Generiere empfohlene Texte für jeden Kriterienabschnitt

2. **Projektplanung** (bfh-elearning-planner)
   - Erstelle detaillierten Projektplan mit Zeitrahmen
   - Berechne Budget mit BFH-Stundenansätzen (Dozierende: 104 CHF, WiMi: 70 CHF, Assistierende: 52 CHF)
   - Validiere, dass der Antrag realistisch und umsetzbar ist
   - Generiere Kostenaufstellung und Begründung

### Schritt 4: Antragsentwurf synthetisieren

Nach Abschluss aller Subagenten, synthetisiere die Ergebnisse in einen strukturierten Antrag:

1. Erstelle den Antrag gemäss offizieller Struktur:
   - **Angaben zur antragstellenden Person**
   - **Formale Angaben** (Projekttitel, Zeitraum, Fördersumme, Departement, Studiengang, Lehrveranstaltung, ECTS)
   - **Abstract** (max. 500 Wörter)
   - **Konzept der Lehrveranstaltung** (max. 1 Seite)
   - **Projektziele** (Nutzen aus Sicht der Zielgruppe)
   - **Projektschritte** (Tabelle mit Zeitplan)
   - **Projektkosten** (max. 1/2 Seite mit Kostenaufstellung)
   - **Begründung der Förderungswürdigkeit** (max. 2 Seiten, alle 6 Kriterien)

2. Stelle sicher, dass alle sechs Bewertungskriterien explizit adressiert werden:
   - ✓ Bezug zur Strategie (Vielfalt/Future Skills/Vernetzung)
   - ✓ Didaktische Innovation
   - ✓ Qualitätsverbesserung
   - ✓ Nachhaltigkeit
   - ✓ Vernetzung
   - ✓ Transferpotenzial

### Schritt 5: Ausgaben generieren

1. **Markdown-Entwurf**: Speichere nach `outputs/bfh_elearning/proposal_draft.md`
2. **Word-Dokument**: Konvertiere mit pandoc zu DOCX:
   ```bash
   cd outputs/bfh_elearning && pandoc -o proposal_draft.docx proposal_draft.md
   ```

### Qualitätscheckliste

Vor der Finalisierung prüfe:
- [ ] Alle erforderlichen Abschnitte sind vollständig
- [ ] Abstract ist unter 500 Wörtern
- [ ] Konzept der Lehrveranstaltung ist max. 1 Seite
- [ ] Projektkosten-Begründung ist max. 1/2 Seite
- [ ] Förderungswürdigkeit-Begründung ist max. 2 Seiten
- [ ] Alle 6 Kriterien sind explizit adressiert
- [ ] BFH-Tools (Moodle, Kaltura) werden verwendet
- [ ] Lehrveranstaltung hat mindestens 2 ECTS
- [ ] Stundenansätze entsprechen BFH-Vorgaben
- [ ] Bereitschaft zur OER-Publikation ist erklärt
- [ ] Sprache ist Deutsch mit Schweizer Rechtschreibung (kein ß)
