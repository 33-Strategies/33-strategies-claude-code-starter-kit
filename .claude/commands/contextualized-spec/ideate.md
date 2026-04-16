---
description: Context-grounded ideation with documentation
allowed-tools: Read, Grep, Glob, Bash(git:*), Bash(npm:*), Bash(npx:*), Task(playwright-expert)
argument-hint: "<task-brief>"
category: workflow
---

# Preflight ▸ Discovery ▸ Plan

**Task Brief:** $ARGUMENTS

---

## Workflow Instructions

Execute this structured engineering workflow for ideation that enforces complete investigation for any code-change task (bug fix or feature). This version integrates the **Context Ecosystem** — loading the project's core context documents to ground every ideation in the project's identity, current phase, and strategic direction.

Follow each step sequentially.

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

This output directory will be where you create the ideation documentation md file.

### Step 2: Load Context Foundation

**NEW — Context Ecosystem Integration**

Before scoping or researching, ground yourself in the project's identity and current state.

1. **Locate the context triad** — check these locations in order:
   - `.claude/context/PROJECT_CONTEXT.md`
   - `.claude/context/SOUL_DOCUMENT.md`
   - `.claude/context/PROJECT_OWNER.md`
   - Also check project root for any of these files

2. **If found, extract and record:**
   - **Project identity:** What is this project? Who is it for? What is the vision?
   - **Current phase/milestone:** Where are we in the build? What are the active acceptance criteria?
   - **Decision log:** What has already been decided that this ideation should respect?
   - **Constraints & non-goals:** What is explicitly out of bounds for this project?
   - **Active priorities:** What is the project currently focused on?

3. **Write a "Context Grounding" block** that frames the task brief against the project's stated goals:
   - Does this task align with the current phase?
   - Does it serve the project's stated users and vision?
   - Does it conflict with any logged decisions or stated non-goals?
   - If there's a potential conflict, flag it explicitly for the user.

4. **If no context documents are found:**
   - Note this in the ideation document
   - Recommend the user create them (link to context ecosystem docs)
   - Proceed with standard ideation flow

Record findings under **Context Grounding** section.

### Step 3: Echo & Scope

Write an "Intent & Assumptions" block that:
- Restates the task brief in 1-3 sentences
- Lists explicit assumptions
- Lists what's explicitly out-of-scope to avoid scope creep
- **References the context grounding** — tie scope decisions back to project phase, vision, or constraints where applicable

This becomes the opening of the ideation file.

### Step 4: Pre-Reading & Codebase Mapping

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

### Step 5: Root Cause Analysis (bugs only)

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

### Step 6: Research

1. Consult the research-expert agent to conduct comprehensive research into potential solutions to the task
   - **IMPORTANT:** Include `{current-month-year}` (from Step 0) in all search queries to get current results
   - Example: "TypeScript 5 satisfies operator patterns March 2026" not just "TypeScript satisfies operator"
2. Consider which potential solutions are most appropriate for this code base by exploring the local repo further if necessary
3. Summarize the most promising potential approaches, the pros and cons of each, and an ultimate recommendation.

Record findings under **Research Findings**

### Step 7: Clarification

1. Create a list of any unspecified requirements or clarifications that would be helpful for the user to decide upon

2. **CRITICAL: For EVERY clarifying question, you MUST provide:**
   - **Options table:** 2-4 distinct options (labeled A, B, C, etc.) with brief descriptions
   - **Recommendation:** Your specific recommendation with rationale
   - **Context alignment note:** If any option would conflict with project context, flag it

   **Format each clarification as:**
   ```markdown
   ### N. {Question Title}

   **Question:** {The specific question being asked}

   | Option | Description |
   |--------|-------------|
   | **A) {Option name}** | {Brief description of this approach} |
   | **B) {Option name}** | {Brief description of this approach} |
   | **C) {Option name}** | {Brief description of this approach} |

   **Recommendation:** **Option X** — {Rationale explaining why this is the best choice for this codebase/context}

   **Context note:** {Any alignment or conflict with project context, phase, or decisions — omit if none}
   ```

3. This structured format ensures the user can quickly:
   - Understand the trade-offs at a glance
   - See your informed recommendation
   - Make a decision or request more detail on specific options

### Step 8: Write ideation document

Create `docs/ideation/{slug}.md` with the following structure:

```markdown
# {Task Title}

**Slug:** {slug}
**Author:** Claude Code
**Date:** {current-date}
**Branch:** preflight/{slug}
**Related:** {links-to-issues/PRs/specs}

---

## 0) Context Grounding

**Project:** {project name from context docs}
**Current Phase:** {active phase/milestone}
**Phase Alignment:** {how this task relates to current phase goals}
**Relevant Decisions:** {any prior decisions that affect this work}
**Constraints:** {project-level constraints that apply}

> {1-2 sentence summary of how this task fits into the project's current direction}

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
- **Recommendation** {concise description}

## 6) Clarifications Needed

{For each clarification, use this format:}

### 1. {Question Title}

**Question:** {The specific question being asked}

| Option | Description |
|--------|-------------|
| **A) {Option name}** | {Brief description} |
| **B) {Option name}** | {Brief description} |
| **C) {Option name}** | {Brief description} |

**Recommendation:** **Option X** — {Rationale}

**Context note:** {Alignment or conflict with project context}


```


---

## Example Usage

```bash
/ideate Fix chat UI auto-scroll bug when messages exceed viewport height
```

This will execute the full workflow, creating comprehensive ideation document at `docs/ideation/fix-chat-ui-auto-scroll-bug.md` and guide you through discovery of the task — grounded in the project's context foundation.
