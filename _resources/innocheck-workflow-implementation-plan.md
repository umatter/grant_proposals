# Innocheck Grant Proposal Workflow: Claude Code Implementation Plan

## Executive Summary

This document provides a complete implementation plan for an agentic workflow using Claude Code to assist in drafting Innosuisse Innovationsscheck grant proposals. The workflow is initiated via a custom slash command that processes local context documents and orchestrates specialized subagents to produce a comprehensive, criteria-aligned proposal draft.

---

## 1. Understanding the Innocheck Grant

### Key Requirements (from Innosuisse)

| Aspect | Details |
|--------|---------|
| **Funding** | Up to CHF 15,000 (100% of research partner costs) |
| **Eligibility** | SMEs, startups, organizations with <250 FTEs, Swiss UID |
| **Purpose** | Preliminary study: concept development, feasibility analysis, innovation/market potential |
| **Duration** | 6 months to complete the study |
| **Restriction** | No grant received in past 2 years |

### Evaluation Criteria (Critical for Proposal)

1. **Innovation Level** (Innovationsgrad): Scientific and economic novelty
2. **Potential Benefit** (Potenzieller Nutzen): Value creation for implementation partner
3. **Partner Competencies**: Capabilities for execution and market implementation
4. **Financial Plan**: Realistic personnel and material costs
5. **Follow-up Project**: Likelihood of subsequent innovation project

### Required Application Content

- Title of preliminary study
- General info: implementation partner, research partner, contacts
- Innovation description: novelty, feasibility, research partner tasks, deliverables, competition, benefits
- Financial plan: personnel and material costs

---

## 2. Workflow Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                         USER INVOCATION                             │
│  /innocheck @research_goal.md @budget.xlsx @partner_info.md         │
└───────────────────────────────┬─────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────────┐
│                    MAIN ORCHESTRATOR                                │
│  .claude/commands/innocheck.md                                      │
│  - Parses input documents                                           │
│  - Activates Innocheck Skill                                        │
│  - Spawns specialized subagents                                     │
│  - Synthesizes final proposal                                       │
└───────────────────────────────┬─────────────────────────────────────┘
                                │
        ┌───────────────────────┼───────────────────────┐
        │                       │                       │
        ▼                       ▼                       ▼
┌───────────────┐     ┌───────────────┐     ┌───────────────┐
│ INNOVATION    │     │ MARKET        │     │ FINANCIAL     │
│ ANALYST       │     │ RESEARCHER    │     │ PLANNER       │
│ Subagent      │     │ Subagent      │     │ Subagent      │
└───────┬───────┘     └───────┬───────┘     └───────┬───────┘
        │                     │                     │
        └──────────┬──────────┴──────────┬──────────┘
                   │                     │
                   ▼                     ▼
        ┌───────────────┐     ┌───────────────┐
        │ INNOCHECK     │     │ SWISS GRANT   │
        │ SKILL         │     │ WRITING SKILL │
        │ (Guidelines)  │     │ (Style/Tone)  │
        └───────────────┘     └───────────────┘
                   │
                   ▼
        ┌─────────────────────────────────────┐
        │         FINAL OUTPUT                │
        │  - innocheck_proposal_draft.md      │
        │  - innocheck_proposal.docx          │
        │  - budget_summary.xlsx              │
        └─────────────────────────────────────┘
```

---

## 3. Project File Structure

```
your-project/
├── .claude/
│   ├── commands/
│   │   └── innocheck.md              # Main slash command
│   │
│   ├── agents/
│   │   ├── innovation-analyst.md     # Subagent for innovation assessment
│   │   ├── market-researcher.md      # Subagent for market/competition
│   │   └── financial-planner.md      # Subagent for budget validation
│   │
│   └── skills/
│       └── innocheck-guidelines/
│           ├── SKILL.md              # Main skill file
│           ├── references/
│           │   ├── evaluation-criteria.md
│           │   ├── application-structure.md
│           │   └── common-mistakes.md
│           └── templates/
│               ├── proposal-template.md
│               └── budget-template.md
│
├── CLAUDE.md                         # Project-level Claude configuration
│
└── inputs/                           # User context documents (example)
    ├── research_goal.md
    ├── budget.xlsx
    └── partner_info.md
