# Skills Directory

Skills are project-specific knowledge bases that encode domain expertise, brand guidelines, or specialized technical patterns.

## What is a Skill?

A skill is a markdown file that contains structured knowledge Claude Code can reference when working on your project. Unlike commands (which define workflows) or agents (which define personas), skills encode **knowledge**.

## When to Create a Skill

Create a skill when you have:
- **Brand/Design Guidelines**: Typography, colors, component patterns, voice and tone
- **Domain Expertise**: Business logic rules, industry terminology, regulatory requirements
- **Technical Patterns**: Project-specific architectures, integration patterns, API conventions
- **Quality Standards**: Code review criteria, documentation templates, testing requirements

## Skill Structure

```markdown
# [Skill Name]

## Overview
Brief description of what this skill covers.

## Core Principles
Key rules or guidelines that should always be followed.

## Detailed Knowledge

### Category 1
Specific knowledge, patterns, or rules.

### Category 2
More specific knowledge.

## Examples
Concrete examples showing how to apply this knowledge.

## Anti-patterns
What NOT to do, with explanations of why.
```

## How Skills are Used

1. **Reference in Commands**: Commands can load skills for context
2. **Agent Knowledge**: Agents can reference skills for domain expertise
3. **Claude Context**: Skills inform Claude's understanding of your project

## Example: Design System Skill

```markdown
# Acme Design System

## Overview
Design guidelines for Acme Corp's web applications.

## Typography
- **Headings**: Inter Bold
- **Body**: Inter Regular
- **Code**: JetBrains Mono

## Colors
- **Primary**: #3B82F6 (blue-500)
- **Accent**: #F59E0B (amber-500)
- **Background**: #FFFFFF

## Component Patterns
### Buttons
- Primary: Blue background, white text
- Secondary: White background, blue border
- Destructive: Red background, white text

## Anti-patterns
- Never use more than 3 colors in a single view
- Avoid gradients except for hero sections
```

## Creating Your First Skill

1. Create a new `.md` file in this directory
2. Follow the structure above
3. Reference it in your commands or CLAUDE.md
4. Test by asking Claude about topics covered in the skill
