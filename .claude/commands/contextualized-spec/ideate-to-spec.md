---
description: Transform ideation document into context-aligned, validated specification
allowed-tools: Read, Grep, Glob, Write, SlashCommand(/spec:create:*), SlashCommand(/spec:validate:*)
argument-hint: "<path-to-ideation-doc>"
category: workflow
---

# Ideate → Spec Workflow (Context-Grounded)

**Ideation Document:** $ARGUMENTS

---

## Workflow Instructions

This command bridges the gap between ideation and implementation by transforming an ideation document into a validated, implementation-ready specification. This version ensures the resulting spec is grounded in the project's context ecosystem.

Follow each step sequentially.

### Step 0: Load Context Foundation

**NEW — Before processing the ideation document, load the project's context.**

1. Check for context documents:
   - `.claude/context/PROJECT_CONTEXT.md`
   - `.claude/context/SOUL_DOCUMENT.md`
   - `.claude/context/PROJECT_OWNER.md`

2. If found, extract:
   - Current phase and acceptance criteria
   - Project vision and identity
   - Decision log
   - Constraints and non-goals

3. Hold this context throughout the workflow — it will inform:
   - How you frame decisions for the user (Step 2)
   - Whether additional specs are needed (Step 3)
   - The spec creation prompt (Step 4)
   - Validation criteria (Step 6)

4. If not found, note absence and proceed normally.

### Step 1: Read & Synthesize Ideation Document

1. Read the ideation document at the provided path
2. Extract and synthesize:
   - **Context Grounding** (Section 0, if present) — How this task fits the project
   - **Intent & Assumptions** (Section 1) — What we're building and why
   - **Codebase Map** (Section 3) — Components/modules that will be affected
   - **Root Cause Analysis** (Section 4, if present) — Bug context
   - **Research Findings** (Section 5) — Recommended approach and alternatives
   - **Clarifications** (Section 6) — Open questions from ideation

3. **Cross-reference with project context:**
   - Does the ideation's scope fit the current phase?
   - Do any proposed approaches conflict with logged decisions?
   - Are there constraints the ideation didn't account for?

### Step 2: Interactive Decision Gathering

Review the clarifications from Section 6 of the ideation document. For each clarification:

1. **Present the decision point clearly** to the user with:
   - Context from the ideation research
   - Recommended option (if any) from Section 5
   - Pros/cons of alternatives (if applicable)
   - Impact on implementation complexity/scope
   - **Context alignment note** — flag if any option would conflict with project context

2. **Ask the user to decide** with specific options when possible:
   - Multiple choice format for clear alternatives
   - Open-ended questions for creative/architectural decisions
   - Default recommendations to speed up decision-making

3. **Record decisions** in a structured format:
   ```
   Decision {N}: {Question}
   User choice: {Answer}
   Rationale: {Why this matters for the spec}
   Context note: {Any alignment considerations}
   ```

**Example interaction format:**
```
Decision 1: Image proxy URL construction
From research: We can either construct proxy URLs in the plugin OR in banner-data.json

Options:
  A) Plugin constructs URLs (recommended) - More flexible, handles edge cases
  B) Pre-construct in banner-data.json - Simpler, but less dynamic

Context note: PROJECT_CONTEXT notes that we prefer logic in the application layer
over pre-computed data when possible (see: Architectural Principles)

Which approach do you prefer? [A/B or your own approach]
```

### Step 3: Identify Additional Specifications Needed

Based on the ideation document, user decisions, and project context:

1. **Determine specification scope:**
   - Is this a single feature/fix or does it need multiple specs?
   - Are there prerequisite changes needed first?
   - Should any parts be deferred to follow-up work?
   - **Does the project context suggest splitting this differently?** (e.g., some work belongs in current phase, some in next)

2. **Ask the user:**
   - "Should I create one comprehensive spec or break this into multiple smaller specs?"
   - "Are there any parts of the ideation that should be out-of-scope for the initial spec?"
   - If applicable: "The project context places [X] in a future phase — should we defer that?"