```

---

## 4. Implementation Files

### 4.1 Main Slash Command

**File: `.claude/commands/innocheck.md`**

```markdown
---
description: Draft an Innosuisse Innovationsscheck proposal from context documents
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

# Innocheck Grant Proposal Generator

You are an expert grant proposal writer specializing in Swiss innovation funding.
Generate a comprehensive Innosuisse Innovationsscheck proposal based on the provided context documents.

## Input Processing

The user has provided these context files: $ARGUMENTS

### Step 1: Load and Analyze Context Documents

1. Read all provided files using appropriate tools (Read for text, analyze Excel with Python if needed)
2. Extract and categorize information:
   - **Research Goal**: Core innovation idea, scientific approach, expected outcomes
   - **Budget Data**: Personnel costs, material costs, timeline
   - **Partner Info**: Implementation partner details, research partner credentials
   - **Market Context**: Competition, market opportunity, value proposition

### Step 2: Activate Innocheck Skill

Load the innocheck-guidelines skill for evaluation criteria and structure guidance.
Read the evaluation criteria and common mistakes to avoid.

### Step 3: Spawn Specialized Analysis Subagents (in parallel)

Use the Task tool to spawn these subagents simultaneously:

1. **Innovation Analyst** (@innovation-analyst)
   - Assess novelty and scientific merit
   - Identify unique selling points
   - Evaluate technical feasibility
   - Generate innovation level justification

2. **Market Researcher** (@market-researcher)
   - Analyze competitive landscape
   - Assess market potential
   - Identify differentiation factors
   - Generate market opportunity narrative

3. **Financial Planner** (@financial-planner)
   - Validate budget realism
   - Check cost allocations
   - Ensure compliance with Innosuisse guidelines
   - Generate financial plan summary

### Step 4: Synthesize Proposal Draft

After all subagents complete, synthesize findings into a structured proposal:

1. Create the proposal following the official structure:
   - Title (concise, descriptive)
   - Innovation Description
   - Feasibility Analysis
   - Research Partner Tasks & Deliverables
   - Competitive Situation
   - Potential Benefits
   - Financial Plan Summary

2. Ensure all five evaluation criteria are explicitly addressed:
   - ✓ Innovation Level
   - ✓ Potential Benefit
   - ✓ Partner Competencies
   - ✓ Financial Plan Realism
   - ✓ Follow-up Project Potential

### Step 5: Generate Outputs

1. **Markdown Draft**: Save to `outputs/innocheck_proposal_draft.md`
2. **Word Document**: Generate polished `.docx` for submission preparation
3. **Budget Summary**: Create structured budget table if needed

### Quality Checklist

Before finalizing, verify:
- [ ] All required sections are complete
- [ ] Innovation uniqueness is clearly articulated
- [ ] Benefits quantified where possible
- [ ] Budget items are realistic and justified
- [ ] Research partner role is clearly defined
- [ ] Deliverables are specific and measurable
- [ ] Language is professional and precise (German or English as appropriate)
```

---

### 4.2 Innocheck Guidelines Skill

**File: `.claude/skills/innocheck-guidelines/SKILL.md`**

```markdown
---
name: innocheck-guidelines
description: |
  Guidelines and best practices for Innosuisse Innovationsscheck proposals.
  Use when drafting Swiss innovation grant applications, evaluating proposal
  quality, or understanding Innocheck evaluation criteria. Triggers on:
  Innocheck, Innovationsscheck, Innosuisse, Swiss grant, preliminary study funding.
allowed-tools:
  - Read
  - Grep
  - Glob
---

# Innosuisse Innovationsscheck Proposal Guidelines

## Overview

The Innovationsscheck funds preliminary studies up to CHF 15,000 for SMEs (<250 FTE)
to assess the feasibility of innovative ideas with a Swiss research partner.

## Quick Reference

