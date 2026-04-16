---
description: Implement a validated specification with post-execution context sync
category: validation
allowed-tools: Task, Read, TodoWrite, Grep, Glob, Bash(claudekit:*), Bash(stm:*), Bash(jq:*)
argument-hint: "<path-to-spec-file>"
---

# Implement Specification

Implement the specification at: $ARGUMENTS

!claudekit status stm

## Pre-Execution Checks

1. **Check Task Management**:
   - If STM shows "Available but not initialized" → Run `stm init` first, then `/spec:decompose` to create tasks
   - If STM shows "Available and initialized" → Use STM for tasks
   - If STM shows "Not installed" → Use TodoWrite instead

2. **Verify Specification**:
   - Confirm spec file exists and is complete
   - Check that required tools are available
   - Stop if anything is missing or unclear

3. **Load Context Foundation**:
   - Check for `.claude/context/PROJECT_CONTEXT.md`, `SOUL_DOCUMENT.md`, `PROJECT_OWNER.md`
   - If present, note current phase and acceptance criteria
   - This context informs implementation decisions and the post-execution sync

## Implementation Process

### 1. Analyze Specification

Read the specification to understand:
- What components need to be built
- Dependencies between components
- Testing requirements
- Success criteria

### 2. Load or Create Tasks

**Using STM** (if available):
```bash
stm list --status pending -f json
```

**Using TodoWrite** (fallback):
Create tasks for each component in the specification

### 3. Implementation Workflow

For each task, follow this cycle:

**Available Agents:**
!`claudekit list agents`

#### Step 1: Implement

Launch appropriate specialist agent:

```
Task tool:
- description: "Implement [component name]"  
- subagent_type: [choose specialist that matches the task]
- prompt: |
    First run: stm show [task-id]
    This will give you the full task details and requirements.
    
    Then implement the component based on those requirements.
    Follow project code style and add error handling.
    Report back when complete.
```

#### Step 2: Write Tests

Launch testing expert:

```
Task tool:
- description: "Write tests for [component]"
- subagent_type: testing-expert [or jest/vitest-testing-expert]
- prompt: |
    First run: stm show [task-id]
    
    Write comprehensive tests for the implemented component.
    Cover edge cases and aim for >80% coverage.
    Report back when complete.
```

Then run tests to verify they pass.

#### Step 3: Code Review (Required)

**Important:** Always run code review to verify both quality AND completeness. Task cannot be marked done without passing both.

Launch code review expert:

```
Task tool:
- description: "Review [component]"
- subagent_type: code-review-expert
- prompt: |
    First run: stm show [task-id]
    
    Review implementation for BOTH:
    1. COMPLETENESS - Are all requirements from the task fully implemented?
    2. QUALITY - Code quality, security, error handling, test coverage
    
    Categorize any issues as: CRITICAL, IMPORTANT, or MINOR.
    Report if implementation is COMPLETE or INCOMPLETE.
    Report back with findings.
```

#### Step 4: Fix Issues & Complete Implementation

If code review found the implementation INCOMPLETE or has CRITICAL issues:

1. Launch specialist to complete/fix:
   ```
   Task tool:
   - description: "Complete/fix [component]"
   - subagent_type: [specialist matching the task]
   - prompt: |
       First run: stm show [task-id]
       
       Address these items from code review:
       - Missing requirements: [list any incomplete items]
       - Critical issues: [list any critical issues]
       
       Update tests if needed.
       Report back when complete.
   ```

2. Re-run tests to verify fixes

3. Re-review to confirm both COMPLETE and quality standards met

4. Only when implementation is COMPLETE and all critical issues fixed:
   - If using STM: `stm update [task-id] --status done`
   - If using TodoWrite: Mark task as completed

#### Step 5: Commit Changes

Create atomic commit following project conventions:
```bash
git add [files]
git commit -m "[follow project's commit convention]"
```

### 4. Track Progress

Monitor implementation progress:

**Using STM:**
```bash
stm list --pretty              # View all tasks
stm list --status pending      # Pending tasks
stm list --status in-progress  # Active tasks
stm list --status done         # Completed tasks
```

**Using TodoWrite:**
Track tasks in the session with status indicators.

### 5. Complete Implementation

Implementation is complete when:
- All tasks are COMPLETE (all requirements implemented)
- All tasks pass quality review (no critical issues)
- All tests passing
- Documentation updated

## Step 6: Post-Execution Context Sync

**NEW — After all implementation tasks are complete, perform a context ecosystem review.**

This is the step that closes the loop — ensuring that what was just built is reflected in the project's living documents.

### 6a. Internal Documentation Update

Review and update internal documentation based on what was built:

1. **CLAUDE.md** — Did we establish new patterns, discover gotchas, or add key files that should be noted?
2. **Developer guides** — Does this feature warrant a new developer guide in `developer-guides/`? If it's a significant feature, create one.
3. **README.md** — Does this change anything a user would need to know?

### 6b. Core Context Document Review

**Only if context documents are present.** Read each document and assess what needs updating:

1. **PROJECT_CONTEXT.md** — Machine-readable project constitution:
   - Does the "current build state" section need updating?
   - Did we complete a milestone or acceptance criterion?
   - Were any new technical decisions made during implementation?
   - Did we discover new constraints?

2. **SOUL_DOCUMENT.md** — Human-readable operating manual:
   - Do phase acceptance criteria need checkbox updates?
   - Should the decision log be appended with decisions made during this build?
   - Did any key workflows change?
   - Is the "what's been built" narrative still accurate?

3. **PROJECT_OWNER.md** — Agent behavioral protocol:
   - Does the agent need awareness of new areas that were built?
   - Should new routing rules be added for the new feature?
   - Are there new patterns the agent should enforce?

### 6c. Present Changes for Approval

**Do not auto-commit context document changes.** Instead:

1. Present a summary of proposed changes:
   ```markdown
   ## Context Document Updates
   
   ### PROJECT_CONTEXT.md
   - [ ] Updated current build state: [description]
   - [ ] Added decision: [description]
   
   ### SOUL_DOCUMENT.md
   - [ ] Checked off acceptance criteria: [list]
   - [ ] Appended to decision log: [entry]
   
   ### PROJECT_OWNER.md
   - [ ] Added routing rule for [new area]
   
   ### Internal Docs
   - [ ] Updated CLAUDE.md: [what changed]
   - [ ] Created developer guide: [name] (or: No new guide needed because [reason])
   ```

2. Ask the user to approve, modify, or skip each update

3. Apply approved changes

4. Commit context document updates separately from implementation commits:
   ```bash
   git add [context files]
   git commit -m "docs: update context documents after [feature name] implementation"
   ```

## If Issues Arise

If any agent encounters problems:
1. Identify the specific issue
2. Launch appropriate specialist to resolve
3. Or request user assistance if blocked