3. **Record the specification plan:**
   ```
   Primary spec: {description}
   Additional specs (if any): {list}
   Deferred work: {list}
   Phase-deferred: {items pushed to future phases, with phase reference}
   ```

### Step 4: Build Spec Creation Prompt

Construct a rich, detailed prompt for `/spec:create` that includes:

1. **Task description** (from ideation Intent + user decisions):
   - Clear, imperative statement of what to build/fix
   - Include "why" context from ideation research
   - Reference the recommended approach from Section 5

2. **Technical context** (from Codebase Map):
   - Files/components that will be modified (with paths)
   - Data flow and dependencies
   - Potential blast radius

3. **Implementation constraints** (from decisions + ideation + project context):
   - User decisions made in Step 2
   - Architectural choices from research
   - Out-of-scope items
   - **Project-level constraints** from context documents (decisions, non-goals, architectural principles)

4. **Acceptance criteria** (inferred from ideation + context):
   - User-visible outcomes
   - Technical requirements
   - Non-regression requirements
   - **Phase acceptance criteria** that this work advances (from context docs)

### Step 5: Execute Spec Creation

1. **Inform the user:**
   ```
   Creating specification with the following scope:
   - {Primary task description}
   - {Key technical constraints}
   - {Main acceptance criteria}
   - {Context alignment: current phase, relevant decisions}

   Proceeding with /spec:create...
   ```

2. **Execute `/spec:create`** with the constructed prompt from Step 4

3. **Capture the spec file path** from the command output

### Step 6: Validate the Specification

1. **Execute `/spec:validate`** on the newly created spec file

2. **Capture validation results:**
   - Completeness score
   - Missing elements (if any)
   - Validation warnings or recommendations
   - Implementation readiness assessment
   - **Context alignment assessment** (aligned/misaligned/partially aligned)

### Step 7: Present Summary & Next Steps

Create a comprehensive summary for the user:

```markdown
## Specification Summary

**Spec Location:** {path/to/spec.md}
**Validation Status:** {PASS/NEEDS_WORK}
**Completeness Score:** {score}/10
**Context Alignment:** {Aligned/Partially Aligned/Not Evaluated}

### What Was Specified

1. {Key feature/fix described}
2. {Technical approach chosen}
3. {Implementation scope}

### Context Alignment

- **Current phase:** {phase name}
- **Phase criteria advanced:** {which acceptance criteria this work addresses}
- **Decisions referenced:** {any project decisions that informed the spec}
- **Constraints honored:** {any constraints that shaped the scope}

### Decisions Made

{List all decisions from Step 2 with user's choices}

### Validation Results

{Summary of /spec:validate output}

### Remaining Decisions (if any)

{List any decisions that still need to be made before implementation}
- [ ] {Decision 1}
- [ ] {Decision 2}

### Recommended Next Steps

1. [ ] Review the specification at {spec-path}
2. [ ] {If validation failed: Address validation feedback}
3. [ ] {If validation passed: Execute with /spec:execute {spec-path}}
4. [ ] {Any follow-up specs needed}

### Deferred Work

{Any items explicitly deferred during ideation or spec creation}
{Items deferred to specific future phases, with phase references}
```

---

## Example Usage

```bash
/ideate-to-spec docs/ideation/add-proxy-config-to-figma-plugin.md
```

This will:
1. Load project context for grounding
2. Read your ideation document
3. Walk you through clarification decisions (with context alignment notes)
4. Create a detailed specification using `/spec:create`
5. Validate it with `/spec:validate` (including context alignment)
6. Present a summary with next steps

---

## Notes

- **Interactive by design:** This command MUST pause and ask the user for decisions
- **Context preservation:** All ideation research AND project context carry forward into the spec
- **Validation feedback loop:** If validation fails, summarize what needs fixing
- **Traceability:** Link spec back to ideation document AND context documents for full provenance
- **Phase awareness:** Actively flag when work should be deferred to a future phase