| Criterion | Weight | Key Questions |
|-----------|--------|---------------|
| Innovation Level | High | What makes this scientifically/economically novel? |
| Potential Benefit | High | What value does this create for the implementation partner? |
| Partner Competencies | Medium | Can partners execute and commercialize? |
| Financial Plan | Medium | Are costs realistic for the proposed work? |
| Follow-up Potential | Medium | Will this lead to a larger innovation project? |

## Detailed Guidance

For evaluation criteria details, read: `references/evaluation-criteria.md`
For application structure, read: `references/application-structure.md`
For common mistakes to avoid, read: `references/common-mistakes.md`

## Writing Style Guidelines

1. **Be Specific**: Avoid vague claims; use concrete examples and metrics
2. **Highlight Novelty**: Clearly differentiate from existing solutions
3. **Quantify Benefits**: Use numbers for market size, efficiency gains, cost savings
4. **Show Competence**: Reference relevant experience, publications, prior projects
5. **Define Deliverables**: Make outputs measurable and time-bound
6. **Connect to Follow-up**: Explicitly describe the path to a larger project

## Language

Proposals may be submitted in German, French, Italian, or English.
Match the language to the implementation partner's primary business language.
```

---

**File: `.claude/skills/innocheck-guidelines/references/evaluation-criteria.md`**

```markdown
# Innocheck Evaluation Criteria - Detailed Guide

## 1. Innovation Level (Innovationsgrad)

### What Evaluators Look For
- Scientific novelty: New methods, technologies, or approaches
- Economic novelty: New business models, market applications, or value chains
- Differentiation from state-of-the-art

### How to Address
- Conduct literature/patent search to establish novelty
- Explicitly state: "Unlike existing solutions X and Y, our approach..."
- Identify the specific scientific or technological advancement
- Reference TRL (Technology Readiness Level) if applicable

### Red Flags
- Claims of novelty without evidence
- Solutions already available in the market
- Incremental improvements presented as breakthrough innovation

---

## 2. Potential Benefit (Potenzieller Nutzen)

### What Evaluators Look For
- Clear value proposition for the implementation partner
- Quantifiable benefits (cost reduction, revenue increase, efficiency gains)
- Strategic alignment with business goals

### How to Address
- Use concrete numbers: "Expected 30% reduction in production costs"
- Show market opportunity: TAM/SAM/SOM analysis
- Describe competitive advantage gained
- Outline path to commercialization

### Red Flags
- Vague benefit descriptions
- No clear business case
- Benefits only for the research partner

---

## 3. Partner Competencies (Kompetenzen der Projektpartner)

### What Evaluators Look For
- Implementation partner: Industry expertise, market access, resources
- Research partner: Scientific credentials, relevant experience, infrastructure
- Complementary skills between partners

### How to Address
- List relevant prior projects and publications
- Describe existing infrastructure and equipment
- Highlight team expertise and track record
- Show history of successful collaboration (if any)

### Red Flags
- Mismatched expertise to project requirements
- Lack of commercialization experience
- No clear role definition for each partner

---

## 4. Financial Plan (Finanzplan)

### What Evaluators Look For
- Realistic cost estimates
- Appropriate allocation between personnel and materials
- Justification for all expenses
- Compliance with funding rules

### Innosuisse Guidelines
- Maximum CHF 15,000 for the preliminary study
- 100% of research partner costs are covered
- Personnel costs: Based on standard rates
- Material costs: Must be directly project-related

### How to Address
- Break down costs by work package
- Use standard hourly/daily rates
- Justify any large material expenditures
- Align costs with proposed activities

### Red Flags
- Budget doesn't match scope of work
- Unexplained large expenses
- Missing cost categories

---

## 5. Follow-up Project (Folgeprojekt)

### What Evaluators Look For
- Clear path to a larger innovation project
- Defined milestones and decision points
- Realistic timeline for next steps
- Commitment from implementation partner

### How to Address
- Describe the envisioned innovation project
- Outline what the preliminary study will enable
- Mention potential for IP development
- Indicate investment readiness post-study

### Red Flags
- No clear next steps defined
- Preliminary study as standalone activity
- Unrealistic commercialization timeline
```

---

**File: `.claude/skills/innocheck-guidelines/references/application-structure.md`**

```markdown
# Innocheck Application Structure

