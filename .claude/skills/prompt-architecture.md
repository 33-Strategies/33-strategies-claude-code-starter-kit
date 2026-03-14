---
name: prompt-architecture
description: Reference guide for designing production LLM prompt systems in this codebase. Covers the Two-Pass Gap Analysis pattern, structured extraction prompts, confidence scoring, dynamic rubric injection, and validation patterns. Use when building AI extraction, synthesis, or coaching features.
---

# Prompt Architecture Skill

This skill documents the prompt engineering patterns used in 33 Strategies production systems. Use when building new LLM-powered features, reviewing AI integration code, or debugging extraction/synthesis quality issues.

---

## When to Use This Skill

- **Building extraction features** (brain dumps, text-to-structured-data)
- **Implementing AI coaching** (conversational assistants with tool calling)
- **Adding scoring/assessment** systems
- **Reviewing prompt quality** in code review
- **Debugging LLM output issues** (missed info, wrong categories, confidence problems)

---

## Core Pattern: Two-Pass Gap Analysis

The signature pattern in this codebase: run two sequential LLM calls where the second critically audits the first.

### Why Two Passes?

Single-pass extraction commonly fails in predictable ways:
- **Misses information** mentioned in passing
- **Creates shallow summaries** that lose nuance
- **Assigns content to wrong categories**
- **Over-confident scoring** without sufficient evidence
- **Fragments related information** across fields

The second pass acts as a **quality auditor**, not a "find more stuff" pass.

### Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        EXTRACTION FLOW                          │
├─────────────────────────────────────────────────────────────────┤
│   ┌──────────────┐                                              │
│   │ Raw Text     │                                              │
│   │ (transcript, │                                              │
│   │  document)   │                                              │
│   └──────┬───────┘                                              │
│          │                                                      │
│          ▼                                                      │
│   ┌──────────────────────────────────────────────────┐          │
│   │            FIRST PASS (Extraction)               │          │
│   │  - Domain-specific extraction prompt             │          │
│   │  - Structured output schema (Zod)                │          │
│   │  - Over-extraction preferred                     │          │
│   └──────────────────────┬───────────────────────────┘          │
│                          │                                      │
│                          ▼                                      │
│   ┌──────────────────────────────────────────────────┐          │
│   │           SECOND PASS (Gap Analysis)             │          │
│   │  Inputs:                                         │          │
│   │    - Original raw text                           │          │
│   │    - First-pass extraction (JSON)                │          │
│   │  Outputs:                                        │          │
│   │    - Complete, final structured output           │          │
│   │    (same schema as first pass)                   │          │
│   └──────────────────────┬───────────────────────────┘          │
│                          │                                      │
│                          ▼                                      │
│   ┌──────────────────────────────────────────────────┐          │
│   │              VALIDATION & OUTPUT                 │          │
│   │  - Schema validation                             │          │
│   │  - Fuzzy key matching (if applicable)            │          │
│   │  - Return to frontend                            │          │
│   └──────────────────────────────────────────────────┘          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Implementation Template

```typescript
// First pass: Initial extraction
const { object: firstPassExtraction } = await generateObject({
  model: openai('gpt-4o'),
  schema: extractionSchema,
  system: EXTRACTION_SYSTEM_PROMPT,
  prompt: `${EXTRACTION_PROMPT}\n\nSOURCE:\n${sourceText}`,
});

// Second pass: Gap analysis (quality audit)
let finalExtraction = firstPassExtraction;
try {
  const { object: secondPassExtraction } = await generateObject({
    model: openai('gpt-4o'),
    schema: extractionSchema, // SAME schema - outputs complete set
    system: GAP_ANALYSIS_SYSTEM_PROMPT,
    prompt: `${GAP_ANALYSIS_PROMPT}

ORIGINAL SOURCE:
${sourceText}

FIRST PASS EXTRACTION:
${JSON.stringify(firstPassExtraction, null, 2)}

Return the complete, final extraction.`,
  });
  finalExtraction = secondPassExtraction;
} catch (error) {
  console.warn('[extract] Gap analysis failed, using first-pass:', error);
  // Graceful degradation: use first pass if second fails
}
```

### The Six Audit Scenarios

Instruct the gap analysis prompt to review for these specific quality issues:

| Scenario | Symptom | Action |
|----------|---------|--------|
| **MISSED ENTIRELY** | Info exists in source, no corresponding output | Create new item |
| **UNDER-EXTRACTED** | Item exists but shallow/incomplete | Replace with richer version |
| **MIS-CATEGORIZED** | Info captured in wrong field/category | Move to correct category |
| **OVER-CONFIDENT** | Confidence higher than evidence warrants | Lower confidence, adjust language |
| **FRAGMENTED** | Related info split across multiple items | Consolidate into unified item |
| **CONTRADICTED** | Later statements qualify earlier ones | Reflect full, accurate picture |

### Gap Analysis Prompt Template

```typescript
export const GAP_ANALYSIS_PROMPT = `You have completed a first-pass extraction.
Now perform a critical second-pass review with fresh eyes.

Your role is now that of a QUALITY AUDITOR — not just looking for missed
information, but evaluating whether the first pass truly captured the meaning,
nuance, and strategic value of the source material.

REVIEW FOR THESE SCENARIOS:

1. **MISSED ENTIRELY** → Create new item
2. **UNDER-EXTRACTED** → Replace with richer version
3. **MIS-CATEGORIZED** → Move to correct category
4. **OVER-CONFIDENT** → Adjust confidence and language
5. **FRAGMENTED** → Consolidate related items
6. **CONTRADICTED** → Reflect full, accurate picture

YOUR OUTPUT:

Return the COMPLETE, FINAL set of items:
1. **KEEP** first-pass items that are accurate and complete
2. **REPLACE** items that need correction (only improved version)
3. **ADD** new items for missed information
4. **OMIT** duplicative or consolidated items

Do NOT return "findings" or "observations." Return actual structured items.

Prioritize changes that materially affect understanding.`;
```

### Trade-offs

| Consideration | Impact |
|---------------|--------|
| **Latency** | Adds ~3-5 seconds for second LLM call |
| **Cost** | ~2x API cost per extraction |
| **Quality** | Typically catches 10-20% additional/improved items |
| **Complexity** | Minimal — same schema, additive code path |

### When to Use/Skip

**Use two passes for:**
- Long-form content (transcripts, documents, interviews)
- High-stakes extraction where quality > speed
- Domains with many possible categories
- Content where later statements qualify earlier ones

**Skip second pass when:**
- Latency is critical (real-time applications)
- Source text is short and simple (<500 chars)
- Cost sensitivity outweighs quality needs

---

## Current Implementations

### Clarity Canvas Brain Dump Extraction

**Files:**
- `lib/clarity-canvas/prompts.ts` — `BRAIN_DUMP_EXTRACTION_PROMPT`, `GAP_ANALYSIS_PROMPT`
- `lib/clarity-canvas/extraction-schema.ts` — Zod schemas
- `app/api/clarity-canvas/extract/route.ts` — Two-pass implementation

**Schema structure:**
```typescript
{
  chunks: [{
    content: string,       // Verbatim extracted text
    targetSection: string, // e.g., "individual", "goals"
    targetSubsection: string,
    targetField: string,
    summary: string,       // Max 150 chars for UI
    confidence: number,    // 0-1 scale
    insights: string[],    // Key implications
  }],
  overallThemes: string[],
  suggestedFollowUps: string[],
}
```

### Central Command Prospect Extraction

**Files:**
- `lib/central-command/prompts.ts` — Extraction + gap analysis prompts
- `lib/central-command/schemas.ts` — Zod schemas
- `app/api/central-command/extract/route.ts` — Two-pass implementation

**Key difference:** Includes synthesis sections (narrative analysis) + score assessments + operational recommendations.

---

## Prompt Structure Best Practices

### System Prompt Architecture

Use clear sections with XML-like structure or markdown headers:

```typescript
export const EXTRACTION_SYSTEM_PROMPT = `You are an expert at extracting
structured profile information from transcripts.

