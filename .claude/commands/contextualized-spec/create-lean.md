---
allowed-tools: Read, Write, Grep, Glob, TodoWrite, Task, mcp__context7__resolve-library-id, mcp__context7__get-library-docs, Bash(ls:*), Bash(echo:*), Bash(command:*), Bash(npm:*), Bash(claude:*)
description: Generate a lean, context-grounded spec file that avoids over-engineering
category: validation
argument-hint: "<feature-or-bugfix-description>"
---

## Context
- Existing specs: !`ls -la specs/ 2>/dev/null || echo "No specs directory found"`

## Optional: Enhanced Library Documentation Support

Context7 MCP server provides up-to-date library documentation for better spec creation.

Check if Context7 is available: !`command -v context7-mcp || echo "NOT_INSTALLED"`

If NOT_INSTALLED and the feature involves external libraries, offer to enable Context7:
```
████ Optional: Enable Context7 for Enhanced Documentation ████

Context7 provides up-to-date library documentation to improve spec quality.
This is optional but recommended when working with external libraries.

Would you like me to install Context7 for you? I can:
  1. Install globally: npm install -g @upstash/context7-mcp
  2. Add to Claude Code: claude mcp add context7 context7-mcp

Or you can install it manually later if you prefer.
```

If user agrees to installation:
- Run: `npm install -g @upstash/context7-mcp`
- Then run: `claude mcp add context7 context7-mcp`
- Verify installation and proceed with enhanced documentation support

If user declines or wants to continue without it:
- Proceed with spec creation using existing knowledge

## CONTEXT ECOSYSTEM INTEGRATION

**NEW — Before any analysis, load the project's context foundation.**

### Load Context Triad

1. Check for context documents:
   - `.claude/context/PROJECT_CONTEXT.md` — machine-readable project constitution
   - `.claude/context/SOUL_DOCUMENT.md` — human-readable operating manual
   - `.claude/context/PROJECT_OWNER.md` — agent behavioral protocol

2. If found, extract:
   - **Current phase and acceptance criteria** — what milestone are we in?
   - **Project vision** — who is this for, what does "done" look like?
   - **Constraints and non-goals** — what is out of bounds?
   - **Decision log** — what has been decided?

3. Use this context to **enforce scope discipline** — the context triad is your best tool for knowing what belongs in the main spec vs. Future Improvements. If a "nice to have" idea conflicts with the current phase or stated non-goals, it goes straight to Future Improvements.

4. If no context documents are found:
   - Proceed with standard lean spec creation
   - Note the absence in the spec

## LEAN SPEC PRINCIPLES

This command creates **lean, focused specifications** that avoid over-engineering while maintaining quality:

### What Makes a Spec "Lean"
- **Scope Discipline**: Spec exactly what was requested, no more
- **Context-Informed Scoping**: Use project context to know what belongs now vs. later
- **Essential Testing**: Include tests that validate the feature works, not exhaustive test matrices
- **Immediate Concerns**: Focus on what must be solved now, not future scalability unless explicitly needed
- **Natural Extensions OK**: If something is truly a natural part of the feature, include it
- **Future Ideas Separated**: Good ideas that go beyond scope belong in "Future Improvements"

### Inclusion Guidelines
✅ **DO Include in Main Spec:**
- Direct requirements from the user's request
- Essential error handling for the feature to work
- Natural, expected behavior that users would assume exists
- Critical technical dependencies and integration points
- Minimal viable testing to validate correctness
- Obvious edge cases that would break the feature
- Work that aligns with the current phase's acceptance criteria

❌ **DO NOT Include in Main Spec:**
- "Nice to have" enhancements not requested
- Exhaustive test scenarios beyond basic validation
- Performance optimizations unless explicitly needed
- Future scalability concerns for current usage
- Multiple implementation phases unless truly necessary
- Defensive programming for unlikely edge cases
- Work that belongs to a future phase (per context documents)

🔮 **Move to Future Improvements Section:**
- Enhancements that would be valuable but weren't requested
- Advanced features that build on the core functionality
- Optimizations that aren't immediately needed
- Alternative approaches worth considering later
- Scaling considerations beyond current needs
- Work explicitly scoped for later phases in the context documents

### The "Natural Extension" Test
When unsure if something should be included, ask:
- "Would a user be surprised if this feature didn't have this?"
- "Is this truly part of making the requested feature work?"
- "Would omitting this break user expectations for this feature?"
- **"Does the project context place this in the current phase or a future one?"**

If yes to the first three and it fits the current phase, include it. Otherwise, move to Future Improvements.

## FIRST PRINCIPLES PROBLEM ANALYSIS

Before defining any solution, validate the problem from first principles:

