---
description: Structured discovery, planning, and execution workflow with task dossier
allowed-tools: Read, Grep, Glob, Edit, Write, Bash(git:*), Bash(npm:*), Bash(npx:*), Task(playwright-expert)
argument-hint: "<task-brief>"
category: workflow
---

# Preflight ▸ Discovery ▸ Plan

**Task Brief:** $ARGUMENTS

---

## Workflow Instructions

Execute this structured engineering workflow that enforces "homework before hacking" for any code-change task (bug fix or feature). Follow each step sequentially.

### Step 1: Create Task Slug & Setup

1. Create a URL-safe slug from the task brief (e.g., "fix-chat-scroll-bug")
2. Create output directory: `mkdir -p docs/task-dossiers`
3. Create feature branch: `git checkout -b preflight/{slug}`

### Step 2: Echo & Scope

Write an "Intent & Assumptions" block that:
- Restates the task brief in 1-3 sentences
- Lists explicit assumptions
- Lists what's explicitly out-of-scope to avoid scope creep

This becomes the opening of the dossier.

### Step 3: Pre-Reading & Codebase Mapping

1. Scan repository for:
   - Developer guides in `developer-guides/`
   - Architecture docs
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

### Step 4: Investigation & Root Cause Analysis

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

Record under **Investigation & Findings**.

### Step 5: Plan of Record (POR)

Draft a minimal, safe plan including:
- **Change list:** Specific files to modify + reason for each
- **Design/API deltas:** Props, events, contracts that will change
- **Data/telemetry:** Events to add or modify
- **Blast radius mitigation:** Strategy to minimize impact
- **Rollback plan:** How to revert if needed

Define **Acceptance Criteria** tied to:
- User-visible outcomes (checkbox format)
- Non-regression checks
- Specific test scenarios

### Step 6: Guardrails Checklist

Before making any code edits, enumerate and execute guardrails:

- [ ] Typecheck passes (`npx tsc --noEmit`)
- [ ] Lint clean (`npm run lint`)
- [ ] Search for all consumers of components to be modified
- [ ] Confirm shared component reuse patterns
- [ ] Identify existing unit/integration tests
- [ ] Plan telemetry/analytics verification
- [ ] Identify docs that need updates

Run these checks and record results.

### Step 7: Write Dossier

Create `docs/task-dossiers/{slug}.md` with the following structure:

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

## 4) Investigation & Findings
- **Repro steps:** {numbered list}
- **Observed vs Expected:** {concise description}
- **Evidence:** {code refs, logs, CSS/DOM snapshots}
- **Root-cause hypotheses:** {bulleted with confidence}
- **Decision:** {selected hypothesis + rationale}

## 5) Plan of Record (POR)
- **Change list:** {files/components + reason}
- **Design/API deltas:** {props, events, contracts}
- **Data/telemetry:** {events added/changed}
- **Blast radius mitigation:** {strategy}
- **Rollback:** {how to revert}

## 6) Acceptance Criteria
- [ ] User can {X} without {Y}
- [ ] Layout/behavior meets {rule}
- [ ] No regressions in {A, B, C}
- [ ] E2E passes: {scenario names}

## 7) Guardrails Checklist
- [ ] Typecheck / lint clean
- [ ] Unit/integration tests added/updated
- [ ] Consumer search completed
- [ ] Shared component reuse confirmed
- [ ] Telemetry verified in dev
- [ ] Docs/guides updated

## 8) Validation Results
{To be filled after implementation}

## 9) Change Log
{To be filled during implementation}
| Commit | File(s) | Summary |
|--------|---------|---------|

## 10) Release Notes & Follow-ups
{To be filled after validation}
```

Commit the dossier: `git add docs/task-dossiers/{slug}.md && git commit -m "docs: add task dossier for {slug}"`

### Step 8: Implementation

Execute the POR in small, reviewable commits:
1. Make surgical changes following the plan
2. After each significant change, update the **Change Log** table with:
   - Commit hash
   - File(s) modified
   - Brief summary

3. If the plan changes during implementation, update the POR section

### Step 9: Validation

Convert Acceptance Criteria into concrete checks:
1. Add/update unit tests for changed logic
2. Add/update integration tests for workflows
3. Create at least one E2E test scenario using Playwright
4. If browser automation needed, use the Task tool with `playwright-expert` agent

Record results under **Validation Results** with:
- Unit/integration test summary
- E2E/Playwright scenarios + outcomes
- Screenshots or recordings

### Step 10: PR Preparation

1. Update **Release Notes & Follow-ups** section with:
   - Operator notes for release & support
   - Follow-up tasks or tech debt items

2. Open PR with this checklist in description:

```markdown
## Task Dossier

Link: `docs/task-dossiers/{slug}.md`

## PR Checklist

- [ ] Dossier complete (context → plan → validation)
- [ ] Tests cover acceptance criteria
- [ ] Telemetry confirmed in dev (if applicable)
- [ ] Feature flag / config documented (if applicable)
- [ ] Rollback plan documented
- [ ] Codeowners notified
```

---

## Implementation Guardrails

**Enforce these principles throughout:**

1. **Small, reversible steps** - Feature-flag risky paths; prefer additive over destructive changes
2. **Holistic search** - Grep for component & class names to find all consumers before edits
3. **Contracts first** - Document prop/event/API changes; add type-level tests if possible
4. **Telemetry & tests** - Add/verify events; convert acceptance criteria into tests
5. **Rollback ready** - Keep a clear revert path; include a ready-to-revert commit

---

## Example Usage

```bash
/preflight-discovery Fix chat UI auto-scroll bug when messages exceed viewport height
```

This will execute the full workflow, creating a comprehensive task dossier at `docs/task-dossiers/fix-chat-ui-auto-scroll-bug.md` and guide you through discovery → planning → implementation → validation → PR.
