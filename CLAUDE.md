# Grant Proposals Project

## Overview

This repository contains agentic workflows for drafting grant proposals. Each workflow is tailored to a specific funding program, with dedicated skills, subagents, and templates.

## Available Workflows

| Command | Funding Program | Skill | Output Format |
|---------|-----------------|-------|---------------|
| `/innocheck` | Innosuisse Innovationsscheck | `innocheck-guidelines` | Markdown/Word |
| `/innosuisse-project` | Innosuisse Innovation Project | `innosuisse-project-guidelines` | Markdown |
| `/snsf-project` | SNSF Project Funding | `snsf-guidelines` | LaTeX |
| `/bfh-elearning` | BFH E-Learning Förderprogramm | `bfh-elearning-guidelines` | Markdown/Word |

## Project Structure

- `.claude/commands/` - Slash commands (one per grant type)
- `.claude/agents/` - Specialized subagents (reusable across workflows)
- `.claude/skills/` - Domain-specific guidelines and templates
- `inputs/` - User context documents
- `outputs/` - Generated proposals
- `_resources/` - Implementation plans and documentation

## Workflow Conventions

When generating proposals:
1. Load the relevant skill for the grant type
2. Spawn subagents in parallel for efficiency
3. Synthesize all analyses into a coherent proposal
4. Save outputs to the `outputs/` directory
5. Validate against the funding program's evaluation criteria

## Subagents

### General Purpose (Innocheck)
- **innovation-analyst** - Assesses novelty, feasibility, and scientific merit
- **market-researcher** - Analyzes competition and market potential
- **financial-planner** - Validates budgets and funding compliance

### SNSF-Specific
- **scientific-relevance** - Evaluates significance, originality, and impact
- **methodology-analyst** - Evaluates methods, feasibility, and risks
- **track-record-analyst** - Generates CV achievements and Section 2.2
- **literature-builder** - Builds state-of-research context and bibliography
- **snsf-budget-planner** - Validates budget against SNSF rules

### Innosuisse Innovation Project-Specific
- **project-planner** - Designs work packages, Gantt charts, milestones, deliverables
- **risk-analyst** - Creates risk matrices with mitigation strategies
- **innosuisse-budget-planner** - Validates 40-60% split and notional rates

### BFH E-Learning-Specific
- **didactic-innovation-analyst** - Assesses all 6 BFH evaluation criteria (strategy, innovation, quality, sustainability, networking, transfer)
- **bfh-elearning-planner** - Creates project plans and validates budgets with BFH hourly rates

## Innosuisse Innovation Project Quick Reference

| Parameter | Value |
|-----------|-------|
| Funding | Innosuisse 40-60%, Partner 40-60% |
| Cash Contribution | Min 5-10% of Innosuisse funding |
| Duration | 6-36 months (typical: 18-24) |
| Budget | No max (typical CHF 500K-2M) |
| Submission | Innolink, 6 weeks before meeting |
| Decision Meetings | 8 per year |

**Notional Hourly Rates (Research Partner):**

| Position | Rate (CHF/hr) |
|----------|---------------|
| Full Professor | 160-180 |
| Associate Professor | 140-160 |
| Senior Researcher/Postdoc | 110-130 |
| PhD Student | 70-90 |
| Research Assistant | 60-80 |

**Key Evaluation Criteria:**
1. Level of Innovation - Scientific AND economic novelty
2. Value Creation - Benefit for Swiss economy/society
3. Project Quality - Realistic planning, competent team
4. Sustainable Development - Environmental/social contribution

## SNSF Project Funding Quick Reference

| Parameter | Value |
|-----------|-------|
| Funding Range | CHF 100,000 - 4,000,000 |
| Duration | 1-4 years |
| Per Applicant/Year | Max CHF 250,000 average |
| Per Project/Year | Max CHF 1,000,000 |
| Equipment Cap | CHF 100,000 |
| Research Plan | 15 pages / 60,000 chars |
| CV Achievements | Max 4,350 characters |
| Deadlines | 1 April, 1 October (17:00) |

**Key Evaluation Criteria** (by weight):
1. Methods & Feasibility (highest)
2. Scientific Relevance
3. Originality
4. Track Record
5. Topicality

## BFH E-Learning Förderprogramm Quick Reference

| Parameter | Value |
|-----------|-------|
| Funding Pool | CHF 100,000/year (total) |
| Typical Grant | CHF 10,000 (Stundengutschriften) |
| Course Requirement | Min. 2 ECTS |
| Required Tools | Moodle, Kaltura |
| Deadlines | 30 April (2025), 31 July (future) |
| Output Requirement | OER publication + presentation |

**Hourly Rates:**

| Position | Rate (CHF/hr) |
|----------|---------------|
| Dozierende | 104 |
| Wissenschaftliche Mitarbeitende | 70 |
| Assistierende | 52 |

**Six Evaluation Criteria:**
1. Strategy Alignment - Vielfalt, Future Skills, Vernetzung
2. Didactic Innovation - Novel teaching approaches
3. Quality Improvement - Better learning outcomes
4. Sustainability - Long-term usability
5. Networking - Collaboration between instructors
6. Transfer Potential - OER and Good Practice character

## Language

Proposals may be generated in multiple languages depending on the funding program.
- **SNSF**: English required for economics, STEM, medicine, psychology
- **Innosuisse**: German, French, Italian, or English
- **Innocheck**: German, French, Italian, or English
- **BFH E-Learning**: German only (Swiss orthography, use "ss" not "ß")

Default to the language of the input documents. If unclear, ask the user for preference.

## Quality Standards

All proposals must:
- Address all evaluation criteria for the specific funding program
- Include quantified benefits where possible
- Have realistic, justified budgets
- Define concrete, measurable deliverables
- Follow the structure required by the funding body

## Reference Materials

Successful proposal examples are available in:
- `_resources/old_proposals/snsf_project/` - Two funded SNSF proposals
- `_resources/old_proposals/bfh_elearning/` - Funded BFH E-Learning proposal