## Required Sections

### 1. Title (Titel der Vorstudie)
- Concise and descriptive (max 100 characters recommended)
- Include key technology/innovation keywords
- Avoid jargon and acronyms

### 2. General Information (Allgemeine Informationen)

#### Implementation Partner (Umsetzungspartner)
- Company name and UID
- Contact person details
- Company size (FTE count)
- Brief company description

#### Research Partner (Forschungspartner)
- Institution name
- Contact person and credentials
- Relevant expertise areas
- Previous collaboration history (if any)

### 3. Innovation Description

#### 3.1 Novelty and Innovation Content
- What is new about this approach?
- Scientific/technological advancement
- Differentiation from existing solutions

#### 3.2 Feasibility of Results (Umsetzbarkeit der Resultate)
- Technical feasibility assessment
- Required resources and capabilities
- Risk analysis and mitigation

#### 3.3 Research Partner Tasks
- Specific activities to be performed
- Timeline and milestones
- Expertise contribution

#### 3.4 Deliverables (Lieferbares Ergebnis)
- Concrete outputs of the preliminary study
- Reports, prototypes, analyses, etc.
- Measurable success criteria

#### 3.5 Competitive Situation (Wettbewerbssituation)
- Market landscape analysis
- Key competitors and alternatives
- Competitive advantage

#### 3.6 Potential Benefit (Potenzieller Nutzen)
- Value for implementation partner
- Quantified impact metrics
- Strategic importance

### 4. Financial Plan (Finanzplan)

#### Personnel Costs
- Researcher hours/days
- Hourly/daily rates
- Role descriptions

#### Material Costs
- Equipment, consumables
- Software licenses
- External services (if any)

---

## Formatting Guidelines

1. Use clear section headers
2. Keep paragraphs focused and concise
3. Use bullet points for lists
4. Include tables for complex data
5. Ensure consistent terminology
6. Proofread for grammar and clarity
```

---

**File: `.claude/skills/innocheck-guidelines/references/common-mistakes.md`**

```markdown
# Common Mistakes in Innocheck Applications

## Content Mistakes

### 1. Vague Innovation Claims
❌ "Our solution is innovative and unique"
✅ "Unlike existing solution X which uses approach A, our method employs novel technique B, resulting in 40% improved efficiency"

### 2. Missing Competitor Analysis
❌ "There are no competitors in this space"
✅ "Current alternatives include X, Y, and Z. Our approach differs through..."

### 3. Unclear Deliverables
❌ "A report on the feasibility study"
✅ "A 20-page technical feasibility report including: prototype specifications, cost-benefit analysis, and IP landscape assessment"

### 4. No Follow-up Vision
❌ (No mention of next steps)
✅ "This preliminary study will determine go/no-go criteria for a subsequent 18-month innovation project with target budget of CHF 500,000"

### 5. Research Partner as Service Provider
❌ (Research partner only executes tasks)
✅ "The research partner contributes unique expertise in [area] and will co-develop the methodology"

---

## Structural Mistakes

### 6. Missing Required Sections
- Ensure ALL sections from the template are addressed
- Empty sections raise red flags

### 7. Inconsistent Information
- Budget must match proposed activities
- Timeline must be realistic for 6-month limit
- Team capabilities must match project needs

### 8. Wrong Language/Tone
- Avoid marketing language
- Use precise technical/scientific terminology
- Be objective, not promotional

---

## Financial Mistakes

### 9. Unrealistic Budget
- Costs too low: Appears unfeasible
- Costs too high: Raises questions about scope
- No justification: Automatic concern

### 10. Improper Cost Allocation
- Personnel costs should dominate (typically 70-80%)
- Material costs need specific justification
- No overhead or indirect costs allowed

---

## Process Mistakes

### 11. Eligibility Issues
- Partner received grant in past 2 years
- Organization >250 FTE
- Missing UID number

### 12. Submission Errors
- Incomplete forms
- Missing signatures/confirmations
- Wrong submission portal
```

---

**File: `.claude/skills/innocheck-guidelines/templates/proposal-template.md`**

```markdown
# [TITLE: Preliminary Study on ...]

