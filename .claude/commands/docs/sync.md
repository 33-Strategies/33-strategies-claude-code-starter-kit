---
description: Update all documentation with forward-looking knowledge from recent work - patterns, gotchas, dependencies, and integration points that future developers need to know
allowed-tools: Read, Write, Edit, Grep, Glob, Bash(git:*), Task
model: sonnet
---

# Documentation Sync: Forward-Looking Knowledge Transfer

Analyze recent project work and update all documentation to capture **actionable knowledge** that future developers and AI agents need. This is NOT about documenting what was done (change logs), but about documenting what future contributors need to KNOW to work effectively.

## Phase 1: Discovery

### 1.1 Identify All Documentation
Find all documentation files that may need updating:

```bash
!git ls-files '*.md' | head -20
```

Key files to always check:
- CLAUDE.md (AI agent instructions)
- README.md (project overview)
- Any files in `docs/` or `developer-guides/`
- Any `*-guide.md` or `*-notes.md` files

### 1.2 Analyze Recent Changes
Examine what has been built/modified recently:

```bash
!git log --oneline --since="2 weeks ago" | head -20
```

```bash
!git diff --stat HEAD~10..HEAD 2>/dev/null || git diff --stat $(git rev-list --max-parents=0 HEAD)..HEAD | head -30
```

Get detailed view of key changed files:
```bash
!git diff --name-status HEAD~10..HEAD 2>/dev/null | head -30
```

## Phase 2: Knowledge Extraction

For each significant area of recent work, extract:

### 2.1 Patterns & Conventions
- What coding patterns were established or reinforced?
- What naming conventions are being used?
- What architectural decisions were made?
- What folder/file organization patterns exist?

### 2.2 Gotchas & Pitfalls
- What errors were encountered and how were they solved?
- What edge cases were discovered?
- What doesn't work the way you'd expect?
- What common mistakes should be avoided?

### 2.3 Dependencies & Integration Points
- What new dependencies were added and why?
- How do new components integrate with existing ones?
- What APIs or interfaces were created?
- What data flows were established?

### 2.4 Critical Context
- What business logic or domain knowledge is embedded in the code?
- What assumptions are baked into the implementation?
- What external services or data sources are being used?
- What configuration is required?

## Phase 3: Documentation Updates

### Update Strategy

For each documentation file, determine what forward-looking knowledge needs to be added:

**CLAUDE.md Updates** - Focus on:
- New agent protocols or patterns
- Updated SOPs based on lessons learned
- New tools, scripts, or utilities that agents should know about
- Gotchas that will affect future AI agent work

**README.md Updates** - Focus on:
- New features that users need to understand
- Updated setup or configuration requirements
- Changed project structure or architecture

**Developer Guides** - Focus on:
- How to extend or modify new features
- Integration patterns for new components
- Testing strategies for new functionality

### Update Format

When adding information, structure it as actionable knowledge:

```markdown
## [Feature/Component Name]

**What it does:** Brief functional description

**Key files:**
- `path/to/main-file.ts` - Primary logic
- `path/to/types.ts` - Type definitions

**Integration points:**
- Connects to X via Y
- Requires Z to be configured

**Gotchas:**
- Don't do A because B
- Remember to C when D

**Extending this:**
- To add new X, follow pattern in Y
- Configuration lives in Z
```

## Phase 4: Execute Updates

1. **Read each documentation file** that needs updating
2. **Identify gaps** - what forward-looking knowledge is missing?
3. **Draft additions** - write concise, actionable content
4. **Apply updates** - use Edit tool to add new sections or update existing ones
5. **Verify coherence** - ensure updates integrate well with existing content

## Quality Checklist

Before finalizing each update, verify:

- [ ] **Actionable**: Can someone act on this information?
- [ ] **Forward-looking**: Does this help with future work, not just document past work?
- [ ] **Concise**: Is it as brief as possible while remaining useful?
- [ ] **Contextual**: Does it explain WHY, not just WHAT?
- [ ] **Discoverable**: Is it in the right place for someone to find it?
- [ ] **Non-redundant**: Does it avoid duplicating existing documentation?

## Output

After completing updates, provide a summary:

1. **Files Updated**: List each doc file modified
2. **Knowledge Added**: Brief description of key knowledge captured
3. **Recommended Follow-ups**: Any areas that need deeper documentation

Remember: The goal is to make the next person (human or AI) who works in this codebase more effective by giving them the knowledge they need BEFORE they need it.
