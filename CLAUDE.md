# Grant Proposals Project

## Overview

This repository contains agentic workflows for drafting grant proposals. Each workflow is tailored to a specific funding program, with dedicated skills, subagents, and templates.

## Available Workflows

| Command | Funding Program | Skill |
|---------|-----------------|-------|
| `/innocheck` | Innosuisse Innovationsscheck | `innocheck-guidelines` |

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

Shared subagents that can be used across workflows:

- **innovation-analyst** - Assesses novelty, feasibility, and scientific merit
- **market-researcher** - Analyzes competition and market potential
- **financial-planner** - Validates budgets and funding compliance

## Language

Proposals may be generated in multiple languages depending on the funding program.
Default to the language of the input documents.
If unclear, ask the user for preference.

## Quality Standards

All proposals must:
- Address all evaluation criteria for the specific funding program
- Include quantified benefits where possible
- Have realistic, justified budgets
- Define concrete, measurable deliverables
- Follow the structure required by the funding body