## 1. General Information

### Implementation Partner
- **Company**: [Name]
- **UID**: CHE-XXX.XXX.XXX
- **Contact**: [Name, Email, Phone]
- **Size**: [XX] FTE
- **Description**: [2-3 sentences about the company]

### Research Partner
- **Institution**: [University/Research Institute]
- **Contact**: [Name, Title, Email]
- **Expertise**: [Relevant research areas]

---

## 2. Innovation Description

### 2.1 Novelty and Innovation Content
[Describe what makes this approach scientifically and economically novel]

**Key Innovation Points:**
- [Point 1]
- [Point 2]
- [Point 3]

**Differentiation from State-of-the-Art:**
[Explain how this differs from existing solutions]

### 2.2 Feasibility Assessment
[Assess technical and economic feasibility]

**Technical Feasibility:**
- [Assessment point 1]
- [Assessment point 2]

**Risk Analysis:**
| Risk | Probability | Mitigation |
|------|-------------|------------|
| [Risk 1] | [Low/Medium/High] | [Strategy] |
| [Risk 2] | [Low/Medium/High] | [Strategy] |

### 2.3 Research Partner Tasks
[Specific activities the research partner will perform]

| Task | Duration | Deliverable |
|------|----------|-------------|
| [Task 1] | [X weeks] | [Output 1] |
| [Task 2] | [X weeks] | [Output 2] |
| [Task 3] | [X weeks] | [Output 3] |

### 2.4 Deliverables
[Concrete, measurable outputs]

1. **[Deliverable 1]**: [Description and success criteria]
2. **[Deliverable 2]**: [Description and success criteria]
3. **[Deliverable 3]**: [Description and success criteria]

### 2.5 Competitive Situation
[Analysis of competitors and alternatives]

| Competitor/Alternative | Approach | Our Advantage |
|------------------------|----------|---------------|
| [Competitor 1] | [Their approach] | [Why ours is better] |
| [Competitor 2] | [Their approach] | [Why ours is better] |

### 2.6 Potential Benefit
[Quantified value for the implementation partner]

**Direct Benefits:**
- [Benefit 1 with metrics]
- [Benefit 2 with metrics]

**Strategic Value:**
[How this aligns with business strategy]

---

## 3. Financial Plan

### Personnel Costs
| Role | Days | Rate (CHF/day) | Total (CHF) |
|------|------|----------------|-------------|
| [Senior Researcher] | [X] | [XXX] | [X,XXX] |
| [Research Assistant] | [X] | [XXX] | [X,XXX] |
| **Subtotal** | | | **[X,XXX]** |

### Material Costs
| Item | Justification | Amount (CHF) |
|------|---------------|--------------|
| [Item 1] | [Why needed] | [XXX] |
| [Item 2] | [Why needed] | [XXX] |
| **Subtotal** | | **[X,XXX]** |

### Total Budget
| Category | Amount (CHF) |
|----------|--------------|
| Personnel | [X,XXX] |
| Materials | [X,XXX] |
| **Total** | **[XX,XXX]** |

---

## 4. Follow-up Project Outlook

### Envisioned Innovation Project
[Description of the larger project this preliminary study enables]

### Decision Criteria
[What the preliminary study will determine for go/no-go decision]

### Timeline
- **Preliminary Study Completion**: [Month Year]
- **Go/No-Go Decision**: [Month Year]
- **Innovation Project Start**: [Month Year]
```

---

### 4.3 Subagent Definitions

**File: `.claude/agents/innovation-analyst.md`**

```markdown
---
name: innovation-analyst
description: Analyzes innovation level and scientific novelty for grant proposals
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
---

# Innovation Analyst Subagent

You are a specialized analyst focused on assessing innovation and scientific novelty
for Swiss grant proposals, particularly Innosuisse funding applications.

## Your Responsibilities

1. **Novelty Assessment**
   - Identify the core innovation in the project
   - Evaluate scientific and technological advancement
   - Compare against state-of-the-art

2. **Differentiation Analysis**
   - Identify unique aspects of the approach
   - Articulate competitive advantages
   - Highlight intellectual property potential

