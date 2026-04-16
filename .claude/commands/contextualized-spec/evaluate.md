---
description: Evaluate project state and propose Linear PM updates based on context ecosystem
allowed-tools: Read, Write, Edit, Grep, Glob, Bash(git:*), Bash(npm:*), Task
argument-hint: "<project-name-or-slug>"
category: workflow
---

> **⚠️ DOUBLE APPROVAL REQUIRED**
>
> This command requires explicit user confirmation at TWO points before taking any action:
> 1. **Before analysis**: Confirm that prerequisites are met (Linear integration, context triad, git access)
> 2. **Before any writes**: Confirm each proposed Linear update and context document change
>
> This command is more demanding than the other spec commands. It requires:
> - A working **Linear API integration** (`LINEAR_API_KEY` or `LINEAR_API_KEY_33STRATEGIES` env var)
> - The **context triad** (PROJECT_CONTEXT.md, SOUL_DOCUMENT.md, PROJECT_OWNER.md)
> - **Git history access** for the project being evaluated
>
> Not every project will have all of these set up. If any prerequisite is missing,
> inform the user clearly and stop.

# Project State Evaluation & PM Sync

**Project:** $ARGUMENTS

---

## Purpose

This command enables a "project owner agent" workflow: evaluate the current state of a project by cross-referencing its context documents, codebase, git history, and Linear project — then propose targeted project management updates.

This is the command that closes the loop in the Context Ecosystem — the AI coding vector feeding Linear directly, context-aware and targeted.

## Approval Gate 1: Prerequisites Check

Before doing any analysis, verify ALL prerequisites:

### 1. Context Triad
```
Check for:
- .claude/context/PROJECT_CONTEXT.md
- .claude/context/SOUL_DOCUMENT.md  
- .claude/context/PROJECT_OWNER.md
```

### 2. Linear Integration
```
Check for LINEAR_API_KEY or LINEAR_API_KEY_33STRATEGIES in environment.
Check for lib/linear/ module (or equivalent Linear client).
```

### 3. Git Access
```
Verify git repository with history.
```

**If any prerequisite is missing:**
```
⚠️ Cannot run project evaluation.

Missing:
- [ ] {list missing prerequisites}

To set up:
- Context triad: Create .claude/context/ with PROJECT_CONTEXT.md, SOUL_DOCUMENT.md, PROJECT_OWNER.md
- Linear integration: Set LINEAR_API_KEY environment variable
- Git access: Ensure this is a git repository with commit history

Stopping here. Please set up the missing prerequisites and try again.
```

**If all prerequisites are met, ask the user:**
```
All prerequisites are met. This command will:
1. Read your project context documents
2. Review recent git history
3. Query Linear for current project state
4. Propose PM updates (you'll approve each one)

Proceed? [y/n]
```

**Only continue if the user explicitly confirms.**

## Phase 1: State Gathering

### 1a. Load Context Foundation

Read all three context documents and extract:
- **Project identity and vision**
- **Current phase and milestone**
- **Acceptance criteria** (with current completion state)
- **Decision log**
- **What "done" looks like**

### 1b. Analyze Git History

```bash
# Recent commits
git log --oneline --since="2 weeks ago" | head -30

# Files changed recently
git diff --stat HEAD~20..HEAD 2>/dev/null | head -30

# Contributors
git log --format="%an" --since="2 weeks ago" | sort -u
```

Map recent commits to:
- Features completed
- Bugs fixed
- Areas of active development
- Patterns in commit messages (ticket references, etc.)

### 1c. Query Linear State

Using the project's Linear integration:
- Get all issues for the project
- Get current milestone/cycle status
- Get issue statuses (todo, in-progress, done, cancelled)
- Get any blockers or high-priority items

### 1d. Cross-Reference

Compare the three data sources to identify:

1. **Completed work not reflected in Linear**: Git shows feature X was built, but the Linear issue is still "In Progress"
2. **Linear issues not matching code reality**: Issue marked "Done" but code shows it's partially implemented
3. **Phase progress**: How many acceptance criteria from the context docs have been met?
4. **Stale issues**: Linear issues that haven't been touched and don't appear in recent git activity
5. **Undocumented work**: Significant commits that don't map to any Linear issue

## Phase 2: Health Assessment

Produce a structured project health report:

```markdown
## Project Health Assessment: {project-name}

**Date:** {current date}
**Current Phase:** {from context docs}
**Overall Status:** {On Track / Needs Attention / At Risk}

### Phase Progress
- Acceptance criteria completed: {X}/{Y}
- Recently completed:
  - ✅ {criterion} — completed via {commit/PR reference}
- Still outstanding:
  - ⬜ {criterion} — {status assessment}

### Linear Sync Status
- Issues in sync: {count}
- Issues needing update: {count}
  - {issue-id}: {title} — currently "{status}", should be "{new-status}" because {reason}
  - ...

### Undocumented Work
- {commit range}: {description of work that has no Linear issue}

### Stale Issues
- {issue-id}: {title} — no activity in {N} days, last referenced in {context}

### Risks & Blockers
- {any risks visible from the data}

### Recommended Actions
1. {action 1}
2. {action 2}
...
```

## Approval Gate 2: Proposed Updates

Present ALL proposed actions and require explicit approval for each:

```markdown
## Proposed PM Updates

Review each proposed action. Reply with the numbers you approve (e.g., "1, 3, 5")
or "all" to approve everything, or "none" to skip.

### Linear Issue Updates
1. [ ] {issue-id}: Change status from "{current}" to "{proposed}" — {reason}
2. [ ] {issue-id}: Add comment: "{comment}" — {reason}
3. [ ] {issue-id}: Create new issue: "{title}" — {reason}

### Context Document Updates
4. [ ] PROJECT_CONTEXT.md: Update build state to reflect {change}
5. [ ] SOUL_DOCUMENT.md: Check off acceptance criterion "{criterion}"
6. [ ] SOUL_DOCUMENT.md: Append to decision log: "{entry}"

### Internal Documentation
7. [ ] CLAUDE.md: Add gotcha about {topic}

Which updates should I apply? 
```

**Only execute updates the user explicitly approves.**

## Phase 3: Execute Approved Updates

For each approved action:

1. **Linear updates**: Use the project's Linear API client to make changes
2. **Context document updates**: Use Edit tool
3. **Documentation updates**: Use Edit tool

Commit documentation changes:
```bash
git add [changed files]
git commit -m "docs: project evaluation sync — {summary of changes}"
```

## Output

Final summary:

```markdown
## Evaluation Complete

**Updates applied:**
- Linear: {count} issue updates
- Context docs: {count} changes
- Internal docs: {count} changes

**Skipped (not approved):**
- {list any skipped items}

**Next evaluation recommended:** {timeframe based on project velocity}
```

---

## Notes

- This command is **read-heavy, write-cautious** — it gathers a lot of data but only writes with explicit approval
- Linear API calls are read-only until Approval Gate 2 is passed
- Context document changes are presented as diffs before writing
- This can be run across projects if the user navigates to different project directories
- For projects without Linear integration, the command can still assess context document health and git activity — just skip the Linear sections
