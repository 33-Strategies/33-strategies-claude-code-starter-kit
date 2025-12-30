# Customization Guide

This guide walks through customizing the starter kit for your specific project.

## Overview

The starter kit has three layers:

1. **Universal** - Works on any project, no changes needed
2. **Configurable** - Works with minor configuration changes
3. **Project-Specific** - You create for your domain/brand

## Step-by-Step Customization

### Step 1: Update CLAUDE.md (Required)

Search for `[CUSTOMIZE]` markers and update:

#### Project Purpose
```markdown
## Project Purpose

[CUSTOMIZE: Describe what this project does]
```

Change to your actual project description:
```markdown
## Project Purpose

This is an e-commerce platform built with Next.js 14, Prisma, and Stripe.
It serves small business owners who want to sell products online.
```

#### Directory Structure
Update to match your actual project layout:
```markdown
## Directory Structure

```
/
├── src/
│   ├── app/           # Next.js App Router pages
│   ├── components/    # React components
│   ├── lib/           # Utilities
│   └── db/            # Prisma schema and migrations
├── prisma/
│   └── schema.prisma
└── tests/
```

#### Local Development
Add your specific commands:
```markdown
## Local Development

```bash
# Start database
docker compose up -d postgres

# Run migrations
npx prisma migrate dev

# Start dev server
npm run dev
```

**Local URL:** http://localhost:3000
```

### Step 2: Configure Hooks (Recommended)

Edit `.claude/settings.json` to match your tooling.

#### If using different linting tools:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit|MultiEdit",
        "hooks": [
          {
            "type": "command",
            "command": "npx eslint --fix {{file}}"
          }
        ]
      }
    ]
  }
}
```

#### If you want to disable certain hooks:

Remove the hook entry or comment out by changing the matcher to something that won't match.

#### If you want to add project-specific validation:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit|MultiEdit",
        "hooks": [
          {
            "type": "command",
            "command": "npm run validate-imports"
          }
        ]
      }
    ]
  }
}
```

### Step 3: Set Permissions (Optional)

Create `.claude/settings.local.json` for tools that shouldn't require approval:

```json
{
  "permissions": {
    "allow": [
      "Bash(npm:*)",
      "Bash(npx:*)",
      "Bash(git:*)",
      "Bash(docker:*)",
      "SlashCommand(/spec:*)",
      "SlashCommand(/git:*)",
      "SlashCommand(/checkpoint:*)",
      "WebSearch",
      "Read(/path/to/project/**)"
    ]
  }
}
```

**Common permission patterns:**
- `Bash(npm:*)` - Allow all npm commands
- `Bash(git:*)` - Allow all git commands
- `SlashCommand(/spec:*)` - Allow all spec workflow commands
- `Read(/path/**)` - Allow reading files in a directory
- `WebFetch(domain:docs.example.com)` - Allow fetching from specific domains

### Step 4: Create Project Skills (Recommended)

Skills encode domain knowledge. Create them in `.claude/skills/`.

#### Design System Skill

`.claude/skills/design-system.md`:
```markdown
# [Company] Design System

## Brand Colors

| Name | Hex | Usage |
|------|-----|-------|
| Primary | #2563EB | CTAs, primary actions |
| Secondary | #64748B | Supporting text |
| Accent | #F59E0B | Highlights, badges |
| Background | #FFFFFF | Page background |
| Surface | #F8FAFC | Card backgrounds |

## Typography

- **Display**: Inter Bold, 48px
- **Heading**: Inter Semibold, 24-32px
- **Body**: Inter Regular, 16px
- **Caption**: Inter Regular, 14px

## Component Patterns

### Buttons
- Primary: Blue background, white text, rounded-lg
- Secondary: White background, blue border
- Ghost: Transparent, blue text on hover

### Cards
- Background: Surface color
- Border: 1px slate-200
- Shadow: shadow-sm
- Padding: 24px
- Border radius: rounded-xl
```

#### Domain Knowledge Skill

`.claude/skills/business-logic.md`:
```markdown
# E-commerce Business Logic

## Order States

1. **pending** - Order created, awaiting payment
2. **paid** - Payment received, ready for fulfillment
3. **processing** - Being prepared for shipment
4. **shipped** - In transit to customer
5. **delivered** - Successfully delivered
6. **cancelled** - Order cancelled (refund issued if paid)

## Pricing Rules

- Tax calculated at checkout based on shipping address
- Shipping free for orders over $50
- Discount codes apply before tax calculation
- Maximum 1 discount code per order

## Inventory Management

- Stock decremented on order creation
- Stock restored if order cancelled within 1 hour
- Low stock alert at 10 units
- Out of stock products hidden from search
```

### Step 5: Customize Commands (Optional)

Some commands reference project-specific things. Update them if needed.

#### Git Commit Command

If your project has specific commit conventions, edit `.claude/commands/git/commit.md` to reference them.

#### Deploy Command

If you use a specific deployment platform, create a deploy command:

`.claude/commands/deploy/production.md`:
```markdown
---
description: Deploy to production
---

# Deploy to Production

Execute the following deployment workflow:

1. Run all tests: `npm test`
2. Build the project: `npm run build`
3. Deploy to Vercel: `vercel --prod`
4. Verify deployment: Check the production URL
5. Report status to user
```

### Step 6: Add Framework-Specific Agents (Optional)

If your project uses frameworks not covered by default agents, create custom ones.

`.claude/agents/prisma-expert.md`:
```markdown
---
name: prisma-expert
description: Prisma ORM expert for database schema design and query optimization
tools: Read, Grep, Glob, Bash, Edit
---

# Prisma Expert

You are an expert in Prisma ORM with deep knowledge of:

## Core Expertise
- Schema design with relations
- Query optimization
- Migration strategies
- Type-safe database access

## Common Tasks
- Design efficient schemas
- Optimize N+1 query problems
- Plan zero-downtime migrations
- Debug connection issues

## Best Practices
- Always use transactions for multi-step operations
- Prefer `findUnique` over `findFirst` when possible
- Use `select` to limit returned fields
- Index frequently queried fields
```

## Common Customization Patterns

### For Next.js Projects

Add to CLAUDE.md:
```markdown
## Framework: Next.js

- App Router (app/ directory)
- Server Components by default
- Use 'use client' directive for client components
- API routes in app/api/
```

### For Python Projects

Update hooks to use Python tools:
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit|MultiEdit",
        "hooks": [
          { "type": "command", "command": "ruff check --fix {{file}}" },
          { "type": "command", "command": "mypy {{file}}" }
        ]
      }
    ]
  }
}
```

### For Monorepo Projects

Add workspace awareness to CLAUDE.md:
```markdown
## Monorepo Structure

- `packages/web` - Next.js frontend
- `packages/api` - Express backend
- `packages/shared` - Shared types and utilities

When working across packages, consider:
- Import from @company/shared for shared code
- Run tests from package directory
- Build dependencies before dependent packages
```

## Testing Your Configuration

After customization:

1. Start a new Claude Code session
2. Run `/task-context test feature` - verify context gathering works
3. Make a small code change - verify hooks run
4. Try `/spec:create simple feature` - verify workflow works

## Troubleshooting

### Skills not being used

- Verify file is in `.claude/skills/`
- Check file has `.md` extension
- Reference skill explicitly in prompts if needed

### Custom agent not found

- Verify file is in `.claude/agents/`
- Check frontmatter has `name` field
- Reference by exact name from frontmatter

### Hooks timing out

- Simplify hook commands
- Check commands actually exist
- Consider using async hooks for slow operations