3. **Feasibility Evaluation**
   - Assess technical readiness
   - Identify implementation challenges
   - Evaluate resource requirements

## Output Format

Provide your analysis in this structure:

```markdown
## Innovation Analysis Report

### Core Innovation
[Description of the key innovation]

### Scientific/Technological Novelty
[What advances the state-of-the-art]

### Unique Selling Points
1. [USP 1]
2. [USP 2]
3. [USP 3]

### Feasibility Assessment
- Technical Readiness: [High/Medium/Low]
- Implementation Complexity: [High/Medium/Low]
- Resource Requirements: [Summary]

### Recommended Narrative
[Suggested text for the innovation section of the proposal]
```

## Analysis Guidelines

- Focus on WHAT is new, not just WHAT is being done
- Quantify improvements where possible
- Reference Technology Readiness Levels (TRL) when applicable
- Consider both incremental and disruptive innovation
- Identify patentable elements
```

---

**File: `.claude/agents/market-researcher.md`**

```markdown
---
name: market-researcher
description: Analyzes market potential and competitive landscape for grant proposals
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - WebSearch
---

# Market Researcher Subagent

You are a specialized market analyst focused on evaluating commercial potential
and competitive positioning for innovation projects.

## Your Responsibilities

1. **Competitive Analysis**
   - Identify direct and indirect competitors
   - Analyze competitor strengths/weaknesses
   - Map competitive positioning

2. **Market Opportunity Assessment**
   - Estimate market size (TAM/SAM/SOM)
   - Identify target customer segments
   - Evaluate market trends

3. **Value Proposition Development**
   - Articulate customer benefits
   - Quantify value creation
   - Define differentiation factors

## Output Format

```markdown
## Market Analysis Report

### Competitive Landscape
| Competitor | Solution Type | Strengths | Weaknesses | Our Advantage |
|------------|---------------|-----------|------------|---------------|
| [Name] | [Type] | [Strengths] | [Weaknesses] | [How we differ] |

### Market Opportunity
- **Total Addressable Market (TAM)**: [CHF/EUR X million]
- **Serviceable Addressable Market (SAM)**: [CHF/EUR X million]
- **Serviceable Obtainable Market (SOM)**: [CHF/EUR X million]

### Target Segments
1. [Segment 1]: [Size and characteristics]
2. [Segment 2]: [Size and characteristics]

### Value Proposition
[Clear statement of customer value]

### Quantified Benefits
- [Benefit 1]: [Metric]
- [Benefit 2]: [Metric]

### Recommended Narrative
[Suggested text for competitive/market sections of the proposal]
```

## Analysis Guidelines

- Use publicly available data and cite sources
- Be realistic about market sizing
- Focus on Swiss/European markets first
- Consider regulatory environment
- Identify potential barriers to entry
```

---

**File: `.claude/agents/financial-planner.md`**

```markdown
---
name: financial-planner
description: Validates and optimizes budget plans for Innocheck proposals
allowed-tools:
  - Read
  - Bash
  - Grep
  - Glob
---

# Financial Planner Subagent

You are a specialized financial analyst focused on grant budget planning
for Swiss research funding, particularly Innosuisse requirements.

## Your Responsibilities

1. **Budget Validation**
   - Check compliance with Innosuisse rules
   - Verify personnel cost calculations
   - Validate material cost justifications

2. **Cost Optimization**
   - Ensure budget matches scope
   - Identify potential gaps or overruns
   - Optimize cost allocation

3. **Financial Narrative**
   - Justify all cost items
   - Explain resource allocation logic
   - Connect costs to deliverables

## Innosuisse Budget Rules

- Maximum total: CHF 15,000
- 100% of research partner costs covered
- Personnel costs: Standard institutional rates
- Material costs: Must be project-specific
- No overhead or indirect costs

## Output Format

