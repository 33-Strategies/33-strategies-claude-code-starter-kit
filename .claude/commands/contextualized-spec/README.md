# Contextualized Spec Commands

Updated versions of the core spec workflow commands, redesigned to integrate with the **Context Ecosystem** — the three-document foundation that gives every AI agent, team member, and workflow deep shared context about what a project is, where it stands, and what matters.

---

## What's Different

The original spec commands (`/spec:ideate`, `/spec:create`, etc.) are excellent at structured ideation, specification, and implementation. But they operate in isolation from the project's identity. They don't know what phase you're in, what's been decided, what the vision is, or what the project's constraints are.

These contextualized versions fix that. Every command in the workflow now:

1. **Loads the context triad** before doing anything else
2. **Grounds decisions in project context** — scoping, architectural choices, and prioritization are informed by the project's stated goals, current phase, and decision history
3. **Flags misalignment** — if a proposed feature conflicts with a logged decision, a stated non-goal, or doesn't fit the current phase, you'll know before writing a line of code
4. **Closes the loop** — after implementation, a new context sync step reviews the core documents and proposes updates so they stay current

The result: you can run the spec workflow and trust that what gets built is aligned with what the project is trying to be, and that the project's living documents stay alive.

---

## The Context Triad

These commands expect three documents in `.claude/context/`:

| Document | Audience | Purpose |
|----------|----------|---------|
| `PROJECT_CONTEXT.md` | AI agents | Machine-readable constitution. 17 sections covering who this is for, what done looks like, what's decided, what the constraints are, and current build state. First file any LLM reads. |
| `SOUL_DOCUMENT.md` | Team | Human-readable operating manual. User portrait, vision narrative, phase acceptance criteria with checkboxes, decision log, key workflows. Read before every call or build session. |
| `PROJECT_OWNER.md` | Claude Code | Agent behavioral protocol. Loaded as a sub-agent. Routes build decisions through project vision. Flags when something's off before code gets written. |

If these documents don't exist, all commands gracefully degrade to the original behavior — they'll note the absence and proceed without context grounding.

---

## The Workflow

```
ideate → best-practices-audit → ideate-to-spec → create / create-lean → validate → decompose → execute
                                                                                                   ↓
                                                                                           context-sync
                                                                                                   ↓
                                                                                           evaluate (optional)
```

### Command-by-Command Changes

#### `ideate.md` — Structured Ideation

**What changed:** Added **Step 2: Load Context Foundation** between setup and scoping.

Before researching or scoping anything, the command now reads the context triad and writes a "Context Grounding" block into the ideation document. This block captures:
- Current project phase and milestone
- How the proposed task aligns with phase goals
- Relevant prior decisions that constrain the work
- Any conflicts with stated non-goals

The rest of the ideation proceeds as before, but scope decisions and clarification questions now reference project context. Clarifications include a "Context note" field for flagging alignment issues.

**New ideation doc section:** Section 0 — Context Grounding (before Intent & Assumptions)

---

#### `create.md` — Full Specification

**What changed:** Added **Context Ecosystem Integration** section and a new **Context Alignment Check** within the first-principles analysis.

The spec creation process now:
- Loads the context triad at the start and holds it in working memory
- Includes a "Context Alignment Check" that verifies vision fit, phase fit, decision consistency, and non-goal compliance before proceeding
- Adds section 5 to the spec output: **Context Alignment** — explicitly maps the feature to project goals
- Section 18 (References) now includes which context document sections informed the spec
- Final validation includes a context alignment check

---

#### `create-lean.md` — Lean Specification

**What changed:** Same context integration as `create.md`, plus context-informed scope discipline.

The lean spec's superpower is knowing what to cut. The context triad makes that easier:
- Work scoped for future phases (per context docs) goes straight to Future Improvements
- Non-goals from the project level reinforce the spec-level non-goals
- The "Natural Extension Test" now includes: "Does the project context place this in the current phase or a future one?"
- Self-audit adds a "Context Alignment Verification" check

---

#### `validate.md` — Specification Validation

**What changed:** Added a fourth validation dimension: **ALIGNMENT — Context Ecosystem Consistency**.

The original validates WHY (intent), WHAT (scope), and HOW (implementation). Now it also validates:
- **Vision alignment**: Does the spec serve the project's stated vision?
- **Phase fit**: Does the work belong in the current phase?
- **Decision consistency**: Any conflicts with the decision log?
- **Non-goal violation**: Any overlap with stated non-goals?
- **Constraint respect**: Does the spec honor project constraints?

