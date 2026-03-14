---
name: best-practices-research
description: Reference guide for researching and applying UI/UX, prompt engineering, and technical implementation best practices. Use when building features, reviewing designs, or validating approaches.
---

# Best Practices Research Guide

This skill provides structured guidance for researching and applying current best practices across three domains: UI/UX, Prompt Engineering, and Technical Implementation. Use it flexibly whenever you need to validate an approach or find better patterns.

---

## When to Use This Skill

- **Before implementing** a new feature (validate the approach)
- **During code review** (check if patterns follow best practices)
- **When stuck** on a design or technical decision
- **After initial ideation** (audit and improve the plan)
- **When debugging UX issues** (find proven patterns)

---

## Domain 1: UI/UX Best Practices

### Research Queries to Use

```
"[feature type] UX best practices 2024 2025"
"[interaction pattern] design patterns accessibility"
"[component type] UI patterns user research"
```

### Key Sources (2024-2025)

| Source | Best For |
|--------|----------|
| [Nielsen Norman Group](https://nngroup.com) | Research-backed UX principles |
| [Smashing Magazine](https://smashingmagazine.com) | Practical UI patterns |
| [A List Apart](https://alistapart.com) | Web standards and accessibility |
| [Baymard Institute](https://baymard.com) | E-commerce and form UX |

### Core Principles to Validate Against

**Conversational/Chat UI:**
- One question at a time (don't overwhelm)
- Proactive suggestions during silence
- Context awareness across turns
- Clear typing/loading indicators
- Easy conversation restart/clear

**Forms and Input:**
- Progressive disclosure (show complexity gradually)
- Inline validation with helpful messages
- Smart defaults based on context
- Autosave for long forms

**Feedback and Response:**
- Immediate acknowledgment of actions
- Clear success/error states
- Undo capabilities for destructive actions
- Progress indicators for long operations

**Information Architecture:**
- Balance depth vs. breadth
- Sufficient info without overwhelm
- Clear hierarchy and scannable content
- Contextual help where needed

### Red Flags to Watch For

- Multiple questions in one prompt
- No loading/typing indicators
- Silent failures
- Walls of text without structure
- Hidden undo/cancel options
- Inconsistent interaction patterns

---

## Domain 2: Prompt Engineering Best Practices

### Research Queries to Use

```
"LLM system prompt engineering best practices 2026"
"[task type] prompt patterns structured output"
"chain of thought prompting [use case]"
"tool calling agent design patterns"
```

### Key Sources (2024-2025)

| Source | Best For |
|--------|----------|
| [Anthropic Docs](https://docs.anthropic.com) | Claude-specific patterns |
| [OpenAI Cookbook](https://cookbook.openai.com) | GPT patterns and examples |
| [Palantir AIP Docs](https://palantir.com/docs/foundry/aip) | Enterprise prompt patterns |
| [LangChain Docs](https://docs.langchain.com) | Agent and chain patterns |

### Core Principles to Validate Against

**System Prompt Structure:**
```xml
<role>
Who the agent is and its core purpose
</role>

<context>
Domain knowledge, current state, relevant data
Use clear separators (XML tags, markdown headers)
</context>

<instructions>
Specific behavioral guidelines
What to do and what NOT to do
</instructions>

<output_format>
Expected response structure
Examples if needed (few-shot)
</output_format>
```

**Key Patterns:**

| Pattern | When to Use |
|---------|-------------|
| **Few-shot examples** | Complex formatting, nuanced classification |
| **Chain-of-thought** | Multi-step reasoning, math, logic |
| **Role prompting** | Consistent personality/expertise |
| **Constraint specification** | Preventing unwanted behaviors |
| **Tool calling** | Structured actions, human-in-the-loop |

**Tool Calling Best Practices:**
- Use `needsApproval: true` for actions with side effects
- Define clear tool descriptions
- Use Zod schemas for type-safe parameters
- Keep tool count manageable (3-7 tools ideal)
- Let model decide when to call (not forced)

### Red Flags to Watch For

- Ambiguous instructions (most failures come from ambiguity)
- Missing constraints on unwanted behavior
- No examples for complex output formats
- Overly long prompts without clear structure
- Forcing tool calls instead of letting model decide
- No separation between instructions and data

---

## Domain 3: Technical Implementation Best Practices

### Research Queries to Use

```
"[framework/SDK] best practices 2024"
"[pattern] implementation [language] production"
"[technology] streaming/caching/performance patterns"
```

### Key Sources by Technology

**Vercel AI SDK:**
| Source | URL |
|--------|-----|
| Official Docs | https://ai-sdk.dev/docs |
| Vercel Blog | https://vercel.com/blog (filter by AI SDK) |
| LogRocket Tutorial | https://blog.logrocket.com/nextjs-vercel-ai-sdk-streaming/ |

**Next.js:**
| Source | URL |
|--------|-----|
| Official Docs | https://nextjs.org/docs |
| Vercel Templates | https://vercel.com/templates |

**React Patterns:**
| Source | URL |
|--------|-----|
| React Docs | https://react.dev |
| Kent C. Dodds Blog | https://kentcdodds.com/blog |

### Vercel AI SDK Patterns (Current as of 2024)

**Streaming Chat Setup:**
```typescript
// Backend: /api/chat/route.ts
import { streamText } from 'ai';
import { openai } from '@ai-sdk/openai';

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai('gpt-4o'),
    system: systemPrompt,
    messages,
    tools: { /* ... */ },
  });

  return result.toDataStreamResponse();
}
```

```typescript
// Frontend: useChat hook
import { useChat } from 'ai/react';

const { messages, input, handleInputChange, handleSubmit, status } = useChat({
  api: '/api/chat',
});

// status: 'idle' | 'in_progress' | 'error'
// Use status for typing indicators
```

**Human-in-the-Loop Tools:**
```typescript
tools: {
  myAction: {
    description: 'Description for the model',
    parameters: z.object({ /* schema */ }),
    needsApproval: true, // User must confirm
    execute: async (params) => {
      // Only runs after approval
    },
  },
}
```

**Middleware for Context Injection:**
```typescript
// Refresh state before each request
const middleware = async (req: Request) => {
  const freshState = await getLatestState();
  // Inject into system prompt
};
```

### Red Flags to Watch For

- Not using framework's built-in patterns (reinventing the wheel)
- Missing error handling for streaming
- No loading/typing states
- Hardcoded values that should be configurable
- Missing TypeScript types for tool parameters
- Not using SSE for streaming (old polling patterns)

---

## Research Workflow

### Quick Validation (5 min)
1. Identify the specific pattern/decision to validate
2. Run 1-2 targeted web searches
3. Scan top 3 results for consensus
4. Apply or adjust based on findings

### Deep Dive (15-30 min)
1. Identify all three domains that apply
2. Run 2-3 searches per domain
3. Fetch and read top sources
4. Compile findings into structured recommendations
5. Prioritize by impact

### Output Format

When reporting findings, use this structure:

```markdown
## [Domain] Findings

**Current Approach:**
[What was proposed/implemented]

**Best Practice Research:**
- [Finding 1] ([Source](url))
- [Finding 2] ([Source](url))

**Recommendation:**
- [ ] [Specific change to make]
- [ ] [Specific change to make]
```

---

## Example Usage

### Scenario: Validating a chat UI design

**Step 1:** Search for chat UX best practices
```
"conversational AI chatbot UX best practices 2024"
"chat interface typing indicator patterns"
```

**Step 2:** Check against core principles
- Does it have typing indicators?
- Does it ask one question at a time?
- Are there quick-action suggestions?

**Step 3:** Apply findings
- Add character-by-character text reveal
- Include quick-action chips during silence
- Ensure proactive greeting based on context

---

## Sources Index

### UI/UX
- [Mind the Product - AI Chatbot UX](https://www.mindtheproduct.com/deep-dive-ux-best-practices-for-ai-chatbots/)
- [Netguru - Chatbot UX Tips 2025](https://www.netguru.com/blog/chatbot-ux-tips)
- [Springs Apps - Chatbot Best Practices](https://springsapps.com/knowledge/top-10-chatbot-best-practices-in-2024)

### Prompt Engineering
- [Palantir - Prompt Engineering](https://www.palantir.com/docs/foundry/aip/best-practices-prompt-engineering)
- [Dextralabs - Prompt Engineering Guide](https://dextralabs.com/blog/prompt-engineering-for-llm/)
- [Lakera - Prompt Engineering 2026](https://www.lakera.ai/blog/prompt-engineering-guide)

### Technical (Vercel AI SDK)
- [AI SDK Docs](https://ai-sdk.dev/docs/introduction)
- [Vercel AI SDK 6 Blog](https://vercel.com/blog/ai-sdk-6)
- [LogRocket - Streaming Tutorial](https://blog.logrocket.com/nextjs-vercel-ai-sdk-streaming/)