```markdown
## Financial Analysis Report

### Budget Summary
| Category | Proposed (CHF) | Validated (CHF) | Notes |
|----------|----------------|-----------------|-------|
| Personnel | [X,XXX] | [X,XXX] | [Assessment] |
| Materials | [X,XXX] | [X,XXX] | [Assessment] |
| **Total** | **[X,XXX]** | **[X,XXX]** | |

### Personnel Cost Breakdown
| Role | Days | Rate | Total | Justification |
|------|------|------|-------|---------------|
| [Role] | [X] | [XXX] | [X,XXX] | [Why this effort] |

### Material Cost Breakdown
| Item | Amount | Justification |
|------|--------|---------------|
| [Item] | [XXX] | [Why needed] |

### Compliance Check
- [ ] Within CHF 15,000 limit
- [ ] Personnel costs reasonable
- [ ] Materials directly project-related
- [ ] No prohibited cost types

### Issues Identified
[List any problems found]

### Recommendations
[Suggestions for improvement]

### Recommended Financial Narrative
[Suggested text for financial plan section]
```

## Analysis Guidelines

- Use standard Swiss research rates as benchmarks
- Ensure days/effort match deliverables
- Flag any unusual expenses
- Recommend reallocation if needed
```

---

### 4.4 Project CLAUDE.md

**File: `CLAUDE.md`**

```markdown
# Innocheck Proposal Workflow Project

## Overview
This project contains an agentic workflow for drafting Innosuisse Innovationsscheck grant proposals.

## Key Commands

- `/innocheck @file1.md @file2.xlsx ...` - Generate a proposal draft from context files

## Project Structure

- `.claude/commands/` - Custom slash commands
- `.claude/agents/` - Specialized subagents
- `.claude/skills/innocheck-guidelines/` - Innocheck-specific skill with guidelines
- `inputs/` - Place context documents here
- `outputs/` - Generated proposals appear here

## Workflow Behavior

When generating proposals:
1. Always load the innocheck-guidelines skill first
2. Spawn subagents in parallel for efficiency
3. Synthesize all analyses into a coherent proposal
4. Generate both Markdown and Word document outputs
5. Validate against all five Innosuisse evaluation criteria

## Language

Proposals may be in German, French, Italian, or English.
Default to the language of the input documents.
If unclear, ask the user for preference.

## Quality Standards

All proposals must:
- Address all five evaluation criteria explicitly
- Include quantified benefits where possible
- Have realistic, justified budgets
- Define concrete, measurable deliverables
- Articulate a clear path to follow-up project
```

---

## 5. Step-by-Step Implementation Guide

### Phase 1: Setup (15 minutes)

```bash
# Navigate to your project
cd your-project

# Create directory structure
mkdir -p .claude/commands
mkdir -p .claude/agents
mkdir -p .claude/skills/innocheck-guidelines/references
mkdir -p .claude/skills/innocheck-guidelines/templates
mkdir -p inputs
mkdir -p outputs
```

### Phase 2: Create Core Files (30 minutes)

1. Copy each file from Section 4 to its respective location
2. Verify file structure matches Section 3

### Phase 3: Test Basic Functionality (15 minutes)

```bash
# Start Claude Code
claude

# Verify command is available
/help
# Should show: /innocheck - Draft an Innosuisse Innovationsscheck proposal...

# Test skill loading
> "What are the Innocheck evaluation criteria?"
# Should trigger the innocheck-guidelines skill
```

### Phase 4: Prepare Test Input Documents (20 minutes)

Create sample input files in `inputs/`:

**`inputs/research_goal.md`** (example):
```markdown
# Research Goal: AI-Powered Quality Control for Precision Manufacturing

## Innovation Idea
Develop a machine learning system for real-time defect detection in CNC machining.

## Scientific Approach
- Computer vision with custom CNN architecture
- Edge computing for real-time inference
- Integration with existing SCADA systems

## Expected Outcomes
- Proof-of-concept prototype
- Accuracy benchmarks vs. human inspection
- Integration feasibility assessment
```

**`inputs/partner_info.md`** (example):
```markdown
# Partner Information

## Implementation Partner
- Company: SwissPrecision AG
- UID: CHE-123.456.789
- FTE: 45
- Industry: Precision manufacturing
- Contact: Hans Mueller, CTO, hans@swissprecision.ch

