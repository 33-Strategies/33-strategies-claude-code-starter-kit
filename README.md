# Claude Code Starter Kit

A pre-configured Claude Code infrastructure with production-ready commands, agents, and hooks. Start any project with a complete AI-assisted development workflow from day one.

## What's Included

### Commands (23)

Slash commands for common development workflows:

| Category | Commands | Purpose |
|----------|----------|---------|
| **Specification** | `/spec:ideate`, `/spec:create`, `/spec:validate`, `/spec:decompose`, `/spec:execute` | Full spec-driven development lifecycle |
| **Code Quality** | `/code-review`, `/validate-and-fix` | Multi-aspect review and automated fixes |
| **Git Workflow** | `/git:commit`, `/git:status`, `/git:push`, `/git:checkout` | Smart Git operations with conventions |
| **Checkpoints** | `/checkpoint:create`, `/checkpoint:list`, `/checkpoint:restore` | Safe experimentation with stash-based checkpoints |
| **Research** | `/task-context`, `/preflight-discovery`, `/research`, `/web-analyze` | Context gathering and research workflows |
| **Documentation** | `/create-dev-guide`, `/docs:sync`, `/create-e2e-test-plan` | Documentation generation and maintenance |
| **Infrastructure** | `/create-subagent`, `/create-command`, `/agents-md:init` | Extend your Claude Code setup |

### Agents (23)

Specialized AI personas for domain expertise:

| Domain | Agents |
|--------|--------|
| **TypeScript** | `typescript-expert`, `typescript-type-expert`, `typescript-build-expert` |
| **React** | `react-expert`, `react-performance-expert` |
| **Framework** | `nextjs-expert` |
| **Database** | `database-expert`, `postgres-expert` |
| **DevOps** | `devops-expert`, `docker-expert`, `github-actions-expert` |
| **Testing** | `playwright-expert`, `quick-check-expert` |
| **Quality** | `code-review-expert`, `linting-expert`, `refactoring-expert` |
| **Other** | `git-expert`, `documentation-expert`, `css-styling-expert`, `research-expert`, `triage-expert`, `code-search` |

### Hooks

Pre-configured automation that runs on every code change:

- **PreToolUse**: File guard checks before modifications
- **PostToolUse**: Lint, typecheck, and test changed files
- **Stop**: Full project validation when session ends

## Quick Start

### Option 1: Fork This Repository

1. Fork this repo to your GitHub account
2. Clone your fork to a new project directory
3. Customize `CLAUDE.md` (search for `[CUSTOMIZE]`)
4. Start Claude Code

### Option 2: Copy to Existing Project

```bash
# From your project root
git clone https://github.com/YOUR_ORG/claude-code-starter-kit.git /tmp/ccs
cp -r /tmp/ccs/.claude .
cp /tmp/ccs/CLAUDE.md .
rm -rf /tmp/ccs
```

### Option 3: Use as Template

Click "Use this template" on GitHub to create a new repository with this structure.

## Prerequisites

### Required: claudekit

The hooks system uses `claudekit-hooks` for file watching and validation. Install it:

```bash
npm install -g @anthropic/claudekit
# or
brew install anthropic/tap/claudekit
```

Verify installation:

```bash
claudekit status
```

### Required: STM (Simple Task Master)

Task management commands use STM. Install it:

```bash
npm install -g @anthropic/stm
# or
brew install anthropic/tap/stm
```

Verify installation:

```bash
stm --version
```

## Configuration

### 1. Customize CLAUDE.md

Open `CLAUDE.md` and search for `[CUSTOMIZE]`. Update:

- Project name and purpose
- Directory structure
- Local development commands
- Environment variables
- Deployment process

### 2. Configure Hooks (Optional)

Edit `.claude/settings.json` to customize hooks:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit|MultiEdit",
        "hooks": [
          {
            "type": "command",
            "command": "your-custom-lint-command"
          }
        ]
      }
    ]
  }
}
```

### 3. Add Project-Specific Skills (Optional)

Create skills for domain knowledge:

```bash
# Create a design system skill
cat > .claude/skills/design-system.md << 'EOF'
# Design System

## Colors
- Primary: #3B82F6
- Accent: #F59E0B

## Typography
- Headings: Inter Bold
- Body: Inter Regular
EOF
```

### 4. Set Permissions (Optional)

Create `.claude/settings.local.json` for auto-approved tools:

```json
{
  "permissions": {
    "allow": [
      "Bash(npm:*)",
      "Bash(git:*)",
      "SlashCommand(/spec:*)",
      "WebSearch"
    ]
  }
}
```

## Usage

### Starting a New Feature

```bash
# 1. Ideate and explore
/spec:ideate "Add user authentication"

# 2. Create detailed spec
/spec:create "OAuth2 authentication with Google and GitHub"

# 3. Validate spec is complete
/spec:validate specs/auth-feature.md

# 4. Decompose into tasks
/spec:decompose specs/auth-feature.md

# 5. Execute implementation
/spec:execute
```

### Code Review

```bash
# Multi-aspect review of recent changes
/code-review

# Or specify what to review
/code-review src/components/
```

### Safe Experimentation

```bash
# Before trying something risky
/checkpoint:create "Before refactoring auth module"

# Try the change...

# If it didn't work
/checkpoint:restore latest
```

### Research and Context

```bash
# Quick context for a task
/task-context "Add rate limiting to API endpoints"

# Deep research with citations
/research "Best practices for WebSocket authentication"
```

## Project Structure

```
.claude/
├── commands/           # Slash commands
│   ├── spec/          # Specification workflow
│   ├── git/           # Git operations
│   ├── checkpoint/    # Safe experimentation
│   └── ...
├── agents/            # Specialized AI personas
│   ├── typescript/    # TypeScript experts
│   ├── react/         # React experts
│   ├── database/      # Database experts
│   └── ...
├── skills/            # Project knowledge (add your own)
│   └── README.md      # How to create skills
└── settings.json      # Hook configuration
```

## Extending

### Create a New Command

```bash
/create-command my-command "What this command does"
```

Or manually create `.claude/commands/my-command.md`:

```markdown
---
description: What this command does
argument-hint: "<required-arg>"
---

# Command Name

Instructions for Claude to follow when this command is invoked.
```

### Create a New Agent

```bash
/create-subagent
```

Follow the prompts to create a specialized agent.

### Add a Skill

See `.claude/skills/README.md` for guidance on creating domain-specific knowledge bases.

## Troubleshooting

### Hooks not running

1. Verify claudekit is installed: `claudekit status`
2. Check `.claude/settings.json` syntax is valid JSON
3. Ensure matcher patterns match your tools

### Commands not appearing

1. Verify files are in `.claude/commands/`
2. Check frontmatter has `description` field
3. Restart Claude Code session

### Agents not available

1. Verify files are in `.claude/agents/`
2. Check agent file has proper frontmatter
3. Reference agents by exact filename (without `.md`)

## Contributing

Found a bug or want to add a feature? PRs welcome!

1. Fork this repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

MIT License - Use freely in your projects.

---

Built with love by [33 Strategies](https://33strategies.ai)
