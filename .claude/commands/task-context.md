---
description: Quick context discovery and relevance analysis for upcoming tasks
allowed-tools: Read, Grep, Glob, Task(Explore)
argument-hint: "<task-brief>"
category: workflow
---

# Task Context Discovery

**Task Brief:** $ARGUMENTS

---

## Workflow Instructions

Execute this streamlined discovery workflow to gather relevant context before starting work. This is "reconnaissance before coding" without the full implementation pipeline.

### Step 1: Echo & Scope

Write a brief 2-3 sentence restatement of the task to confirm understanding:
- What is being asked?
- What's the primary goal?
- What's explicitly out of scope?

### Step 2: Pre-Reading & Codebase Reconnaissance

**Search for relevant documentation:**
1. Developer guides in `developer-guides/`
2. Architecture docs and README files
3. Related spec files in `specs/`
4. E2E test plans in `e2e-testing-plans/`
5. Task dossiers in `docs/task-dossiers/`

**Search for relevant code using Task(Explore):**
- Use keywords inferred from the task brief
- Look for components, modules, utilities that will be affected
- Identify data models, database schemas, APIs
- Find tests, configuration files, feature flags
- Locate styles, layouts, UI patterns

**Build a mental map of:**
- Primary files/modules involved
- Shared dependencies (utilities, hooks, stores)
- Data flow patterns
- Potential blast radius
- Related features or previous similar work

### Step 3: Analyze & Synthesize

For each relevant item found, ask:
- **Why is this relevant?** What aspect of the task does it inform?
- **How will it influence my approach?** What constraints, patterns, or opportunities does it reveal?
- **What decisions does it help me make?** Does it suggest a particular implementation path?

### Step 4: Output Summary

Provide a concise summary organized as:

```markdown
## Task Context Summary

**Task Understanding:**
{2-3 sentence restatement of what you're being asked to do}

**Relevant Context Found:**

### Documentation
- **{Doc name}** ({path})
  - **Why relevant:** {brief explanation}
  - **Influences approach:** {how this will shape your implementation}

### Code/Components
- **{Component/Module name}** ({path})
  - **Why relevant:** {brief explanation}
  - **Influences approach:** {how this will shape your implementation}

### Tests & Patterns
- **{Test/Pattern}** ({path})
  - **Why relevant:** {brief explanation}
  - **Influences approach:** {how this will shape your implementation}

### Database/Data
- **{Schema/Model}** ({path})
  - **Why relevant:** {brief explanation}
  - **Influences approach:** {how this will shape your implementation}

**Key Insights:**
1. {Most important finding that will guide implementation}
2. {Second most important finding}
3. {Third most important finding}

**Recommended Approach:**
{1-2 paragraphs describing the high-level approach informed by the context above}

**Potential Risks/Considerations:**
- {Risk or consideration from context}
- {Risk or consideration from context}
```

---

## Principles

1. **Breadth over depth** - Survey widely, read strategically
2. **Relevance filter** - Only include context that directly informs the task
3. **Actionable insights** - Every finding should connect to "how this shapes my approach"
4. **Concise output** - Aim for clarity and brevity, not exhaustive detail

---

## Example Usage

```bash
/task-context Add magazine name extraction to the dashboard filtering UI
```

This will gather relevant context about the magazine extraction feature, dashboard code, filtering patterns, and database schema, then provide a focused summary of how this context will inform the implementation approach.
