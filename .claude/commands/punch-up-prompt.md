---
description: Enriches user prompts with deep project context, execution details, and clear success criteria
allowed-tools: Read, Grep, Glob
argument-hint: "<paste-your-prompt-here>"
category: workflow
---

# Prompt Enhancement Protocol

You are executing the `punch-up-prompt` command. Follow this structured process to transform the user's original prompt into a contextually-grounded, execution-ready directive.

## Step 1: Re-familiarize with the Project

Read and absorb the following project documentation:

**Project Context:**
@CLAUDE.md

Focus on:
- Core project goals and purpose
- Current phase of development
- Key constraints and technical architecture
- Success criteria and acceptance requirements

## Step 2: Gather Current Context

Analyze the conversation history to understand:
- What specific sub-task or problem is currently being addressed
- Whether the user is debugging, iterating, brainstorming, or executing
- What bug, challenge, or ambiguity the user is trying to resolve right now
- Recent work completed and remaining tasks

## Step 3: Analyze the Original Prompt

The user's original prompt to enhance:

```
$ARGS
```

Extract from this prompt:
- **Functional intent**: What is it trying to accomplish?
- **Contextual need**: Why is this needed at this point in the project?
- **Expected output**: What format or deliverable is expected?
- **Current gaps**: What context or specificity is missing?

## Step 4: Punch It Up — With Precision

Produce an enhanced version of the prompt that is:

### Functionally Sharper
- More detailed and better scoped
- Clear on output format and expectations
- Specific about files, components, or systems involved
- Includes concrete acceptance criteria

### Contextually Anchored
- References specific elements from CLAUDE.md (architecture, patterns, constraints)
- Maps to relevant items in TASKLIST.md (phase, acceptance criteria, dependencies)
- Explains why this task matters to the project's success
- Connects to the broader project goals from README.md

### Execution-Ready
- Optimized for immediate action without additional clarification
- Includes specific file paths, function names, or component references
- Provides clear success criteria and testing steps
- Anticipates edge cases or common pitfalls from the codebase

## Output Format

Respond with:

```markdown
## 🎯 Punched-Up Prompt

[Your enhanced, context-rich version here]

---

## 📋 Context Integration

**Project Phase**: [Which phase/milestone this relates to]
**Related Tasks**: [TASKLIST.md items this addresses]
**Architecture Notes**: [Relevant patterns/constraints from CLAUDE.md]
**Success Criteria**: [How to verify completion]

---

## 🔍 Key Improvements Made

- [Improvement 1: e.g., "Added specific file paths from project structure"]
- [Improvement 2: e.g., "Integrated layout algorithm constraints from CLAUDE.md"]
- [Improvement 3: e.g., "Referenced current execution phase from TASKLIST.md"]
```

## Important Guidelines

- **Do not** simply rewrite the prompt generically
- **Do** integrate deep project context into every aspect of the request
- **Do** explain *why this task matters now* as much as *what to do*
- **Do** make the prompt immediately actionable for an AI agent
- **Do** reference specific files, functions, components, or systems from the codebase
- **Do** connect the task to broader project goals and current execution phase