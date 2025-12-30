# Commands Reference

Complete reference for all included slash commands.

## Specification Workflow

### `/spec:ideate <topic>`

Structured ideation for new features or changes.

**What it does:**
- Explores problem space
- Generates potential approaches
- Documents trade-offs
- Outputs to `docs/ideation/`

**Example:**
```
/spec:ideate Add user notifications system
```

### `/spec:create <description>`

Generate a detailed specification from a feature description.

**What it does:**
- Creates structured spec document
- Defines requirements and acceptance criteria
- Identifies dependencies and risks
- Outputs to `specs/`

**Example:**
```
/spec:create OAuth2 authentication with Google and GitHub providers
```

### `/spec:create-lean <description>`

Generate a minimal, focused specification (avoids over-engineering).

**Example:**
```
/spec:create-lean Add dark mode toggle to settings
```

### `/spec:validate <path>`

Validate a spec has sufficient detail for implementation.

**What it does:**
- Checks for completeness
- Identifies ambiguities
- Verifies acceptance criteria
- Reports gaps

**Example:**
```
/spec:validate specs/auth-feature.md
```

### `/spec:decompose <path>`

Break a validated spec into implementable tasks.

**What it does:**
- Creates STM tasks for each component
- Identifies dependencies
- Estimates complexity
- Sets validation criteria

**Example:**
```
/spec:decompose specs/auth-feature.md
```

### `/spec:execute`

Implement tasks from a decomposed specification.

**What it does:**
- Works through STM task list
- Implements each task
- Validates against acceptance criteria
- Marks tasks complete

### `/spec:ideate-to-spec <path>`

Transform an ideation document into a full specification.

**Example:**
```
/spec:ideate-to-spec docs/ideation/notifications.md
```

---

## Code Quality

### `/code-review [target]`

Multi-aspect code review using parallel expert agents.

**Aspects reviewed:**
1. Architecture & Design
2. Code Quality
3. Security & Dependencies
4. Performance & Scalability
5. Testing Coverage
6. Documentation & API Design

**Examples:**
```
/code-review                     # Review recent changes
/code-review src/components/     # Review specific directory
/code-review PR #123             # Review pull request
```

### `/validate-and-fix`

Run quality checks and automatically fix issues.

**What it does:**
- Runs linting
- Runs type checking
- Runs tests
- Attempts auto-fixes for common issues
- Reports unfixable issues

---

## Git Workflow

### `/git:commit`

Create a commit following project conventions.

**What it does:**
- Analyzes staged changes
- Generates commit message
- Follows conventional commits or project style
- Includes change summary

### `/git:status`

Analyze current git state with insights.

**What it does:**
- Shows modified files
- Identifies potential issues
- Suggests next actions

### `/git:push`

Push commits with safety checks.

**What it does:**
- Verifies no sensitive files
- Checks branch protection
- Provides status update

### `/git:checkout <branch>`

Smart branch creation with conventional naming.

**Examples:**
```
/git:checkout feature/add-auth
/git:checkout fix/login-bug
/git:checkout feature add-auth     # Auto-prefixes
```

### `/git:ignore-init`

Initialize .gitignore with Claude Code patterns.

---

## Checkpoints

### `/checkpoint:create [description]`

Create a git stash checkpoint for safe experimentation.

**Example:**
```
/checkpoint:create Before refactoring the auth module
```

### `/checkpoint:list`

List all available checkpoints.

### `/checkpoint:restore <id|latest>`

Restore to a previous checkpoint.

**Examples:**
```
/checkpoint:restore latest
/checkpoint:restore 2
```

---

## Research & Context

### `/task-context <brief>`

Quick context discovery for upcoming tasks.

**What it does:**
- Searches relevant documentation
- Finds related code
- Identifies patterns
- Summarizes key insights

**Example:**
```
/task-context Add rate limiting to API endpoints
```

### `/preflight-discovery <brief>`

Comprehensive discovery, planning, and execution workflow.

**What it does:**
- Full codebase reconnaissance
- Generates task dossier
- Plans implementation approach
- Documents findings in `docs/task-dossiers/`

**Example:**
```
/preflight-discovery Migrate from REST to GraphQL
```

### `/research <question>`

Deep research with parallel subagents and citations.

**Example:**
```
/research Best practices for WebSocket authentication in Next.js
```

### `/web-analyze <url> [prompt]`

Analyze live web page content.

**Examples:**
```
/web-analyze https://docs.example.com
/web-analyze https://competitor.com What features do they offer?
```

---

## Documentation

### `/create-dev-guide <area>`

Write a developer guide for a part of the application.

**Example:**
```
/create-dev-guide Authentication System
```

### `/docs:sync`

Update documentation with forward-looking knowledge from recent work.

### `/create-e2e-test-plan <feature>`

Generate E2E testing plan for a feature.

**Example:**
```
/create-e2e-test-plan User Registration Flow
```

---

## Infrastructure

### `/create-subagent`

Create a new specialized AI agent with domain expertise.

### `/create-command [name] [description]`

Create a new slash command.

**Example:**
```
/create-command deploy-staging "Deploy current branch to staging"
```

### `/agents-md:init`

Initialize project with AGENTS.md and symlinks.

### `/agents-md:cli <tool>`

Capture CLI tool documentation for AI reference.

**Example:**
```
/agents-md:cli prisma
```

---

## Development

### `/dev:cleanup`

Clean up debug files, test artifacts, and status reports.

### `/config:bash-timeout <duration>`

Configure bash timeout values.

**Example:**
```
/config:bash-timeout 5m
```

---

## GitHub

### `/gh:repo-init <name>`

Create a new GitHub repository with proper setup.

**Example:**
```
/gh:repo-init my-new-project
```

---

## Tips

### Command Arguments

- `<required>` - Must provide
- `[optional]` - Can omit
- Arguments can usually be natural language descriptions

### Chaining Commands

Commands work well in sequence:
```
/spec:ideate Add caching        # Explore idea
/spec:create Redis caching      # Create spec
/spec:validate specs/caching.md # Validate
/spec:decompose specs/caching.md # Break into tasks
/spec:execute                    # Implement
```

### Getting Help

- Type `/` to see available commands
- Commands with `-` show usage hints
- Check `.claude/commands/` for full documentation