### Core Problem Investigation
- **Strip Away Solution Assumptions**: What is the core problem, completely separate from any proposed solution?
- **Root Cause Analysis**: Why does this problem exist? What created this need?
- **Goal Decomposition**: What are we fundamentally trying to achieve?
- **Value Proposition**: What is the minimum viable solution that delivers core value?
- **Scope Validation**: Are we solving the right problem, or treating symptoms of a deeper issue?

### Problem Validation Questions
- **Real vs. Perceived**: Is this solving a real problem that users actually have?
- **Assumption Audit**: What assumptions might be wrong?
- **Essential vs. Nice-to-Have**: What is absolutely necessary vs. what would be nice?

### Context Alignment Check
- **Phase fit**: Does this feature belong in the current phase?
- **Vision alignment**: Does this serve the project's stated users and goals?
- **Decision consistency**: Does this conflict with any logged decisions?

**CRITICAL: Only proceed if the core problem is clearly defined, validated, and fits the current project phase. If uncertain, request additional context.**

## MANDATORY PRE-CREATION VERIFICATION

After validating the problem from first principles, complete these technical checks:

### 1. Context Discovery Phase
- Search existing codebase for similar features/specs using AgentTool
- **Use specialized subagents** when research involves specific domains (TypeScript, React, testing, databases, etc.)
- Identify potential conflicts or duplicates
- Verify feature request is technically feasible
- Document any missing prerequisites

### 2. Request Validation
- Confirm request is well-defined and actionable
- If vague or incomplete, STOP and ask clarifying questions
- Validate scope is appropriate (not too broad/narrow)

### 3. Quality Gate
- Only proceed if you have 80%+ confidence in implementation approach
- If uncertain, request additional context before continuing
- Document any assumptions being made

**CRITICAL: If any validation fails, STOP immediately and request clarification.**

## Your task

Create a **lean, focused specification document** in the `specs/` folder for the following feature/bugfix: $ARGUMENTS

First, analyze the request to understand:
1. Whether this is a feature or bugfix
2. The **exact scope** of what was requested (nothing more)
3. Related existing code/features
4. External libraries/frameworks involved
5. **Whether this fits the current project phase** (per context documents)

If the feature involves external libraries or frameworks AND Context7 is available:
- Use `mcp__context7__resolve-library-id` to find the library
- Use `mcp__context7__get-library-docs` to get up-to-date documentation
- Reference official patterns and best practices from the docs

## FOCUSED INTEGRATION ANALYSIS

Map the system impact **for the requested feature only**:

### Immediate Integration Points
- **Direct Data Flow**: How does data flow for THIS specific feature?
- **Required Dependencies**: What services/APIs/databases must this touch?
- **Existing Touchpoints**: Where does this feature integrate with existing code?

### User Flow (Lean Version)
- **Primary Path**: The main happy path flow
- **Critical Errors**: Only error cases that would break the feature
- **Entry/Exit**: How users start and finish with this feature

**Keep this analysis focused on what's needed for the feature to work, not every possible scenario.**

## LEAN SPEC STRUCTURE

Create a spec document with these sections:

### Core Sections (Always Include)

1. **Title**: Clear, descriptive title of the feature/bugfix
2. **Status**: Draft/Under Review/Approved/Implemented
3. **Authors**: Your name and date
4. **Overview**: Brief description and purpose (2-3 sentences)
5. **Context Alignment**: 1-3 sentences on how this fits the current phase and project direction. Reference specific context document sections if applicable. Note any tensions.
6. **Problem Statement**: What problem this solves and why it's needed
7. **Goals**: What we aim to achieve (bullet points - only what was requested)
8. **Non-Goals**: What is explicitly out of scope (be generous here — cross-reference project-level non-goals)
9. **Technical Approach**:
   - How we'll implement this (high-level)
   - Key files/modules that will change
   - External libraries/frameworks (if any)
   - Integration points with existing code
10. **Implementation Details**:
   - Specific changes needed
   - Code examples (if helpful)
   - API/data model changes (if any)
11. **Testing Approach**:
    - What tests are needed to validate this works
    - Key scenarios to test
    - **Keep this minimal** - just what's needed to verify correctness
12. **Open Questions**: Any unresolved questions or decisions

### Conditional Sections (Only Include If Relevant)

13. **User Experience**: Only if this has user-facing changes
14. **Migration/Rollout**: Only if this requires data migration or phased rollout
15. **Security/Performance**: Only if there are immediate concerns for this feature
16. **Documentation**: Only if new documentation is needed (not just updating existing)

### Mandatory Final Section

17. **Future Improvements and Enhancements**

**⚠️ IMPORTANT: Everything in this section is OUT OF SCOPE for initial implementation.**

