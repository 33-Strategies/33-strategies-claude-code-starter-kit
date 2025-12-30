# Hooks Guide

Understanding and customizing Claude Code hooks.

## What Are Hooks?

Hooks are automated actions that run in response to Claude Code events. They provide:

- **Quality gates** - Prevent bad code from persisting
- **Automation** - Run linting, testing, validation automatically
- **Guardrails** - Protect sensitive files from modification

## Hook Types

### PreToolUse

Runs **before** a tool executes. Can block the tool execution.

**Use cases:**
- File protection (prevent modifying sensitive files)
- Validation before writes
- Permission checks

**Example:**
```json
{
  "matcher": "Write|Edit",
  "hooks": [{
    "type": "command",
    "command": "check-file-protection {{file}}"
  }]
}
```

### PostToolUse

Runs **after** a tool executes successfully.

**Use cases:**
- Linting changed files
- Type checking
- Running related tests
- Formatting

**Example:**
```json
{
  "matcher": "Write|Edit|MultiEdit",
  "hooks": [{
    "type": "command",
    "command": "npm run lint -- {{file}}"
  }]
}
```

### Stop

Runs when Claude Code session ends or pauses.

**Use cases:**
- Full project validation
- Test suite execution
- Final quality checks

**Example:**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "npm test"
  }]
}
```

### SessionStart

Runs when a new Claude Code session begins.

**Use cases:**
- Environment validation
- Dependency checks
- Status reporting

### UserPromptSubmit

Runs when user submits a prompt.

**Use cases:**
- Context injection
- Prompt preprocessing
- Logging

### SubagentStop

Runs when a subagent completes.

**Use cases:**
- Subagent output validation
- Cleanup after specialized tasks

## Configuration

Hooks are configured in `.claude/settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [...],
    "PostToolUse": [...],
    "Stop": [...],
    "SessionStart": [...],
    "UserPromptSubmit": [...],
    "SubagentStop": []
  }
}
```

## Matchers

Matchers determine which tool invocations trigger a hook.

### Single Tool
```json
{ "matcher": "Write" }
```

### Multiple Tools (OR)
```json
{ "matcher": "Write|Edit|MultiEdit" }
```

### All Tools
```json
{ "matcher": "*" }
```

### Pattern Matching
```json
{ "matcher": "Bash(*)" }
```

## Hook Commands

### Using claudekit-hooks

The starter kit uses `claudekit-hooks` for hook execution:

```json
{
  "type": "command",
  "command": "claudekit-hooks run lint-changed"
}
```

Available claudekit hooks:
- `file-guard` - Protect sensitive files
- `lint-changed` - Lint modified files
- `typecheck-changed` - Type check modified files
- `test-changed` - Run tests for modified files
- `check-comment-replacement` - Detect comment-only replacements
- `check-unused-parameters` - Detect unused parameters
- `typecheck-project` - Full project type check
- `lint-project` - Full project lint
- `test-project` - Full test suite
- `check-todos` - Verify STM tasks
- `self-review` - AI self-review of changes

### Custom Commands

Use any shell command:

```json
{
  "type": "command",
  "command": "npm run lint -- {{file}}"
}
```

### Available Variables

- `{{file}}` - The file being operated on
- `{{tool}}` - The tool name
- `{{args}}` - Tool arguments (JSON)

## Default Configuration

The starter kit includes these default hooks:

### PreToolUse
```json
[{
  "matcher": "Read|Edit|MultiEdit|Write|Bash",
  "hooks": [{
    "type": "command",
    "command": "claudekit-hooks run file-guard"
  }]
}]
```

### PostToolUse
```json
[
  {
    "matcher": "Write|Edit|MultiEdit",
    "hooks": [
      { "command": "claudekit-hooks run lint-changed" },
      { "command": "claudekit-hooks run typecheck-changed" },
      { "command": "claudekit-hooks run test-changed" }
    ]
  },
  {
    "matcher": "Edit|MultiEdit",
    "hooks": [
      { "command": "claudekit-hooks run check-comment-replacement" },
      { "command": "claudekit-hooks run check-unused-parameters" }
    ]
  }
]
```

### Stop
```json
[{
  "matcher": "*",
  "hooks": [
    { "command": "claudekit-hooks run typecheck-project" },
    { "command": "claudekit-hooks run lint-project" },
    { "command": "claudekit-hooks run test-project" },
    { "command": "claudekit-hooks run check-todos" },
    { "command": "claudekit-hooks run self-review" }
  ]
}]
```

## Customization Examples

### For Python Projects

```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write|Edit|MultiEdit",
      "hooks": [
        { "type": "command", "command": "ruff check --fix {{file}}" },
        { "type": "command", "command": "mypy {{file}}" },
        { "type": "command", "command": "pytest {{file}} -x" }
      ]
    }],
    "Stop": [{
      "matcher": "*",
      "hooks": [
        { "type": "command", "command": "ruff check ." },
        { "type": "command", "command": "mypy ." },
        { "type": "command", "command": "pytest" }
      ]
    }]
  }
}
```

### For Go Projects

```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write|Edit|MultiEdit",
      "hooks": [
        { "type": "command", "command": "gofmt -w {{file}}" },
        { "type": "command", "command": "go vet {{file}}" }
      ]
    }],
    "Stop": [{
      "matcher": "*",
      "hooks": [
        { "type": "command", "command": "go build ./..." },
        { "type": "command", "command": "go test ./..." }
      ]
    }]
  }
}
```

### Minimal Configuration

For projects that want basic hooks only:

```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write|Edit|MultiEdit",
      "hooks": [{
        "type": "command",
        "command": "npm run lint -- --fix {{file}}"
      }]
    }]
  }
}
```

### Disable All Hooks

```json
{
  "hooks": {
    "PreToolUse": [],
    "PostToolUse": [],
    "Stop": []
  }
}
```

## Troubleshooting

### Hooks not running

1. Check `.claude/settings.json` is valid JSON
2. Verify matcher pattern matches the tool
3. Check claudekit is installed: `claudekit status`

### Hooks timing out

1. Simplify commands (avoid full test suites on every edit)
2. Use targeted commands (`lint {{file}}` vs `lint .`)
3. Consider moving slow checks to Stop hooks

### Hook blocks unexpectedly

1. Check PreToolUse hooks for overly strict guards
2. Review file protection patterns
3. Check command exit codes (non-zero blocks)

### Hook output not visible

Hooks run silently by default. To debug:
1. Run the command manually to see output
2. Check Claude Code logs
3. Add explicit logging to hook commands

## Best Practices

1. **Keep PostToolUse fast** - Run on changed files only
2. **Use Stop for comprehensive checks** - Full test suites belong here
3. **Protect sensitive files** - Use PreToolUse file guards
4. **Fail fast** - Exit early on errors
5. **Be specific** - Target hooks to relevant tools only
