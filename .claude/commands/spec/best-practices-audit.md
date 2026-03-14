---
description: Audit an ideation document against UI/UX, prompt engineering, and technical best practices via online research
category: spec
allowed-tools: WebSearch, WebFetch, Read, Write, Edit
argument-hint: "<path-to-ideation-doc>"
---

Audit the ideation document at $ARGUMENTS against current best practices in three areas:

1. **UI/UX Best Practices** (if the spec involves user interfaces)
2. **Prompt Engineering Best Practices** (if the spec involves LLM prompts/agents)
3. **Technical Implementation Best Practices** (for the specific technologies mentioned)

## Audit Process

### Step 1: Read and Analyze the Ideation Document

Read the ideation document and identify:
- What UI/UX patterns are proposed?
- Are there any LLM prompts or agent designs?
- What specific technologies are mentioned (frameworks, SDKs, APIs)?

### Step 2: Conduct Targeted Research

For each applicable area, conduct web searches to find current best practices (prioritize 2024-2025 sources):

**UI/UX Research Queries (if applicable):**
- Search for UX best practices specific to the interaction patterns proposed
- Look for design system guidance for the type of UI being built
- Find accessibility considerations for the proposed interactions

**Prompt Engineering Research Queries (if applicable):**
- Search for system prompt design patterns for the agent's purpose
- Look for structured extraction best practices if the agent needs to output structured data
- Find conversation flow and multi-turn dialogue best practices

**Technical Research Queries (if applicable):**
- Search for implementation patterns for the specific frameworks/SDKs mentioned
- Look for performance optimization and production best practices
- Find integration patterns if multiple systems are being combined

### Step 3: Generate Audit Report

Create a structured report with:

```markdown
# Best Practices Audit: [Feature Name]

## Executive Summary
[2-3 sentences on overall alignment with best practices and key gaps]

## UI/UX Audit
### Current Proposal Summary
[What the ideation proposes]

### Best Practice Findings
[Research findings with source links]

### Recommended Changes
- [ ] Change 1: [specific change]
- [ ] Change 2: [specific change]

## Prompt Engineering Audit
### Current Proposal Summary
[What the ideation proposes for prompts/agents]

### Best Practice Findings
[Research findings with source links]

### Recommended Changes
- [ ] Change 1: [specific change]
- [ ] Change 2: [specific change]

## Technical Implementation Audit
### Current Proposal Summary
[Technologies and patterns proposed]

### Best Practice Findings
[Research findings with source links]

### Recommended Changes
- [ ] Change 1: [specific change]
- [ ] Change 2: [specific change]

## Sources
- [Source 1](url)
- [Source 2](url)
...
```

### Step 4: Update Ideation Document

After presenting the audit report:
1. Ask the user which recommendations they want to incorporate
2. Edit the original ideation document to integrate approved changes
3. Mark the audit as complete

## Important Notes

- Skip any section that isn't relevant to the spec (e.g., skip prompt engineering if there are no LLM components)
- Prioritize actionable, specific recommendations over general advice
- Include source links for all research findings
- Focus on changes that would materially improve the feature, not minor optimizations
