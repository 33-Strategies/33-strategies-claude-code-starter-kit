# CLAUDE.md — [YOUR_PROJECT_NAME]

> **Note**: This is a template. Search for `[CUSTOMIZE]` to find sections that need project-specific details.

## Project Purpose

[CUSTOMIZE: Describe what this project does, who it's for, and its core value proposition. 2-3 sentences.]

---

## Directory Structure

[CUSTOMIZE: Update this to match your actual project structure]

```
/
├── CLAUDE.md                     # You are here
├── README.md                     # Project overview
├── package.json                  # Dependencies
├── tsconfig.json                 # TypeScript config (if applicable)
│
├── src/                          # Source code
│   ├── components/               # UI components
│   ├── lib/                      # Utilities and helpers
│   ├── hooks/                    # Custom React hooks
│   └── styles/                   # Stylesheets
│
├── tests/                        # Test files
│
└── docs/                         # Documentation
```

---

## Local Development

[CUSTOMIZE: Add your specific development commands]

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Run tests
npm test

# Build for production
npm run build
```

**Local URL:** [CUSTOMIZE: e.g., http://localhost:3000]

### Environment Variables

[CUSTOMIZE: List required environment variables]

```bash
cp .env.example .env
```

Required variables:
- `DATABASE_URL` - Database connection string
- `API_KEY` - External API key
- [Add more as needed]

---

## Conventions

### Naming

- **Directories:** kebab-case (`user-profile`, `api-handlers`)
- **Components:** PascalCase (`UserProfile.tsx`, `ApiHandler.tsx`)
- **Utilities:** camelCase (`formatDate.ts`, `parseConfig.ts`)
- **Constants:** SCREAMING_SNAKE_CASE (`MAX_RETRIES`, `API_BASE_URL`)

### File Organization

[CUSTOMIZE: Add your specific file organization rules]

- Keep related files close together
- One component per file
- Co-locate tests with source files when possible

### Code Style

[CUSTOMIZE: Reference your linting/formatting setup]

- Use ESLint and Prettier (config in `.eslintrc` and `.prettierrc`)
- Run `npm run lint` before committing
- TypeScript strict mode enabled

---

## Common Patterns

### Error Handling

```typescript
// Prefer explicit error types
type Result<T> = { success: true; data: T } | { success: false; error: string };

async function fetchData(): Promise<Result<Data>> {
  try {
    const data = await api.get('/endpoint');
    return { success: true, data };
  } catch (error) {
    return { success: false, error: error.message };
  }
}
```

### React Strict Mode Initialization

Prevent duplicate initialization in development (Strict Mode mounts components twice):

```tsx
const MyComponent = () => {
  const [data, setData] = useState([]);
  const hasInitialized = useRef(false);

  useEffect(() => {
    if (hasInitialized.current) return;
    hasInitialized.current = true;
    initializeData();
  }, []);

  return <div>{/* ... */}</div>;
};
```

### Async Operations

```typescript
// Use AbortController for cancellable requests
useEffect(() => {
  const controller = new AbortController();

  fetchData({ signal: controller.signal })
    .then(setData)
    .catch(err => {
      if (err.name !== 'AbortError') handleError(err);
    });

  return () => controller.abort();
}, []);
```

---

## Troubleshooting

### Common Issues

**Build fails with type errors:**
- Run `npm run typecheck` to see all errors
- Check that all imports resolve correctly
- Verify environment variables are set

**Tests failing locally but passing in CI:**
- Check for timezone-dependent assertions
- Verify test isolation (no shared state)
- Run tests in sequence: `npm test -- --runInBand`

**Hot reload not working:**
- Clear `.next` or `dist` cache folders
- Restart the dev server
- Check for circular imports

---

## Dependencies

[CUSTOMIZE: List your core dependencies and their purposes]

```json
{
  "dependencies": {
    "react": "^18.x",
    "typescript": "^5.x"
  }
}
```

---

## Task Management

This project uses **STM (Simple Task Master)** for task tracking. STM should be used for:
- Tracking implementation tasks from specifications
- Managing dependencies between tasks
- Persisting task state across sessions

### Common STM Commands

```bash
# List all tasks
stm list --pretty

# Add a new task
stm add "Task title" --description "Brief description" --validation "Acceptance criteria"

# Update task status
stm update <id> --status in_progress
stm update <id> --status completed

# Search tasks
stm grep "pattern"
```

### Workflow Integration

1. **Specification Decomposition**: Use `/spec:decompose <spec-file>` to break specs into tasks
2. **Task Execution**: Use `/spec:execute` to implement decomposed tasks
3. **Progress Tracking**: Use `stm list --pretty` to monitor progress

---

## Deployment

[CUSTOMIZE: Add your deployment process]

### Staging

```bash
# Deploy to staging
npm run deploy:staging
```

### Production

```bash
# Deploy to production
npm run deploy:production
```

### Environment Configuration

[CUSTOMIZE: Document your environment setup]

| Environment | URL | Branch |
|-------------|-----|--------|
| Development | localhost:3000 | - |
| Staging | staging.example.com | develop |
| Production | example.com | main |

---

## Available Commands

This project includes Claude Code slash commands for common workflows:

### Specification Workflow
- `/spec:ideate <topic>` - Structured ideation with documentation
- `/spec:create <description>` - Generate a feature specification
- `/spec:validate <path>` - Validate spec completeness
- `/spec:decompose <path>` - Break spec into implementable tasks
- `/spec:execute` - Implement decomposed tasks

### Code Quality
- `/code-review` - Multi-aspect code review
- `/validate-and-fix` - Run quality checks and auto-fix issues

### Git Workflow
- `/git:commit` - Create commit following project conventions
- `/git:status` - Analyze current git state
- `/checkpoint:create` - Create a git stash checkpoint
- `/checkpoint:restore` - Restore from checkpoint

### Research & Context
- `/task-context <brief>` - Quick context discovery for a task
- `/preflight-discovery <brief>` - Comprehensive discovery workflow
- `/research <question>` - Deep research with citations

### Documentation
- `/create-dev-guide <area>` - Write a developer guide
- `/docs:sync` - Update documentation with recent changes

---

## Skills

[CUSTOMIZE: Document any project-specific skills you create]

Skills are project-specific knowledge bases in `.claude/skills/`. They encode:
- Brand guidelines and design systems
- Domain expertise and business logic
- Technical patterns specific to this project

To create a new skill, add a `.md` file to `.claude/skills/` with structured knowledge.

---

## Getting Help

- Check existing code for patterns and examples
- Use `/task-context <topic>` to find relevant context
- Reference framework documentation
- Use specialized agents for domain expertise (TypeScript, React, databases, etc.)

---

## [CUSTOMIZE: Project-Specific Sections]

Add sections specific to your project:
- API documentation
- Database schema
- Authentication patterns
- Third-party integrations
- Business logic rules
