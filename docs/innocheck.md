# Innocheck Workflow

Workflow for generating Innosuisse Innovationsscheck proposals (Swiss innovation funding for preliminary studies).

## Overview

The Innovationsscheck funds preliminary studies up to **CHF 15,000** (100% funded) for SMEs to assess the feasibility of innovative ideas with a Swiss research partner. This workflow automates the proposal drafting process.

## Quick Facts

| Parameter | Value |
|-----------|-------|
| **Funding** | Up to CHF 15,000 (100% of research partner costs) |
| **Eligibility** | SMEs, startups, organizations with <250 FTEs, Swiss UID |
| **Purpose** | Preliminary study: concept development, feasibility analysis |
| **Duration** | 6 months to complete the study |
| **Language** | German, French, Italian, or English |
| **Output** | Markdown draft + Word document |

## Usage

```bash
claude

# Run the workflow with your input files
/innocheck inputs/research_goal.md inputs/partner_info.md inputs/budget.xlsx
```

## Input Requirements

Prepare documents containing:

| Information | Description | Example |
|-------------|-------------|---------|
| **Research Goal** | Core innovation idea, scientific approach, expected outcomes | Technical concept, methodology, deliverables |
| **Partner Info** | Implementation partner details (company, UID, contact) | Company profile, key personnel |
| **Budget Data** | Personnel costs, material costs, timeline | Hours breakdown, rates, expenses |
| **Market Context** | Competition, market opportunity, value proposition | Competitive analysis, market size |

## Workflow Steps

```
1. Parse Input Documents
   ↓
2. Activate innocheck-guidelines skill
   ↓
3. Spawn Subagents (parallel)
   ├── Innovation Analyst
   ├── Market Researcher
   └── Financial Planner
   ↓
4. Synthesize Proposal
   ↓
5. Generate Outputs
```

### Subagents

| Agent | Responsibility | Output |
|-------|----------------|--------|
| **Innovation Analyst** | Assess novelty, feasibility, USPs | Innovation level justification |
| **Market Researcher** | Analyze competition, market potential | Market opportunity narrative |
| **Financial Planner** | Validate budget, check compliance | Financial plan summary |

## Evaluation Criteria

The workflow ensures all five Innosuisse criteria are addressed:

| Criterion | Weight | What Evaluators Look For |
|-----------|--------|--------------------------|
| **Innovation Level** | High | Scientific AND economic novelty, differentiation from existing solutions |
| **Potential Benefit** | High | Concrete value creation for implementation partner, quantified impact |
| **Partner Competencies** | Medium | Ability to execute study and commercialize results |
| **Financial Plan** | Medium | Realistic costs, appropriate allocation between personnel/materials |
| **Follow-up Potential** | Medium | Clear path to subsequent Innosuisse Innovation Project |

## Output Files

Generated in `outputs/`:

| File | Description |
|------|-------------|
| `innocheck_proposal_draft.md` | Complete proposal in Markdown |
| `innocheck_proposal.docx` | Formatted Word document for submission |
| `budget_summary.md` | Structured budget table (if needed) |

## Proposal Structure

The generated proposal follows this structure:

1. **Title** - Concise, descriptive project title
2. **Innovation Description** - What is being developed and why it's novel
3. **Feasibility Analysis** - Technical and commercial feasibility assessment
4. **Research Partner Tasks & Deliverables** - Specific activities and outputs
5. **Competitive Situation** - How this differs from existing solutions
6. **Potential Benefits** - Value creation for implementation partner
7. **Financial Plan Summary** - Budget breakdown and justification

## Best Practices

### DO
- Be specific with concrete examples and metrics
- Highlight novelty with explicit differentiation
- Quantify benefits (market size, efficiency gains, cost savings)
- Show competence through references to prior work
- Define measurable, time-bound deliverables
- Connect explicitly to a follow-up Innovation Project

### DON'T
- Make vague claims without evidence
- Focus only on technology (include business value)
- Underestimate time/cost requirements
- Forget the follow-up project pathway
- Include costs outside scope (marketing, equipment)

## Common Mistakes

| Mistake | Impact | Prevention |
|---------|--------|------------|
| Vague innovation claims | Low score on novelty | Compare explicitly to 2-3 existing solutions |
| Missing market validation | Weak benefit argument | Include customer/market evidence |
| Unrealistic budget | Budget rejection | Use standard rates, justify all items |
| No follow-up pathway | Lower priority | Describe concrete next-project concept |

## Related Resources

- **Skill folder**: `.claude/skills/innocheck-guidelines/`
- **Slash command**: `.claude/commands/innocheck.md`
- **Official website**: [Innosuisse Innovationsscheck](https://www.innosuisse.ch)

## See Also

- [Innosuisse Innovation Project](innosuisse-project.md) - Full R&D projects (CHF 500K-2M)
- [SNSF Project Funding](snsf-project.md) - Scientific research grants
