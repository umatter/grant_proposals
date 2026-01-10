# Innosuisse Innovation Project Workflow

Workflow for generating Innosuisse Innovation Project proposals (full R&D collaborative projects with implementation partners).

## Overview

Innosuisse Innovation Projects fund collaborative R&D between Swiss research institutions and implementation partners. Projects must demonstrate both **scientific excellence** and **economic value creation** for Switzerland. Innosuisse covers 40-60% of costs; the implementation partner contributes the remainder.

## Quick Facts

| Parameter | Value |
|-----------|-------|
| **Innosuisse Funding** | 40-60% of total project costs |
| **Partner Contribution** | 40-60% (minimum 5-10% as cash to research partner) |
| **Duration** | 6-36 months (typical: 18-24 months) |
| **Budget** | No maximum (typical: CHF 500,000-2,000,000) |
| **Submission** | Via Innolink platform |
| **Deadline** | 6 weeks before decision meeting (8 meetings/year) |
| **Language** | German, French, Italian, or English |
| **Output** | Markdown (5 files) |

## Usage

```bash
claude

# Run the workflow with your input files
/innosuisse-project inputs/project_concept.md inputs/partner_info.md inputs/budget.xlsx
```

## Input Requirements

Prepare documents containing:

| Information | Description | Example |
|-------------|-------------|---------|
| **Project Concept** | Innovation idea, technical approach, research questions | Technical specification, methodology |
| **Partners** | Research partner (university/FH), Implementation partner (company) | Institution details, company profile |
| **Team** | Named personnel from both partners with roles | CVs, FTE commitments |
| **Market Context** | Target market, customers, competition | Market analysis, customer interviews |
| **Budget Data** | Personnel effort (FTE), equipment, materials, travel | Cost breakdown by work package |
| **Timeline** | Desired duration, key phases | Draft Gantt chart |

## Workflow Steps

```
1. Parse Input Documents
   ↓
2. Activate innosuisse-project-guidelines skill
   ↓
3. Spawn 6 Subagents (parallel)
   ├── Innovation Analyst (reused)
   ├── Market Researcher (reused)
   ├── Methodology Analyst (reused)
   ├── Project Planner (new)
   ├── Risk Analyst (new)
   └── Innosuisse Budget Planner (new)
   ↓
4. Synthesize Innolink Application (9 sections)
   ↓
5. Validate Against Criteria & Budget Rules
   ↓
6. Generate Outputs
```

### Subagents

| Agent | Type | Responsibility | Output |
|-------|------|----------------|--------|
| **Innovation Analyst** | Reused | Scientific AND economic novelty | Level of Innovation section |
| **Market Researcher** | Reused | TAM/SAM/SOM, competitive landscape | Value Creation section |
| **Methodology Analyst** | Reused | Technical approach, team competence | Solution Description section |
| **Project Planner** | New | Work packages, Gantt, milestones | Project Setup section |
| **Risk Analyst** | New | Risk matrix, mitigation strategies | Risk Analysis section |
| **Innosuisse Budget Planner** | New | 40-60% split, notional rates | Budget section |

## Evaluation Criteria

| Criterion | Weight | What Evaluators Look For |
|-----------|--------|--------------------------|
| **Level of Innovation** | High | Scientific AND economic novelty, state-of-the-art comparison |
| **Value Creation** | High | Quantified benefit for Swiss economy/society, viable business model |
| **Project Quality** | High | Realistic planning, competent team, clear deliverables |
| **Sustainable Development** | Medium | Environmental and/or social contribution |

## Output Files

Generated in `outputs/innosuisse_project/`:

| File | Description |
|------|-------------|
| `proposal_draft.md` | Complete proposal covering all 9 Innolink sections |
| `budget_breakdown.md` | Detailed budget tables with 40-60% compliance verification |
| `gantt_chart.md` | Work package timeline with milestones |
| `risk_matrix.md` | Complete risk analysis with mitigation strategies |
| `submission_checklist.md` | Pre-submission verification checklist |

## Application Structure (Innolink)

