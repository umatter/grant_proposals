# Grant Proposals

Agentic workflows for drafting grant proposals using Claude Code.

## Overview

This repository contains Claude Code workflows that assist in creating comprehensive, criteria-aligned grant proposals. Each workflow processes context documents and orchestrates specialized subagents to produce complete proposal drafts tailored to specific funding programs.

## Available Workflows

| Workflow | Command | Funding Program |
|----------|---------|-----------------|
| [Innocheck](#innocheck) | `/innocheck` | Innosuisse Innovationsscheck (Switzerland) |

## General Usage

### 1. Prepare Input Documents

Place your context documents in the `inputs/` directory. Required documents vary by workflow but typically include:

- **Research goal**: Innovation idea, scientific approach, expected outcomes
- **Partner information**: Implementation and research partner details
- **Budget data**: Personnel and material cost estimates

### 2. Run a Workflow

```bash
claude

# Use the appropriate slash command with your input files
/<workflow> inputs/file1.md inputs/file2.md ...
```

### 3. Review Outputs

Generated proposals appear in `outputs/`.

## Repository Structure

```
grant_proposals/
├── .claude/
│   ├── commands/           # Slash commands for each workflow
│   ├── agents/             # Specialized subagents
│   └── skills/             # Domain-specific guidelines and templates
├── CLAUDE.md               # Project configuration
├── inputs/                 # Your context documents
├── outputs/                # Generated proposals
└── _resources/             # Implementation plans and documentation
```

## Workflow Architecture

Each workflow follows a similar pattern:

```
┌─────────────────────────────────────────────────────────────────────┐
│  /<command> @context_files...                                       │
└───────────────────────────────┬─────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────────┐
│                    MAIN ORCHESTRATOR                                │
│  - Parses input documents                                           │
│  - Activates relevant skills                                        │
│  - Spawns specialized subagents in parallel                         │
│  - Synthesizes final proposal                                       │
└───────────────────────────────┬─────────────────────────────────────┘
                                │
        ┌───────────────────────┼───────────────────────┐
        ▼                       ▼                       ▼
┌───────────────┐     ┌───────────────┐     ┌───────────────┐
│  SUBAGENT 1   │     │  SUBAGENT 2   │     │  SUBAGENT 3   │
│  (Analysis)   │     │  (Research)   │     │  (Validation) │
└───────────────┘     └───────────────┘     └───────────────┘
                                │
                                ▼
                    ┌─────────────────┐
                    │  FINAL OUTPUT   │
                    └─────────────────┘
```

---

## Innocheck

Workflow for Innosuisse Innovationsscheck proposals (Swiss innovation funding).

### Quick Facts

| Aspect | Details |
|--------|---------|
| **Funding** | Up to CHF 15,000 (100% of research partner costs) |
| **Eligibility** | SMEs, startups, organizations with <250 FTEs, Swiss UID |
| **Purpose** | Preliminary study: concept development, feasibility analysis |
| **Duration** | 6 months to complete the study |

### Usage

```bash
/innocheck inputs/research_goal.md inputs/partner_info.md inputs/budget.xlsx
```

### Subagents

- **Innovation Analyst** - Assesses novelty and technical feasibility
- **Market Researcher** - Analyzes competition and market potential
- **Financial Planner** - Validates budget compliance with Innosuisse rules

### Evaluation Criteria

The workflow ensures all five Innosuisse criteria are addressed:

1. **Innovation Level** - Scientific and economic novelty
2. **Potential Benefit** - Value for implementation partner
3. **Partner Competencies** - Execution and commercialization capabilities
4. **Financial Plan** - Realistic personnel and material costs
5. **Follow-up Project** - Path to subsequent innovation project

### Resources

- [Innosuisse Official Website](https://www.innosuisse.ch)
- Implementation plan: `_resources/innocheck-workflow-implementation-plan.md`

---

## Adding New Workflows

To add a workflow for a new grant type:

1. Create a slash command in `.claude/commands/<grant-name>.md`
2. Add domain-specific skills in `.claude/skills/<grant-name>-guidelines/`
3. Create or reuse subagents in `.claude/agents/`
4. Document the workflow in this README

## Architecture Recommendations

### Shared Components (in `.claude/agents/`)

Subagents should be **generic and reusable** across grant types:

| Subagent | Purpose | Why Shared |
|----------|---------|------------|
| `innovation-analyst` | Assess novelty and feasibility | Innovation evaluation is similar across funders |
| `market-researcher` | Analyze competition and market | Market analysis follows common patterns |
| `financial-planner` | Validate budgets | Cost structures are comparable; funder-specific rules go in skills |

Add new shared subagents only for **cross-cutting concerns** (e.g., `ethics-reviewer`, `ip-analyst`, `impact-assessor`).

### Grant-Specific Components (in `.claude/skills/` and `.claude/commands/`)

Skills and commands should be **specific to each funding program**:

| Component | Location | Contains |
|-----------|----------|----------|
| Slash command | `.claude/commands/<grant>.md` | Orchestration logic, which subagents to call, output format |
| Skill | `.claude/skills/<grant>-guidelines/` | Evaluation criteria, application structure, templates, common mistakes |
| References | `.claude/skills/<grant>-guidelines/references/` | Funder-specific rules, eligibility, budget constraints |
| Templates | `.claude/skills/<grant>-guidelines/templates/` | Proposal and budget templates matching funder requirements |

### Rule of Thumb

- **If it applies to most grants** → put it in a shared subagent
- **If it's funder-specific** → put it in the grant's skill folder
- **If you're unsure** → start specific, generalize later when patterns emerge

## Language Support

Workflows support multiple languages where applicable. Output language typically matches the input documents.
