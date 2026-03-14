---
description: Audit a component or file against the 33 Strategies design system
allowed-tools: Read, Grep, Glob
argument-hint: "<file-path-or-component-name>"
---

# Design System Audit

Audit the specified component or file against the 33 Strategies brand identity and design language.

**Target:** $ARGUMENTS

## Step 1: Load the Design Skill

Read the design system specification:
@.claude/skills/33-strategies-frontend-design.md

## Step 2: Locate Target Files

If $ARGUMENTS is a file path, read that file directly.

If $ARGUMENTS is a component name or general reference, search for matching files:
```
Glob for: **/*$ARGUMENTS*
Grep for: $ARGUMENTS in components/
```

## Step 3: Audit Against Design System

For each target file, check compliance with these critical requirements:

### Typography
| Element | Required | Check For |
|---------|----------|-----------|
| Headlines | `font-display` class (Instrument Serif) | `fontFamily: 'Georgia'` or similar is WRONG |
| Body text | `font-body` class (DM Sans) | `fontFamily: 'Inter'` inline is WRONG |
| Labels/markers | `font-mono` class (JetBrains Mono) | Missing mono font on section labels |

### Colors
| Element | Correct Value | Common Mistakes |
|---------|--------------|-----------------|
| Gold accent | `#d4a54a` | `#D4A84B`, `#f59e0b`, other golds |
| Background | `#0a0a0f` | `#0a0a0a`, `#000000` (missing blue undertone) |
| Surface | `#111114` | `#111111`, `#18181b` |
| Gold glow | `rgba(212,165,74,0.3)` | Wrong opacity or RGB values |

### Patterns
- Section labels: Should use `font-mono font-medium tracking-[0.2em] uppercase`
- Headlines: Should use Tailwind classes, not inline fontFamily
- "33" in brand name: Should always be gold (`#d4a54a`)
- Key phrases in headlines: Should be wrapped in gold span

### Anti-Patterns to Flag
- Generic AI aesthetics (purple gradients)
- Inter font used directly
- Emojis in visualizations
- Startup SaaS energy
- Rounded pastel cards

## Step 4: Report Findings

Provide a structured report:

### Compliance Summary
- Overall status: PASS / NEEDS FIXES / CRITICAL ISSUES
- Typography compliance: X/X checks passed
- Color compliance: X/X checks passed
- Pattern compliance: X/X checks passed

### Issues Found
List each issue with:
- File and line number
- Current value
- Expected value
- Severity (Critical / Warning / Minor)

### Recommended Fixes
Provide specific code changes needed, in order of priority.