## Research Partner
- Institution: ETH Zürich, Institute for Machine Learning
- Contact: Prof. Dr. Anna Schmidt
- Expertise: Computer vision, industrial AI applications
- Previous collaboration: None (new partnership)
```

### Phase 5: Full Workflow Test (20 minutes)

```bash
# Start Claude Code
claude

# Run the full workflow
/innocheck inputs/research_goal.md inputs/partner_info.md

# Expected outputs:
# - outputs/innocheck_proposal_draft.md
# - outputs/innocheck_proposal.docx
```

---

## 6. Advanced Customizations

### 6.1 Adding Web Research Capability

Add MCP server for web search:

```bash
# Add Brave Search MCP
claude mcp add brave-search -- npx @anthropic-ai/mcp-server-brave-search
```

Update market-researcher agent to use web search for competitor analysis.

### 6.2 Excel Budget Processing

For complex budget files, add a Python script to `.claude/skills/innocheck-guidelines/scripts/`:

```python
# scripts/parse_budget.py
import pandas as pd
import sys
import json

def parse_budget(filepath):
    """Parse Excel budget file and return structured data."""
    df = pd.read_excel(filepath)
    
    # Adapt this to your budget template structure
    personnel = df[df['Category'] == 'Personnel']
    materials = df[df['Category'] == 'Materials']
    
    return {
        'personnel': personnel.to_dict('records'),
        'materials': materials.to_dict('records'),
        'total': df['Amount'].sum()
    }

if __name__ == '__main__':
    result = parse_budget(sys.argv[1])
    print(json.dumps(result, indent=2))
```

### 6.3 Multi-Language Support

Add language detection and translation guidance to the skill:

```markdown
## Language Handling

1. Detect primary language of input documents
2. If German: Use formal German (Sie-Form)
3. If French: Use standard French (no Swiss-specific terms unless appropriate)
4. If English: Use British English for Swiss context
5. Technical terms: Keep in original language if industry-standard
```

### 6.4 Integration with Document Generation

For Word document generation, leverage the docx skill:

```markdown
## Document Generation

After creating the Markdown draft:
1. Load the docx skill
2. Generate a properly formatted Word document
3. Include:
   - Professional header with project title
   - Proper heading hierarchy
   - Formatted tables for budget
   - Page numbers in footer
```

---

## 7. Troubleshooting

### Common Issues

| Issue | Solution |
|-------|----------|
| Skill not loading | Check SKILL.md is in correct location with valid YAML frontmatter |
| Subagents not spawning | Verify Task tool is in allowed-tools list |
| Command not appearing | Ensure .md file is in .claude/commands/ with valid frontmatter |
| Budget parsing fails | Install pandas: `pip install pandas openpyxl` |

### Debug Mode

```bash
# Run Claude Code with debug output
claude --debug

# Check which skills/commands are loaded
> /help
```

---

## 8. Next Steps

1. **Customize templates** to match your specific research domains
2. **Add domain-specific skills** for your industry vertical
3. **Create feedback loop** to improve proposals based on success rates
4. **Integrate with project management** tools for timeline tracking
5. **Build evaluation scoring** to self-assess proposals before submission

---

## Appendix: Quick Reference Card

```
┌─────────────────────────────────────────────────────────────┐
│                 INNOCHECK WORKFLOW                          │
├─────────────────────────────────────────────────────────────┤
│ COMMAND:  /innocheck @file1 @file2 ...                      │
│                                                             │
│ INPUTS:   - Research goal document (required)               │
│           - Budget spreadsheet (recommended)                │
│           - Partner information (recommended)               │
│           - Any additional context files                    │
│                                                             │
│ OUTPUTS:  - outputs/innocheck_proposal_draft.md             │
│           - outputs/innocheck_proposal.docx                 │
│                                                             │
│ CRITERIA: 1. Innovation Level                               │
│           2. Potential Benefit                              │
│           3. Partner Competencies                           │
│           4. Financial Plan                                 │
│           5. Follow-up Project                              │
│                                                             │
│ LIMIT:    CHF 15,000 / 6 months / <250 FTE                  │
└─────────────────────────────────────────────────────────────┘
```