Output now includes a Context Alignment assessment (Aligned / Misaligned / Partially Aligned / Not Evaluated) and specific alignment issues with examples.

---

#### `decompose.md` — Task Decomposition

**What changed:** Context-aware task creation and a mandatory **context sync task**.

- Before decomposing, loads context docs to note current phase and acceptance criteria
- Tasks that advance a phase acceptance criterion get tagged
- A **final task** is always added: "Context Document Review" — review and update PROJECT_CONTEXT, SOUL_DOCUMENT, and PROJECT_OWNER based on what was built
- Success criteria now includes "Context sync task included"

---

#### `execute.md` — Implementation

**What changed:** Added **Step 6: Post-Execution Context Sync** — the biggest structural addition.

After all implementation tasks pass review and tests, the command now:

**6a. Updates internal documentation:**
- CLAUDE.md (new gotchas, patterns, key files)
- Developer guides (create or update)
- README.md (user-facing changes)

**6b. Reviews core context documents:**
- PROJECT_CONTEXT.md — build state, milestones, technical decisions
- SOUL_DOCUMENT.md — acceptance criteria checkboxes, decision log entries
- PROJECT_OWNER.md — new areas, routing rules, patterns

**6c. Presents changes for approval** — all proposed context updates are shown as a checklist. The user approves, modifies, or skips each one before anything is written. Context document changes get their own commit, separate from implementation commits.

---

#### `ideate-to-spec.md` — Ideation to Specification Bridge

**What changed:** Added **Step 0: Load Context Foundation** and context threading throughout.

- Context is loaded before reading the ideation document
- Ideation content is cross-referenced with project context (phase fit, decision conflicts, constraints)
- Decision gathering (Step 2) includes context alignment notes for each option
- Spec plan (Step 3) considers phase boundaries — work that belongs in a future phase is flagged for deferral
- The spec creation prompt (Step 4) includes project-level constraints
- Validation (Step 6) captures context alignment assessment
- Summary (Step 7) includes a "Context Alignment" section with phase, criteria, and decisions referenced

---

#### `best-practices-audit.md` — Best Practices Audit

**What changed:** Added **Project Context Alignment** as a fourth audit dimension.

In addition to auditing UI/UX, prompt engineering, and technical implementation against industry best practices, the audit now checks:
- Vision fit with project context
- Phase fit with current milestone
- Design consistency with project design principles
- Architectural consistency with logged decisions
- User alignment with stated target users

When project context conflicts with generic best practices, the audit notes the tension and lets the user decide — project context often reflects intentional trade-offs.

---

### New Commands

#### `context-sync.md` — Standalone Context Sync

The post-execution context review, extracted as its own command for ad-hoc use. Run this:
- After completing a feature outside the spec workflow
- At the end of a work session
- When you suspect documents have drifted from reality

Steps: gather current state (git + context docs) → review internal docs → review core context docs → present proposed changes → apply approved changes.

**Requires:** Context triad documents present.

---

#### `evaluate.md` — Project State Evaluation & PM Sync

The full "project owner agent" command. Cross-references context documents, git history, and Linear to produce a project health assessment and propose PM updates.

**⚠️ Requires double user approval** — once before analysis (confirming prerequisites are met) and once before any writes (approving each proposed update).

**Additional prerequisites beyond the context triad:**
- Linear API integration (API key + client library)
- Git history access

This command identifies:
- Completed work not reflected in Linear
- Linear issues that don't match code reality
- Phase progress against acceptance criteria
- Stale issues and undocumented work
- Risks visible from the data

Then proposes targeted updates to Linear issues and context documents — but only executes what the user explicitly approves.

---

## Migration from Original Commands

To use these commands in a project:

1. **Create the context triad** in `.claude/context/` (templates should be provided separately)
2. **Copy these command files** to `.claude/commands/spec/` (replacing the originals) or to a separate namespace
3. **No other changes needed** — the commands are backward-compatible. If no context documents are found, they behave like the originals.

For the `evaluate` command specifically, you'll also need:
- `LINEAR_API_KEY` or `LINEAR_API_KEY_33STRATEGIES` environment variable
- A Linear client library in the project (e.g., `lib/linear/`)
