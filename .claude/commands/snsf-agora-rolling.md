---
description: Draft an SNSF Agora Rolling Call proposal from context documents
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

# SNSF Agora Rolling Call Proposal Generator

You are an expert grant proposal writer specializing in SNSF Agora science communication projects.
Generate a comprehensive 5-page project plan for the Agora Rolling Call based on the provided context documents.

**IMPORTANT:** All outputs must be in English (required by SNSF). The project plan must not exceed 5 pages.

## Input Processing

The user has provided these context files: $ARGUMENTS

### Step 1: Load and Analyze Context Documents

1. Read all provided files using appropriate tools
2. Extract and categorize information:
   - **Research Topic**: The scientific research to be communicated
   - **Communication Idea**: How the research will be shared with the public
   - **Target Audience**: Who the project aims to reach (must include Swiss public)
   - **Team**: Researcher(s) and Agora Specialist(s)
   - **Budget Estimate**: Costs for specialists, production, materials, communication

3. **Team Composition Check** (CRITICAL):
   - Must include at least 1 Researcher (min. 20% work-time allocation, SNSF Article 10 compliant)
   - Must include at least 1 Agora Specialist (communication/media/education expert)
   - If team composition is incomplete, flag this and suggest how to address it

### Step 2: Verify Eligibility

Check that the project is eligible:
- [ ] Communicates fundamental scientific knowledge
- [ ] Fosters dialogue with the public (not one-way communication)
- [ ] Primary target audience is in Switzerland
- [ ] Links to SNSF-funded research OR peer-reviewed publications
- [ ] NOT marketing, institutional communication, or technology transfer
- [ ] NOT connected to NRP/NCCR
- [ ] Budget is CHF 5,000 - 50,000

### Step 3: Activate Agora Guidelines Skill

Load the snsf-agora-guidelines skill for evaluation criteria and format requirements.
Read the evaluation criteria and common mistakes.

### Step 4: Spawn Specialized Analysis Subagents (in parallel)

Use the Task tool to spawn these subagents simultaneously:

1. **Science Communication Analyst** (@science-communication-analyst)
   - Assess content quality and research foundation
   - Analyze target audience and their needs
   - Evaluate communication methods and channels
   - Estimate qualitative and quantitative impact
   - Generate narrative text for sections 2, 3, and 5

2. **Agora Project Planner** (@agora-project-planner)
   - Verify team composition and eligibility
   - Create timeline and activity plan
   - Develop budget breakdown by category
   - Assess feasibility and risks
   - Generate narrative text for section 4

### Step 5: Synthesize 5-Page Project Plan

After all subagents complete, synthesize findings into the required structure:

**Project Plan Structure** (5 pages max):

1. **Summary** (~0.5 pages)
   - Project goal in 2-3 sentences
   - Research topic being communicated
   - Target audience
   - Main communication activities

2. **Research Context** (~1 page)
   - Description of underlying research
   - Link to SNSF-funded work or peer-reviewed publications
   - Why this research is suitable for public communication
   - Key messages to convey

3. **Communication Concept** (~1.5 pages)
   - Target audience definition and justification
   - Communication methods and channels chosen
   - Why these methods are appropriate for the audience
   - Dialogue and interaction elements (bidirectional engagement)

4. **Project Plan and Team** (~1.5 pages)
   - Activities and timeline (table format)
   - Roles of researcher(s) and Agora specialist(s)
   - Team qualifications relevant to this project
   - Collaboration approach between team members

5. **Impact and Feasibility** (~0.5 pages)
   - Expected qualitative impacts (awareness, engagement, attitude change)
   - Expected quantitative impacts (reach, participation numbers)
   - Contribution to diversity and equal opportunities
   - Risk assessment and mitigation

### Step 6: Validation Checklist

Before finalizing, verify:
- [ ] Project plan is under 5 pages
- [ ] All six evaluation criteria are explicitly addressed:
  - ✓ Content quality
  - ✓ Communication method appropriateness
  - ✓ Team expertise
  - ✓ Project feasibility
  - ✓ Expected impacts (qualitative and quantitative)
  - ✓ Diversity and equal opportunities contribution
- [ ] Team includes researcher + Agora specialist
- [ ] Budget is within CHF 5,000 - 50,000
- [ ] Target audience includes Swiss public
- [ ] Dialogue/interaction elements are present (not just one-way)
- [ ] Link to SNSF-funded research or publications is clear
- [ ] Language is English throughout

### Step 7: Generate Outputs

1. **Markdown**: Save to `outputs/snsf_agora_rolling/project_plan.md`
2. **PDF**: Convert to PDF:
   ```bash
   cd outputs/snsf_agora_rolling && pandoc project_plan.md -o project_plan.pdf --pdf-engine=xelatex -V geometry:margin=2.5cm
   ```

### Output Quality Standards

- Language: English only
- Length: Maximum 5 pages
- Style: Clear, accessible language (avoiding jargon)
- Focus: Emphasize public engagement and dialogue, not just dissemination
- Tone: Enthusiastic about science communication but realistic about impacts
