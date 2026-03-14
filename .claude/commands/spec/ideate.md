---
description: Structured ideation with documentation
allowed-tools: Read, Grep, Glob, Bash(git:*), Bash(npm:*), Bash(npx:*), Task(playwright-expert)
argument-hint: "<task-brief>"
category: workflow
---

# Preflight ▸ Discovery ▸ Plan

**Task Brief:** $ARGUMENTS

---

## Workflow Instructions

Execute this structured engineering workflow for ideation that enforces complete investigation for any code-change task (bug fix or feature). Follow each step sequentially.

### Step 0: Establish Current Date

**CRITICAL:** Before any research, determine the current date:

```bash
date "+%B %Y"  # e.g., "March 2026"
```

Store this as `{current-month-year}` and use it in ALL web searches to ensure results are current. For example:
- "React Server Components best practices {current-month-year}"
- "Next.js 14 App Router patterns {current-month-year}"

This prevents outdated 2023/2024 results from polluting research findings.

### Step 1: Create Task Slug & Setup

1. Create a URL-safe slug from the task brief (e.g., "fix-chat-scroll-bug")
2. Create output directory: `mkdir -p docs/ideation`

This output directory will be where you create the ideation documenation md file.

### Step 2: Echo & Scope

Write an "Intent & Assumptions" block that:
- Restates the task brief in 1-3 sentences
- Lists explicit assumptions
- Lists what's explicitly out-of-scope to avoid scope creep

This becomes the opening of the ideation file.

### Step 3: Pre-Reading & Codebase Mapping

1. Scan repository for:
   - Developer guides in `developer-guides/`
   - Architecture docs in the root directory
   - README files
   - Related spec files in `specs/`

2. Search for relevant code using keywords inferred from task:
   - Components, hooks, utilities
   - Styles and layout files
   - Data access patterns
   - Feature flags or config

3. Build a dependency/context map:
   - Primary components/modules (with file paths)
   - Shared dependencies (theme/hooks/utils/stores)
   - Data flow (source → transform → render)
   - Feature flags/config
   - Potential blast radius

Record findings under **Pre-reading Log** and **Codebase Map** sections.

### Step 4: Root Cause Analysis (bugs only)

If the task is a bug fix for existing functionality:

1. Reproduce the issue or model the feature behavior locally
2. Capture:
   - Reproduction steps (numbered)
   - Observed vs expected behavior
   - Relevant logs or error messages
   - Screenshots if UI-related

3. Identify plausible root-cause hypotheses with evidence:
   - Code lines, props/state issues
   - CSS/layout rules
   - Event handlers, race conditions
   - API or data flow issues

4. Select the most likely hypothesis and explain why

Record under **Root Cause Analysis**.

### Step 5: Research

1. Consult the research-expert agent to conduct comprehensive research into potential solutions to the task
   - **IMPORTANT:** Include `{current-month-year}` (from Step 0) in all search queries to get current results
   - Example: "TypeScript 5 satisfies operator patterns March 2026" not just "TypeScript satisfies operator"
2. Consider which potential solutions are most appropriate for this code base by exploring the local repo further if necessary
3. Summarize the most promising potential approaches, the pros and cons of each, and an ultimate recommendation.

Record findings under **Research Findings**

### Step 6: Clarification

1. Create a list of any unspecified requirements or clarification that would be helpful for the user to decide upon

### Step 7: Write ideation document

Create `docs/ideation/{slug}.md` with the following structure:

```markdown
# {Task Title}

**Slug:** {slug}
**Author:** Claude Code
**Date:** {current-date}
**Branch:** preflight/{slug}
**Related:** {links-to-issues/PRs/specs}

---

## 1) Intent & Assumptions
- **Task brief:** {task description}
- **Assumptions:** {bulleted list}
- **Out of scope:** {bulleted list}

## 2) Pre-reading Log
{List files/docs read with 1-2 line takeaways}
- `path/to/file`: takeaway...

## 3) Codebase Map
- **Primary components/modules:** {paths + roles}
- **Shared dependencies:** {theme/hooks/utils/stores}
- **Data flow:** {source → transform → render}
- **Feature flags/config:** {flags, env, owners}
- **Potential blast radius:** {areas impacted}

## 4) Root Cause Analysis
- **Repro steps:** {numbered list}
- **Observed vs Expected:** {concise description}
- **Evidence:** {code refs, logs, CSS/DOM snapshots}
- **Root-cause hypotheses:** {bulleted with confidence}
- **Decision:** {selected hypothesis + rationale}

## 5) Research
- **Potential solutions:** {numbered list with pros and cons for each}
- **Recommendation** {consise description}

## 6) Clarification
- **Clarifications:** {numbered list with decisions for the user to clarify}


```


---

## Example Usage

```bash
/ideate Fix chat UI auto-scroll bug when messages exceed viewport height
```

This will execute the full workflow, creating comprehensive ideation document at `docs/ideation/fix-chat-ui-auto-scroll-bug.md` and guide you through discovery of the task.