The proposal covers all 9 Innolink form sections:

| Section | Content |
|---------|---------|
| 1. Organization Information | Partners, team, roles |
| 2. Value Creation | Business model, market entry, competitive advantages |
| 3. Solution Description | Innovation, technical approach, TRL progression |
| 4. Project Setup | Work packages, Gantt, milestones, deliverables |
| 5. Budget | Personnel, materials, overhead, partner split |
| 6. Risk Analysis | Risk matrix, mitigation, monitoring |
| 7. Market Analysis | TAM/SAM/SOM, competitors, customers |
| 8. IP Considerations | Background/foreground IP, protection strategy |
| 9. Financial Annex | For SMEs <250 FTE |

## Budget Rules

### Notional Hourly Rates (Research Partner)

| Position | Rate (CHF/hr) |
|----------|---------------|
| Full Professor | 160-180 |
| Associate Professor | 140-160 |
| Senior Researcher/Postdoc | 110-130 |
| PhD Student | 70-90 |
| Research Assistant | 60-80 |
| Technical Staff | 80-100 |

### Cost Split Requirements

| Component | Requirement |
|-----------|-------------|
| Innosuisse share | 40-60% of total project costs |
| Implementation partner share | 40-60% (in-kind + cash) |
| Cash minimum | 5-10% of Innosuisse funding |
| Overhead | 15-20% of direct costs |

### Eligible vs. Ineligible Costs

| Eligible | NOT Eligible |
|----------|--------------|
| Personnel (notional rates) | Marketing and sales |
| Materials (project-specific) | General company overhead |
| Equipment (depreciated) | Production setup |
| Travel (project-related) | Commercialization activities |
| Subcontracting (justified) | Standard office equipment |

## Best Practices

### DO
- Emphasize BOTH scientific AND economic novelty
- Quantify market potential and expected benefits
- Show clear path from research to commercialization
- Define measurable deliverables with acceptance criteria
- Demonstrate team complementarity (research + industry)
- Include comprehensive risk matrix with mitigations
- Use notional hourly rates for budget
- Ensure both partners lead work packages

### DON'T
- Focus only on scientific aspects (unlike SNSF)
- Underestimate implementation partner contribution
- Present vague deliverables without metrics
- Omit risk analysis
- Include ineligible costs
- Make implementation partner passive (cash-only)

## Common Mistakes

| Mistake | Impact | Prevention |
|---------|--------|------------|
| Innovation claims without evidence | Low novelty score | Add comparison table to 3+ competitors |
| "The market is huge" | Weak value creation | Use TAM/SAM/SOM with sources |
| Generic risks | Weak risk section | Make risks project-specific with numbers |
| Wrong cost split | Budget rejection | Calculate percentages BEFORE completing |
| Vague deliverables | Low quality score | Add measurable acceptance criteria |
| Passive implementation partner | Partner rejection | Both partners must lead WPs |

## Submission Timeline (2025)

| Decision Meeting | Submit By (6 weeks) | Recommended (7 weeks) |
|-----------------|---------------------|----------------------|
| January 31, 2025 | December 20, 2024 | December 13, 2024 |
| March 21, 2025 | February 7, 2025 | January 31, 2025 |
| April 11, 2025 | February 28, 2025 | February 21, 2025 |
| May 23, 2025 | April 11, 2025 | April 4, 2025 |
| July 4, 2025 | May 23, 2025 | May 16, 2025 |
| September 12, 2025 | August 1, 2025 | July 25, 2025 |
| October 24, 2025 | September 12, 2025 | September 5, 2025 |
| November 28, 2025 | October 17, 2025 | October 10, 2025 |

## Related Resources

- **Skill folder**: `.claude/skills/innosuisse-project-guidelines/`
- **Slash command**: `.claude/commands/innosuisse-project.md`
- **Official website**: [Innosuisse Innovation Projects](https://www.innosuisse.ch)

## See Also

- [Innocheck](innocheck.md) - Preliminary studies (CHF 15K)
- [SNSF Project Funding](snsf-project.md) - Scientific research grants
