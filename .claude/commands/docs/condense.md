---
description: Condense CLAUDE.md while preserving essential patterns and gotchas
allowed-tools: Read, Edit, Write, Grep, Glob
argument-hint: "[path-to-claude-md]"
---

# Condense CLAUDE.md

Thoughtfully reduce a CLAUDE.md file while preserving information critical for future development success.

**Target file:** $ARGUMENTS (defaults to `./CLAUDE.md` if not specified)

## Philosophy

CLAUDE.md files grow organically as projects evolve. Over time they accumulate:
- Verbose explanations of stable, well-understood features
- Exhaustive API route listings (discoverable via code)
- Detailed typography/color tables (belong in design specs)
- Duplicated information (same gotcha explained multiple places)
- Step-by-step tutorials (belong in dedicated guides)

The goal is NOT to minimize length arbitrarily, but to maximize **signal density** — the ratio of actionable, mistake-preventing information to total content.

## What to PRESERVE (High Signal)

### 1. Critical Gotchas (HIGHEST PRIORITY)
Patterns that have caused bugs or will cause bugs if forgotten:
- "Never do X because Y"
- "Always use A instead of B"
- Edge runtime limitations
- Database connection quirks
- Auth system conflicts
- Race condition warnings

### 2. Architecture Decisions
One-liner explanations of WHY something is structured a certain way:
- "Three auth systems because: portals (iron-session), team (NextAuth), external (OAuth)"
- "Atomic commits pattern prevents orphaned sessions"

### 3. Key File Locations
Essential "where is X" references, but condensed:
- Main entry points for each feature area
- Config files that must be edited together

### 4. Environment Requirements
Critical env vars without verbose explanations

### 5. Commands That Must Be Run
Build/deploy commands, especially non-obvious ones

## What to REMOVE or CONDENSE

### 1. Exhaustive API Route Listings → Reference
Replace 50-line API route lists with: "See `app/api/` — routes follow REST conventions"

### 2. Full Typography Tables → Reference
Replace detailed font tables with: "See `.claude/skills/33-strategies-frontend-design.md`"

### 3. Verbose Feature Descriptions → One-liners
"AI-powered platform for building customer clarity through structured modules with voice/text brain dumps, two-pass extraction, source citations, and deep-linking" → "AI-powered customer clarity platform with brain dump extraction"

### 4. Step-by-Step Tutorials → Doc References
"See `docs/developer-guides/X.md` for implementation details"

### 5. Code Examples That Don't Show Gotchas
Remove generic patterns; keep only examples that demonstrate non-obvious behavior

### 6. Database Model Lists → Schema Reference
Replace model lists with: "See `prisma/schema.prisma`"

### 7. Duplicated Information
If the same gotcha appears in multiple sections, consolidate into one "Critical Gotchas" section

## Condensation Process

1. **Read the full file** to understand structure and identify high-signal content
2. **Identify sections** that can be:
   - Removed entirely (redundant with code/docs)
   - Condensed to a reference pointer
   - Kept but tightened
3. **Create a "Critical Gotchas" section** near the top consolidating all mistake-preventing patterns
4. **Preserve directory structure** if useful, but simplify
5. **Keep Quick Reference table** at top (high utility)
6. **Target 40-60% reduction** while preserving ALL gotchas

## Output Format

After condensing, report:
- Original line count
- New line count
- Percentage reduction
- List of preserved gotchas (to verify none were lost)
- Any sections you recommend the user review

## Example Condensation

**Before (verbose):**
```markdown
### Shareable Artifact Links

Clients can generate password-protected public URLs for specific artifacts...

**Key Files:**
- `lib/share/utils.ts` — Slug generation, password hashing (bcrypt), verification
- `app/api/share/create/route.ts` — Create link (requires portal auth)
- `app/api/share/[slug]/auth/route.ts` — Password verification with brute-force protection
[...15 more lines...]

**Security:**
- bcrypt password hashing (work factor 10, ~100ms)
- Per-link lockout: 5 attempts → 15-minute lockout
[...8 more lines...]

**Critical Gotchas:**
- OG image requires `runtime = 'nodejs'` (Prisma not compatible with edge runtime)
- Use full slug in cookie name (not truncated) to prevent collisions
```

**After (condensed):**
```markdown
### Shareable Artifacts
Password-protected public URLs for artifacts. See `docs/developer-guides/shareable-artifact-links-guide.md`.

**Gotchas:**
- OG image requires `runtime = 'nodejs'` (Prisma incompatible with edge)
- Use full slug in cookie name to prevent collisions
```

## Execute

Now read the target CLAUDE.md and produce a condensed version that maximizes signal density while preserving all critical gotchas and patterns.