This section is for documenting valuable ideas that go beyond the current request:
- Enhanced features that would build on this
- Performance optimizations not immediately needed
- Additional testing scenarios beyond basic validation
- Alternative approaches worth considering later
- Scalability improvements for future growth
- Work explicitly scoped for later project phases (reference which phase in context docs)

Include items here liberally - it's better to document good ideas for later than to bloat the main spec.

### Final Section

18. **References**: Links to related issues, PRs, documentation, library docs, and **context documents referenced**

## Guidelines for Lean Specs

- **Scope Discipline**: If it wasn't explicitly requested and isn't a natural part of the feature, move it to Future Improvements
- **Context-Informed**: Let the project context help you decide what's in vs. out
- **Testing**: Include tests that verify the feature works, not exhaustive test matrices
- **Code Examples**: Include only if they clarify the approach
- **Implementation Phases**: Only include if the feature truly needs multiple phases (most don't)
- **Error Handling**: Cover critical errors, not every possible failure mode
- **Edge Cases**: Handle obvious ones that would break the feature, not unlikely scenarios
- **Be Specific**: Vague specs lead to over-implementation
- **No Time Estimates**: Do NOT include time or effort estimations

Name the spec file descriptively:
- Features: `feat-{kebab-case-name}.md`
- Bugfixes: `fix-{issue-number}-{brief-description}.md`
- Add `-lean` suffix if helpful: `feat-{name}-lean.md`

## LEAN SPEC SELF-AUDIT

After completing the spec, perform this self-audit:

### Over-Engineering Check
Ask yourself these questions honestly:

1. **Scope Creep Detection**:
   - [ ] Does the spec implement exactly what was requested?
   - [ ] Did I add features/capabilities not explicitly asked for?
   - [ ] Are all additions truly natural extensions or essential dependencies?
   - [ ] **Does everything in the main spec belong in the current project phase?**

2. **Testing Bloat Check**:
   - [ ] Is the testing approach minimal but sufficient?
   - [ ] Did I specify exhaustive test matrices instead of key scenarios?
   - [ ] Are all specified tests necessary to validate correctness?

3. **Future-Proofing Check**:
   - [ ] Am I solving current problems or hypothetical future ones?
   - [ ] Did I add complexity for scalability not yet needed?
   - [ ] Are performance optimizations premature?

4. **Phase Inflation Check**:
   - [ ] Do I really need multiple implementation phases?
   - [ ] Is "Phase 1" actually the whole feature?
   - [ ] Did I artificially split simple work into phases?

5. **Natural Extension Validation**:
   - For each addition beyond the literal request, confirm:
   - [ ] Would users expect this as part of the feature?
   - [ ] Is this essential for the feature to work properly?
   - [ ] Would omitting this surprise or frustrate users?

6. **Future Improvements Separation**:
   - [ ] Did I move "nice to have" ideas to Future Improvements?
   - [ ] Are there good ideas in the main spec that should be deferred?
   - [ ] Is the Future Improvements section well-populated?

7. **Context Alignment Verification**:
   - [ ] Does this spec respect the project's stated non-goals?
   - [ ] Is all work scoped to the current phase?
   - [ ] Are decisions consistent with the project's decision log?

### Remediation Actions

If you answered "yes" to over-engineering questions (2, 4, 5) or "no" to validation questions (1, 3, 6, 7):

**STOP and revise the spec:**
- Move scope creep items to Future Improvements
- Simplify testing to essential scenarios
- Remove premature optimizations
- Collapse unnecessary phases
- Clarify which additions are truly natural vs. nice-to-have
- Move future-phase work to Future Improvements with phase references

### Quality Gate

Only proceed if:
- ✅ Spec addresses exactly what was requested
- ✅ All additions pass the "natural extension" test
- ✅ Testing is minimal but sufficient
- ✅ Future Improvements section is populated
- ✅ No hypothetical future problems being solved
- ✅ Implementation is as simple as possible while being complete
- ✅ Spec is consistent with project context (phase, vision, decisions)

### Final Honesty Check

Ask yourself:
> "If I were implementing this spec, would I feel like it's asking for more than what the original request described?"

If yes, **revise before completing**. Lean specs are about disciplined focus, not cutting corners.

## COMPLETION

Before marking the task complete:

1. ✅ All core sections meaningfully filled
2. ✅ Context Alignment section references project context
3. ✅ Conditional sections included only if relevant
4. ✅ Future Improvements section populated with deferred ideas
5. ✅ Self-audit completed with passing results
6. ✅ Spec is implementable and focused
7. ✅ No over-engineering detected

Present the completed spec location and a brief summary of:
- What the spec covers (main scope)
- How it aligns with the current project phase
- What was moved to Future Improvements (deferred scope)
- Any assumptions or open questions
