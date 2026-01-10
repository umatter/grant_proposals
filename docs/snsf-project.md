# SNSF Project Funding Workflow

Workflow for generating SNSF (Swiss National Science Foundation) Project Funding proposals (scientific research grants).

## Overview

SNSF Project Funding supports research projects in all disciplines at Swiss higher education institutions. Projects must contribute to **scientific advancement** and can cover basic or applied research. This workflow generates LaTeX-formatted research plans following the structure of successful proposals.

## Quick Facts

| Parameter | Value |
|-----------|-------|
| **Funding Range** | CHF 100,000 - 4,000,000 |
| **Duration** | 1-4 years |
| **Per Applicant** | Max CHF 250,000/year average |
| **Per Project** | Max CHF 1,000,000/year |
| **Equipment Cap** | CHF 100,000 |
| **Research Plan** | 15 pages / 60,000 chars (single applicant) |
| **Research Plan** | 17 pages / 68,000 chars (collaborative) |
| **CV Achievements** | Max 4,350 characters |
| **Deadlines** | 1 April, 1 October (17:00 Swiss time) |
| **Language** | English (required for STEM, economics, medicine, psychology) |
| **Output** | LaTeX |

## Usage

```bash
claude

# Run the workflow with your input files
/snsf-project inputs/research_concept.md inputs/cv.md inputs/budget.xlsx
```

## Input Requirements

Prepare documents containing:

| Information | Description | Example |
|-------------|-------------|---------|
| **Research Concept** | Core idea, questions, hypotheses, expected outcomes | Research proposal draft |
| **Methodology** | Proposed methods, experimental designs, data analysis | Methods section |
| **Track Record** | CV info, prior publications, previous grants | Academic CV |
| **Budget Data** | Personnel, equipment, consumables, travel | Cost breakdown |
| **Team Composition** | PI, project partners, collaborators | Team structure |

## Workflow Steps

```
1. Parse Input Documents
   ↓
2. Activate snsf-guidelines skill
   ↓
3. Determine Proposal Type (single vs. collaborative)
   ↓
4. Spawn 5 Subagents (parallel)
   ├── Scientific Relevance Analyst
   ├── Methodology Analyst
   ├── Track Record Analyst
   ├── Literature Context Builder
   └── SNSF Budget Planner
   ↓
5. Synthesize Research Plan (5 chapters)
   ↓
6. Validate Against Criteria & Limits
   ↓
7. Generate LaTeX Outputs
```

### Subagents

| Agent | Responsibility | Output |
|-------|----------------|--------|
| **Scientific Relevance** | Significance, originality, impact | Section 2.1 (State of Research), Section 2.4 (Significance) |
| **Methodology Analyst** | Methods, feasibility, risks | Section 2.3 (Detailed Research Plan) |
| **Track Record Analyst** | CV achievements, qualifications | Section 2.2 (Own Research), CV narrative |
| **Literature Builder** | Field overview, bibliography | Section 2.1 support, references |
| **SNSF Budget Planner** | Budget validation, justification | Budget justification text |

## Evaluation Criteria

Research shows **Methods & Feasibility** and **Scientific Relevance** have the strongest influence on funding decisions.

| Criterion | Weight | What Evaluators Look For |
|-----------|--------|--------------------------|
| **Methods & Feasibility** | Highest (β=0.436) | Sound methodology, realistic timeline, adequate resources |
| **Scientific Relevance** | High (β=0.401) | Importance to the field, potential impact |
| **Originality** | High | Novel contribution, not incremental |
| **Track Record** | Medium | Demonstrated ability to deliver, relevant expertise |
| **Topicality** | Medium | Timeliness, current relevance |

### Scoring Scale

| Score | Meaning |
|-------|---------|
| 9 | Outstanding |
| 8 | Excellent |
| 7 | Very Good (competitive threshold) |
| 6 | Good |
| 5 | Average (baseline) |

## Output Files

