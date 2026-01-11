# BFH E-Learning Förderprogramm Workflow

Dieser Workflow unterstützt bei der Erstellung von Förderanträgen für das E-Learning-Förderprogramm der Berner Fachhochschule (BFH).

## Übersicht

| Parameter | Wert |
|-----------|------|
| **Kommando** | `/bfh-elearning` |
| **Förderprogramm** | E-Learning-Förderprogramm BFH |
| **Fördersumme** | Typisch CHF 10'000 (Stundengutschriften) |
| **Sprache** | Deutsch (Schweizer Rechtschreibung) |
| **Output** | Markdown + DOCX |

## Schnellstart

```bash
claude

# Workflow mit Kontextdateien starten
/bfh-elearning inputs/kurskonzept.md inputs/projektidee.md inputs/team.md
```

## Eingabedokumente

Bereite folgende Informationen in den Eingabedateien vor:

### Pflichtangaben
- **Lehrveranstaltung**: Name, ECTS, Studiengang, Departement
- **Projektidee**: Was soll entwickelt werden
- **Team**: Antragsteller*in und weitere Beteiligte
- **Budget**: Geschätzter Aufwand nach Rollen

### Empfohlene Zusatzinformationen
- Bestehende Evaluationsergebnisse oder Studierendenfeedback
- Aktuelle Probleme/Herausforderungen im Kurs
- Geplante Kooperationen mit anderen Dozierenden
- Vorhandene technische Infrastruktur

## Workflow-Ablauf

```
┌─────────────────────────────────────────────────────────────┐
│  /bfh-elearning @context_files...                          │
└─────────────────────────────┬───────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    ORCHESTRATOR                             │
│  1. Lädt Kontextdokumente                                   │
│  2. Aktiviert bfh-elearning-guidelines Skill                │
│  3. Startet Subagenten parallel                             │
└─────────────────────────────┬───────────────────────────────┘
                              │
              ┌───────────────┴───────────────┐
              ▼                               ▼
┌─────────────────────────┐     ┌─────────────────────────┐
│ didactic-innovation-    │     │ bfh-elearning-          │
│ analyst                 │     │ planner                 │
│                         │     │                         │
│ - 6 Förderkriterien     │     │ - Projektplan           │
│ - Empfohlene Texte      │     │ - Budgetberechnung      │
│ - Verbesserungshinweise │     │ - Zeitplan              │
└─────────────────────────┘     └─────────────────────────┘
              │                               │
              └───────────────┬───────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    SYNTHESE                                 │
│  - Kombiniert Analysen zu vollständigem Antrag              │
│  - Prüft Qualitätscheckliste                                │
│  - Generiert Output-Dateien                                 │
└─────────────────────────────┬───────────────────────────────┘
                              ▼
                  ┌─────────────────────┐
                  │  outputs/           │
                  │  bfh_elearning/     │
                  │  ├── proposal.md    │
                  │  └── proposal.docx  │
                  └─────────────────────┘
```

## Subagenten

### didactic-innovation-analyst
Analysiert das Projekt anhand der sechs Förderkriterien:

| Kriterium | Was wird bewertet |
|-----------|-------------------|
| Strategiebezug | Vielfalt, Future Skills, Vernetzung |
| Innovation | Didaktische Neuerungen |
| Qualitätsverbesserung | Verbesserungen durch digitale Elemente |
| Nachhaltigkeit | Langfristige Nutzbarkeit |
| Vernetzung | Zusammenarbeit |
| Transferpotenzial | Übertragbarkeit, OER |

**Output:** Empfohlene Texte für jeden Kriterienabschnitt

### bfh-elearning-planner
Erstellt Projektplan und validiert Budget:

| Aspekt | Was wird geprüft |
|--------|------------------|
| Zeitplan | Realistische Phasen, Förderjahr-Konformität |
| Budget | BFH-Stundenansätze, Plausibilität |
| Rollen | Aufgabenzuordnung, Projektleitung |

**Output:** Projektschritte-Tabelle, Kostenaufstellung

## Stundenansätze (verbindlich)

| Rolle | Ansatz (CHF/h) |
|-------|----------------|
| Dozierende | 104 |
| Wissenschaftliche Mitarbeitende | 70 |
| Assistierende | 52 |

## Fristen

| Call | Einreichfrist | Entscheidung |
|------|---------------|--------------|
| Call 2025 | 30. April 2025 | 1. Juni 2025 |
| Call 2026 | 31. Juli 2025 | 30. September 2025 |
| Folgejahre | 31. Juli | 30. September |

## Voraussetzungen

Bevor ein Projekt eingereicht werden kann, müssen folgende Bedingungen erfüllt sein:

- [ ] Lehrveranstaltung mit mindestens 2 ECTS
- [ ] Nutzung von BFH-Tools (Moodle, Kaltura)
- [ ] Kompetenz-, handlungs- und praxisorientiertes Konzept
- [ ] Bereitschaft zur OER-Publikation auf der Virtuellen Akademie
- [ ] Bereitschaft zur Präsentation als Good-Practice-Beispiel

## Output-Dateien

Der Workflow generiert:

| Datei | Beschreibung |
|-------|--------------|
| `proposal_draft.md` | Markdown-Entwurf, strukturiert nach offiziellem Formular |
| `proposal_draft.docx` | Word-Dokument zur finalen Bearbeitung |

## Beispiel-Eingabe

```markdown
# Projektidee: Interaktive Statistik-Übungen

## Lehrveranstaltung
- Name: Statistik für Wirtschaftswissenschaften
- ECTS: 4
- Studiengang: BSc Betriebsökonomie
- Departement: Wirtschaft

## Projektbeschreibung
Entwicklung interaktiver Moodle-Übungen mit automatischem
Feedback für das Modul Statistik. Studierende sollen
selbstgesteuert üben können.

## Team
- Antragsteller: Dr. Max Muster
- Mitwirkende: Maria Beispiel (WiMi)

## Geschätzter Aufwand
- Konzeption: 30h (Dozent)
- Entwicklung: 60h (WiMi)
- Pilotierung: 20h (Dozent, WiMi)
- Abschluss: 10h (WiMi)
```

## Tipps für erfolgreiche Anträge

1. **Konkret sein**: Vermeide vage Aussagen, nutze Beispiele und Zahlen
2. **Didaktik vor Technik**: Erkläre den pädagogischen Mehrwert, nicht die Technik
3. **Nachhaltigkeit zeigen**: Beschreibe, wie das Projekt über die Förderperiode hinaus Bestand hat
4. **Vernetzung aufzeigen**: Auch bei Einzelanträgen: Wer könnte von den Materialien profitieren?
5. **OER-Bereitschaft**: Explizit zusagen, dass Materialien geteilt werden

## Referenzmaterial

- Erfolgreiches Antragsbeispiel: `_resources/old_proposals/bfh_elearning/`
- Offizielles Merkblatt: `_resources/funder_documentations/`
- Antragsformular: `_resources/funder_documentations/Antragsformular-E-Learning-Förderprogramm-BFH.docx`

## Support

Bei Fragen zur Förderung: virtuelle.akademie@bfh.ch
