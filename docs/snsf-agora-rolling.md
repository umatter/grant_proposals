# SNSF Agora Rolling Call Workflow

This workflow supports the creation of SNSF Agora Rolling Call proposals for science communication projects (up to CHF 50,000).

## Overview

| Parameter | Value |
|-----------|-------|
| **Command** | `/snsf-agora-rolling` |
| **Funding Program** | SNSF Agora Rolling Call |
| **Funding Range** | CHF 5,000 - 50,000 |
| **Duration** | Max. 3 years |
| **Decision Timeline** | Within 4 months |
| **Language** | English only |
| **Output** | Markdown + PDF |

## What is SNSF Agora?

Agora supports communication projects that promote **dialogue** between science and society. Unlike traditional dissemination, Agora emphasizes two-way engagement where the public can interact with and respond to scientific content.

**Key principle:** Agora is about DIALOGUE, not just broadcasting.

## Quick Start

```bash
claude

# Run the workflow with your context documents
/snsf-agora-rolling inputs/research_summary.md inputs/communication_idea.md inputs/team.md
```

## Input Documents

Prepare the following information in your input files:

### Required Information
- **Research Topic**: The scientific research to be communicated
- **Communication Idea**: How you plan to engage the public
- **Target Audience**: Who you want to reach (must include Swiss public)
- **Team**: Researcher(s) AND Agora Specialist(s)
- **Budget Estimate**: Approximate costs

### Team Requirements (CRITICAL)

Your team MUST include:

| Role | Requirements |
|------|-------------|
| **Researcher** | Min. 20% work-time allocation, SNSF Article 10 compliant |
| **Agora Specialist** | Communication/media/education/art professional |

**Note:** Missing an Agora specialist is the most common reason for rejection.

## Workflow Architecture

```
┌─────────────────────────────────────────────────────────────┐
│  /snsf-agora-rolling @context_files...                     │
└─────────────────────────────┬───────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    ORCHESTRATOR                             │
│  1. Loads context documents                                 │
│  2. Verifies team composition                               │
│  3. Activates snsf-agora-guidelines skill                   │
│  4. Spawns subagents in parallel                            │
└─────────────────────────────┬───────────────────────────────┘
                              │
              ┌───────────────┴───────────────┐
              ▼                               ▼
┌─────────────────────────┐     ┌─────────────────────────┐
│ science-communication-  │     │ agora-project-          │
│ analyst                 │     │ planner                 │
│                         │     │                         │
│ - Content quality       │     │ - Team validation       │
│ - Target audience       │     │ - Timeline creation     │
│ - Communication methods │     │ - Budget planning       │
│ - Impact assessment     │     │ - Feasibility check     │
└─────────────────────────┘     └─────────────────────────┘
              │                               │
              └───────────────┬───────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    SYNTHESIS                                │
│  - Combines analyses into 5-page project plan               │
│  - Validates against all 6 criteria                         │
│  - Generates output files                                   │
└─────────────────────────────┬───────────────────────────────┘
                              ▼
                  ┌─────────────────────┐
                  │  outputs/           │
                  │  snsf_agora_rolling/│
                  │  ├── project_plan.md│
                  │  └── project_plan.pdf│
                  └─────────────────────┘
```

## Evaluation Criteria

All six criteria are **equally weighted**:

| Criterion | What Evaluators Look For |
|-----------|--------------------------|
| **Content Quality** | Sound research, clear messages |
| **Communication Method** | Appropriate for target audience, includes dialogue |
| **Team Expertise** | Relevant skills for communication project |
| **Feasibility** | Realistic timeline and budget |
| **Expected Impact** | Quantitative reach + qualitative engagement |
| **Diversity** | Inclusive approach, accessibility |

## Project Plan Structure (5 pages max)

| Section | Pages | Content |
|---------|-------|---------|
| 1. Summary | 0.5 | Project goal, audience, main activities |
| 2. Research Context | 1.0 | Research description, SNSF link, why communicate |
| 3. Communication Concept | 1.5 | Audience, methods, dialogue elements |
| 4. Project Plan and Team | 1.5 | Timeline, roles, qualifications |
| 5. Impact and Feasibility | 0.5 | Expected outcomes, risks |

## Budget Guidelines

| Category | Description |
|----------|-------------|
| Agora specialists | Fees for communication professionals |
| External partners | Collaborating organizations |
| Production/materials | Videos, exhibits, printed materials |
| Communication/publication | Events, promotion, media |

**NOT Eligible:**
- Researcher salaries at Swiss universities (generally)
- Equipment >CHF 50,000 without multiple quotes
- Institutional overhead

## Eligibility Checklist

Before submitting, ensure:

- [ ] Team includes researcher + Agora specialist
- [ ] Project promotes **dialogue** (not just dissemination)
- [ ] Primary audience is in Switzerland
- [ ] Links to SNSF-funded research or peer-reviewed publications
- [ ] NOT marketing, institutional communication, or tech transfer
- [ ] NOT connected to NRP/NCCR
- [ ] Budget is CHF 5,000 - 50,000
- [ ] Project plan ≤ 5 pages
- [ ] All content in English

## Output Files

| File | Description |
|------|-------------|
| `project_plan.md` | 5-page project plan in markdown |
| `project_plan.pdf` | PDF version for mySNF submission |

## Example Input

```markdown
# Research Topic

Our recent SNSF-funded research (project #12345) discovered a new
mechanism for plastic degradation using engineered bacteria...

# Communication Idea

We want to engage Swiss high school students (ages 15-18) in
understanding biotechnology and environmental science through:
1. Interactive workshops at schools
2. A hands-on exhibit at science festivals
3. A video series with Q&A sessions

# Team

Researcher: Dr. Anna Schmidt (University of Bern, 30% allocation)
Agora Specialist: Marco Weber (Science Communication Agency, project lead)

# Budget Estimate

~CHF 35,000 for workshops, exhibit materials, video production
```

## Common Mistakes to Avoid

1. **No Agora specialist** - Every project needs a communication expert
2. **One-way communication** - Include dialogue and interaction
3. **Vague audience** - Define specifically who you're reaching
4. **No SNSF link** - Must connect to SNSF research or publications
5. **Over 5 pages** - Stick to the limit

## Resources

- [SNSF Agora Main Page](https://www.snf.ch/en/JnT2xEAERCgO8qQc/funding/science-communication/agora)
- [Agora Rolling Call Guidelines (PDF)](https://www.snf.ch/media/en/11uidBmLs6bQo9J3/Guidelines-for-applicants-Agora-rolling-call.pdf)
- [Agora Specialist CV Template](https://media.snf.ch/en/clUgfhGap1PBEhKa)

## Contact

For questions about Agora funding: agora@snf.ch