## Your Role
[Clear description of the AI's purpose and expertise]

## Context
[Domain knowledge, current state, relevant constraints]

## Output Requirements
[Expected structure, what to include/exclude]

## Rules
[Specific behavioral guidelines, numbered for clarity]
1. Extract content verbatim when possible
2. Rate confidence 0-1 based on clarity
3. DO NOT fabricate — but DO infer from stated facts
...`;
```

### Extraction Prompt Structure

Follow this proven pattern from Clarity Canvas:

```typescript
export const BRAIN_DUMP_EXTRACTION_PROMPT = `
// 1. DOMAIN OVERVIEW
You are an expert at extracting structured profile information from
unstructured speech transcripts.

// 2. TARGET STRUCTURE (explicit field list)
SECTIONS:
- individual: Who they are (background, thinking, working, values)
  - background: career, education, expertise, experience_years, industry
  - thinking: decision_making, problem_solving, risk_tolerance
  ...

// 3. EXTRACTION RULES (numbered, specific)
EXTRACTION RULES:
1. Extract content verbatim when possible
2. Map to MOST SPECIFIC field that fits
3. Generate concise summary (max 150 chars)
4. Rate confidence (0-1):
   - 0.8-1.0: Explicitly stated facts
   - 0.5-0.7: Reasonable inferences
   - Below 0.5: Do not include
5. DO NOT fabricate — but DO extract reasonable inferences
6. If relevant to multiple fields, create SEPARATE chunk for EACH
7. For role-based inferences, extract what is strongly implied
8. Capture quantities, numbers, specific details

// 4. OUTPUT FORMAT (explicit)
OUTPUT FORMAT:
Return a JSON object with:
- chunks: Array of extracted information
- overallThemes: High-level themes identified
- suggestedFollowUps: Questions to fill gaps`;
```

### Key Principles

1. **Explicit field enumeration** — List every valid field so the model knows what's available
2. **Numbered rules** — Makes it easy for model to reference specific behaviors
3. **Confidence scale definition** — Always define what 0.8 vs 0.5 means
4. **Over-extraction preference** — "Prefer over-extraction — user will review"
5. **Inference guidelines** — When it's OK to infer vs when to stay literal

---

## Confidence Scoring Pattern

### Standard Confidence Scale

```typescript
// In system prompt:
Rate your confidence (0-1) based on how clearly the information was stated:
- 0.9-1.0: Explicitly stated, unambiguous facts
- 0.7-0.89: Strongly implied, high confidence inference
- 0.5-0.69: Moderately implied, reasonable inference
- 0.3-0.49: Weak signal, some ambiguity
- Below 0.3: Do not include (too uncertain)
```

### Field-Level Confidence (Persona Sharpener)

For complex objects, track confidence per field:

```typescript
{
  persona: { /* ... */ },
  fieldConfidence: {
    demographicsAgeRange: 0.9,      // Explicitly stated
    demographicsLifestyle: 0.7,     // Strongly implied
    jobsFunctional: 0.8,
    goalsPrimary: 0.95,
    frustrationsMain: 0.6,          // Inferred from context
  }
}
```

---

## Dynamic Rubric Injection

For scoring systems that learn from feedback, inject calibrated rubrics into prompts.

### Pattern (Central Command)

```typescript
// lib/central-command/prompts.ts
export function buildExtractionSystemPrompt(
  rubrics: Record<ScoreDimension, RubricContent>
): string {
  // Format each rubric with high/medium/low indicators
  const scoreDimensions = Object.entries(rubrics)
    .map(([dim, rubric]) => {
      return `**${dim}** (${rubric.description}):
- Score 7-10 (High): ${rubric.indicators.high.join('; ')}
- Score 4-6 (Medium): ${rubric.indicators.medium.join('; ')}
- Score 1-3 (Low): ${rubric.indicators.low.join('; ')}`;
    })
    .join('\n\n');

  return `${EXTRACTION_PROMPT_BASE}

### Score Assessments
${scoreDimensions}

${EXTRACTION_PROMPT_SUFFIX}`;
}
```

### Rubric Structure

```typescript
interface RubricContent {
  description: string;
  indicators: {
    high: string[];    // What justifies 7-10
    medium: string[];  // What justifies 4-6
    low: string[];     // What justifies 1-3
  };
}
```

### Fallback Pattern

Always have fallback rubrics for when database is unavailable:

```typescript
const { rubrics, source } = await getRubricsWithFallback();
// source: 'database' | 'cache' | 'initial'
const systemPrompt = buildExtractionSystemPrompt(rubrics);
```

---

## Synthesis vs Extraction

This codebase distinguishes two output types:

### Extraction (Operational Data)
- Specific data points: names, dates, emails, numbers
- High precision, factual only
- Short, structured values
- Example: `{ contactName: "John", contactEmail: "john@co.com" }`

### Synthesis (Narrative Analysis)
- Analytical paragraphs connecting dots
- Inference and interpretation encouraged
- Longer, prose-style content
- Example: `{ strategicAssessment: "This prospect represents a strong fit because..." }`

### Central Command Pattern

```typescript
// In extraction prompt:
## Your Output Has Two Parts

### PART 1: Client Synthesis (PRIMARY — this is the main value)
Write a thorough, synthesized profile. This is NOT a summary — it's
an analysis. Connect dots, identify patterns, draw conclusions.

**companyOverview**: Who are they? What's their market position?
**goalsAndVision**: What are they trying to accomplish?
**painAndBlockers**: What's getting in their way?
**strategicAssessment**: Should we pursue? What makes them interesting or risky?
**recommendedApproach**: How should we pitch? What angle resonates?

### PART 2: Operational Extractions (SECONDARY)
Extract specific data points into recommendations:
- company_info → name, industry, website
- contact_info → contactName, contactEmail, contactPhone
- next_action → nextAction
```

---

## Validation & Error Handling

### Fuzzy Key Matching

LLMs sometimes output slightly wrong field names. Use fuzzy matching:

```typescript
// lib/clarity-canvas/key-matching.ts
const matchedFieldKey = fuzzyMatchKey(chunk.targetField, validFields);
if (!matchedFieldKey) {
  droppedChunks.push({
    reason: `Unknown field: "${chunk.targetField}"`,
    chunk
  });
  continue;
}
```

### Path Auto-Correction (Chat Coach)

For conversational tools, map common AI mistakes to valid paths:

```typescript
// In commit route
const FIELD_CORRECTIONS: Record<string, string> = {
  'main_goal': 'goals/immediate/current_focus',
  'objectives': 'goals/immediate/current_focus',
  'vision': 'goals/strategy/exit_vision',
  'company_description': 'organization/fundamentals/company_name',
  // ... 30+ common mistakes
};
```

### Graceful Degradation

Always fall back to first-pass results if second pass fails:

```typescript
let finalExtraction = firstPassExtraction;
try {
  const { object } = await generateObject({ /* second pass */ });
  finalExtraction = object;
} catch (error) {
  console.warn('[extract] Gap analysis failed, using first-pass');
  // Continue with first pass — user still gets usable output
}
```

---

## Observability

### Logging Pattern

Track extraction quality metrics:

```typescript
console.log(`[extract] First pass: ${firstPass.chunks.length} chunks`);
console.log(`[extract] Second pass: ${finalOutput.chunks.length} chunks`);

// Track changes between passes
const changes = analyzeChanges(firstPass, finalOutput);
console.log(`[extract] Gap analysis made ${changes.length} changes`);
```

### Change Analysis

Categorize what the gap analysis changed:

```typescript
interface GapAnalysisChange {
  type: 'added' | 'improved' | 'consolidated' | 'confidence_adjusted';
  description: string;
  fieldKey?: string;
}
```

---

## Quick Reference

### Prompt Checklist

Before deploying a new extraction prompt:

- [ ] **Clear role definition** — Who is the AI?
- [ ] **Explicit field enumeration** — All valid fields listed
- [ ] **Numbered rules** — Easy to reference, no ambiguity
- [ ] **Confidence scale defined** — What do 0.8 vs 0.5 mean?
- [ ] **Output format specified** — JSON structure documented
- [ ] **Inference guidelines** — When OK vs when to stay literal
- [ ] **Over-extraction preference** — User will review

### Two-Pass Checklist

- [ ] Same schema for both passes
- [ ] Gap analysis prompt includes all 6 audit scenarios
- [ ] "Return complete final set" instruction (not a diff)
- [ ] Original source + first-pass JSON in second prompt
- [ ] Graceful degradation (try/catch with fallback)
- [ ] Logging for both passes

### Common Prompt Failures

| Symptom | Likely Cause | Fix |
|---------|--------------|-----|
| Wrong categories | Ambiguous field definitions | Add explicit field descriptions |
| Low confidence everywhere | Confidence scale undefined | Define what 0.8 vs 0.5 means |
| Missing info | "Only extract explicit" too strict | Add inference guidelines |
| Hallucinated fields | Free-form field names allowed | Use Zod enums/literals |
| Verbose outputs | No length constraints | Add "max 150 chars" etc. |

---

## File Reference

| File | Purpose |
|------|---------|
| `lib/clarity-canvas/prompts.ts` | Brain dump extraction + gap analysis prompts |
| `lib/clarity-canvas/extraction-schema.ts` | Zod schemas for extraction |
| `lib/central-command/prompts.ts` | Prospect extraction + synthesis prompts |
| `lib/central-command/rubric.ts` | Dynamic rubric loading |
| `app/api/clarity-canvas/extract/route.ts` | Two-pass extraction implementation |
| `app/api/central-command/extract/route.ts` | Two-pass extraction with rubrics |
| `docs/reference/second-pass-gap-analysis-framework.md` | Detailed framework docs |

---

*Last Updated: February 2026*
