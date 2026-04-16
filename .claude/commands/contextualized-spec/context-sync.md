---
description: Review and update project context documents to reflect current reality
allowed-tools: Read, Write, Edit, Grep, Glob, Bash(git:*)
argument-hint: "[optional: description of what was recently built]"
category: workflow
---

# Context Ecosystem Sync

**Recent work context:** $ARGUMENTS

---

## Purpose

This command reviews the project's core context documents and internal documentation to ensure they reflect the current state of the project. Run this after completing a feature, at the end of a work session, or whenever you suspect the context documents have drifted from reality.

This is the standalone version of the context sync step that's also embedded in `/spec:execute`. Use this when you want to run a sync independently.

## Prerequisites

This command requires the **context triad** to be present:
- `.claude/context/PROJECT_CONTEXT.md`
- `.claude/context/SOUL_DOCUMENT.md`
- `.claude/context/PROJECT_OWNER.md`

If any are missing, inform the user and suggest creating them before running this command.

## Workflow

### Step 1: Gather Current State

1. **Read all three context documents** to understand what they currently say
2. **Review recent git activity:**
   ```bash
   git log --oneline -20
   ```
3. **Review recent changes:**
   ```bash
   git diff --stat HEAD~10..HEAD 2>/dev/null || git diff --stat HEAD~5..HEAD
   ```
4. If the user provided a description of recent work (`$ARGUMENTS`), use that as primary context

### Step 2: Internal Documentation Review

Check and update internal docs:

1. **CLAUDE.md** — Does it need new gotchas, patterns, or key file references based on recent work?
2. **Developer guides** (`developer-guides/`) — Does a new guide need to be created? Do existing guides need updates?
3. **README.md** — Any user-facing changes?

For each file that needs updating:
- Read the current content
- Identify specific additions or changes needed
- Draft the updates

### Step 3: Core Context Document Review

For each context document, assess what needs updating:

#### PROJECT_CONTEXT.md — Machine-readable constitution

Check each section against current reality:
- **Current build state**: Is the description of what's built accurate?
- **Phase/milestone**: Have we completed or advanced any milestones?
- **Technical stack**: Did we add or remove any technologies?
- **Constraints**: Did we discover new constraints during implementation?
- **Integration points**: Did we add new integrations?

#### SOUL_DOCUMENT.md — Human-readable operating manual

Check each section:
- **Phase acceptance criteria**: Can any checkboxes be checked off?
- **Decision log**: Were any decisions made during implementation that should be recorded? (Include date, what was decided, and why)
- **Key workflows**: Did any workflows change or get added?
- **User portrait/vision**: Still accurate, or has our understanding evolved?

#### PROJECT_OWNER.md — Agent behavioral protocol

Check:
- **New areas**: Does the agent need to know about new features/areas that were built?
- **Routing rules**: Should the agent gate decisions in new areas?
- **Patterns**: Are there new patterns the agent should enforce or be aware of?
- **Anti-patterns**: Did we discover anything the agent should warn against?

### Step 4: Present Proposed Changes

**Do not auto-commit.** Present all proposed changes in a structured format:

```markdown
## Proposed Context Updates

### Internal Documentation

#### CLAUDE.md
- [x] Add gotcha: {description}
- [x] Update key files: {what changed}

#### Developer Guides
- [x] Created: {new-guide-name.md} (or: No new guide needed — {reason})
- [x] Updated: {existing-guide.md} — {what changed}

### Core Context Documents

#### PROJECT_CONTEXT.md
- [ ] Section "{section}": {what to update}
- [ ] Section "{section}": {what to update}

#### SOUL_DOCUMENT.md
- [ ] Acceptance criteria: Check off "{criterion}"
- [ ] Decision log: Append "{decision}" (dated {date})

#### PROJECT_OWNER.md
- [ ] Add awareness of {new area}
- [ ] New routing rule: {description}

### No Changes Needed
- {List any documents that are already up to date}
```

### Step 5: Apply Approved Changes

1. Ask the user to approve, modify, or skip each proposed change
2. Apply approved changes using Edit tool
3. Commit context updates:
   ```bash
   git add [changed context and doc files]
   git commit -m "docs: sync context documents with current project state"
   ```

## Quality Checklist

Before finalizing:
- [ ] All three context documents reviewed
- [ ] CLAUDE.md checked for new gotchas/patterns
- [ ] Developer guides checked (created or updated if needed)
- [ ] Changes are specific and actionable (not vague)
- [ ] Decision log entries include dates and rationale
- [ ] No context document section was skipped
- [ ] User approved all changes before writing

## Output

Provide a summary of:
1. **Documents updated**: List each file modified
2. **Key changes**: Most important updates made
3. **Still accurate**: Documents confirmed as current
4. **Recommended follow-ups**: Any areas that need deeper attention