Generated in `outputs/snsf_project/`:

| File | Description |
|------|-------------|
| `main.tex` | Complete research plan in LaTeX |
| `ref.bib` | Bibliography entries |
| `cv_achievements.md` | CV achievements narrative with character count |
| `budget_justification.md` | Budget breakdown with justifications |
| `README.md` | Compilation instructions and checklist |

## Research Plan Structure

The generated proposal follows the SNSF structure:

| Section | Pages | Content |
|---------|-------|---------|
| **1. Summary** | ~1 | Title, questions, methodology, outcomes (write last) |
| **2.1 State of Research** | 2-3 | Context, current knowledge, gaps, positioning |
| **2.2 Own Research** | 1-2 | Prior work, preliminary results, enabling experience |
| **2.3 Detailed Research Plan** | 6-8 | Objectives, hypotheses, methods, WPs, risks |
| **2.4 Significance and Impact** | 1-2 | Scientific relevance, broader impact, applications |
| **2.5 Schedule and Milestones** | ~1 | Gantt chart, milestones, go/no-go points |

## Budget Rules

### Budget Limits

| Limit | Value |
|-------|-------|
| Per applicant average | Max CHF 250,000/year |
| Per project | Max CHF 1,000,000/year |
| Equipment | Max CHF 100,000 total |

### Eligible Costs

| Category | Included | NOT Included |
|----------|----------|--------------|
| Personnel | Research staff, scientific collaborators | Applicant's own salary |
| Equipment | Research equipment up to CHF 100K | Standard office equipment |
| Consumables | Project-specific materials | General supplies |
| Travel | Research-related travel | Conferences (separate funding) |

### Prohibited Costs

- Applicant's salary
- Overhead costs
- Publication costs (separate funding available)
- Journal metrics in CV (DORA compliance)

## Best Practices

### DO
- Be specific about novelty with explicit differentiation
- Show feasibility with justified methods
- Connect track record directly to the proposed project
- Quantify where possible (impact, sample sizes, effect sizes)
- Assess risks with concrete mitigation strategies
- Use diverse outputs in CV (data, software, not just papers)
- Include recent works (last 5 years) and seminal contributions

### DON'T
- Oversell without evidence
- Ignore limitations or risks
- Exceed page/character limits
- Use journal metrics in CV (Impact Factor, h-index)
- Include applicant's own salary in budget
- Add overheads or publication costs

## Common Mistakes

| Mistake | Impact | Prevention |
|---------|--------|------------|
| Exceeding page limits | Formal rejection | Count characters, use LaTeX template |
| Weak methods section | Lowest scores | Justify every methodological choice |
| Generic significance claims | Low relevance score | Be specific about contributions |
| CV over character limit | Formal rejection | Count characters (max 4,350) |
| Including prohibited costs | Budget revision | Review eligible costs list |
| Ignoring risks | Feasibility concerns | Add dedicated risk section |

## Submission Deadlines

| Deadline | Submit By |
|----------|-----------|
| Spring deadline | 1 April, 17:00 Swiss time |
| Autumn deadline | 1 October, 17:00 Swiss time |

## Language Requirements

| Discipline | Required Language |
|------------|-------------------|
| Mathematics, Natural Sciences | English |
| Engineering Sciences | English |
| Biology, Medicine | English |
| Psychology | English |
| Economics, Political Science | English |
| Humanities | German, French, Italian, or English |

## Related Resources

- **Skill folder**: `.claude/skills/snsf-guidelines/`
- **Slash command**: `.claude/commands/snsf-project.md`
- **Example proposals**: `_resources/old_proposals/snsf_project/`
- **Official website**: [SNSF Project Funding](https://www.snf.ch)

## See Also

- [Innocheck](innocheck.md) - Preliminary studies (CHF 15K)
- [Innosuisse Innovation Project](innosuisse-project.md) - Industry-academia R&D (CHF 500K-2M)
