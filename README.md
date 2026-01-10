# Grant Proposals

Agentic workflows for drafting grant proposals using Claude Code.

## Overview

This repository contains Claude Code workflows that assist in creating comprehensive, criteria-aligned grant proposals. Each workflow processes context documents and orchestrates specialized subagents to produce complete proposal drafts tailored to specific funding programs.

## Available Workflows

| Workflow | Command | Funding Program | Output |
|----------|---------|-----------------|--------|
| [Innocheck](docs/innocheck.md) | `/innocheck` | Innosuisse Innovationsscheck | Markdown/Word |
| [Innosuisse Project](docs/innosuisse-project.md) | `/innosuisse-project` | Innosuisse Innovation Project | Markdown |
| [SNSF Project](docs/snsf-project.md) | `/snsf-project` | SNSF Project Funding | LaTeX |

## Quick Comparison

| Aspect | Innocheck | Innosuisse Project | SNSF Project |
|--------|-----------|-------------------|--------------|
| **Budget** | CHF 15,000 (fixed) | No max (typ. 500K-2M) | CHF 100K-4M |
| **Duration** | 6 months | 6-36 months | 1-4 years |
| **Funding Rate** | 100% | 40-60% | 100% |
| **Focus** | Feasibility study | Innovation + Value creation | Scientific research |
| **Partners** | Research + Implementation | Research + Implementation | Academic |
| **Complexity** | Simple | Medium-High | High |

## General Usage

### 1. Prepare Input Documents

Place your context documents in the `inputs/` directory. Required documents vary by workflow but typically include:

- **Research goal**: Innovation idea, scientific approach, expected outcomes
- **Partner information**: Implementation and research partner details
- **Budget data**: Personnel and material cost estimates
- **Team CVs**: Key personnel qualifications

### 2. Run a Workflow

```bash
claude

# Use the appropriate slash command with your input files
/<workflow> inputs/file1.md inputs/file2.md ...
```

### 3. Review Outputs

Generated proposals appear in `outputs/<workflow-name>/`.

## Repository Structure

```
grant_proposals/
├── .claude/
│   ├── commands/           # Slash commands for each workflow
│   ├── agents/             # Specialized subagents
│   └── skills/             # Domain-specific guidelines and templates
├── docs/                   # Detailed workflow documentation
├── CLAUDE.md               # Project configuration
├── inputs/                 # Your context documents
├── outputs/                # Generated proposals
│   ├── innocheck/
│   ├── innosuisse_project/
│   └── snsf_project/
└── _resources/             # Implementation plans and reference materials
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
│  SUBAGENT 1   │     │  SUBAGENT 2   │     │  SUBAGENT N   │
│  (parallel)   │     │  (parallel)   │     │  (parallel)   │
└───────────────┘     └───────────────┘     └───────────────┘
                                │
                                ▼
                    ┌─────────────────┐
                    │  FINAL OUTPUT   │
                    └─────────────────┘
```

## Subagents

### Shared (Reusable)
| Subagent | Purpose |
|----------|---------|
| `innovation-analyst` | Assess novelty, feasibility, scientific merit |
| `market-researcher` | Analyze competition and market potential |
| `methodology-analyst` | Evaluate methods, feasibility, risks |

### Innocheck-Specific
| Subagent | Purpose |
|----------|---------|
| `financial-planner` | Validate CHF 15K budget compliance |

### Innosuisse Project-Specific
| Subagent | Purpose |
|----------|---------|
| `project-planner` | Design work packages, Gantt charts, milestones |
| `risk-analyst` | Create risk matrices with mitigation strategies |
| `innosuisse-budget-planner` | Validate 40-60% split, notional rates |

### SNSF-Specific
| Subagent | Purpose |
|----------|---------|
| `scientific-relevance` | Evaluate significance, originality, impact |
| `track-record-analyst` | Generate CV achievements narrative |
| `literature-builder` | Build state-of-research and bibliography |
| `snsf-budget-planner` | Validate budget against SNSF limits |

## Documentation

Detailed documentation for each workflow is available in the `docs/` folder:

- [Innocheck Workflow](docs/innocheck.md) - Feasibility studies (CHF 15K)
- [Innosuisse Innovation Project](docs/innosuisse-project.md) - Full R&D projects (CHF 500K-2M)
- [SNSF Project Funding](docs/snsf-project.md) - Scientific research (CHF 100K-4M)

## Adding New Workflows

To add a workflow for a new grant type:

1. Create a slash command in `.claude/commands/<grant-name>.md`
2. Add domain-specific skills in `.claude/skills/<grant-name>-guidelines/`
3. Create or reuse subagents in `.claude/agents/`
4. Document the workflow in `docs/<grant-name>.md`
5. Update this README

## Language Support

Workflows support multiple languages where applicable:

- **SNSF**: English required for economics, STEM, medicine, psychology
- **Innosuisse**: German, French, Italian, or English
- **Innocheck**: German, French, Italian, or English

Output language typically matches the input documents. If unclear, the workflow will ask for preference.

## Resources

- [Innosuisse Official Website](https://www.innosuisse.ch)
- [SNSF Official Website](https://www.snf.ch)
- Reference materials: `_resources/`
