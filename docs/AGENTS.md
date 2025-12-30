# Agents Reference

Complete reference for all included specialized agents.

## How Agents Work

Agents are specialized AI personas with domain expertise. Claude Code automatically selects appropriate agents based on task type, or you can explicitly request one.

**Automatic selection:**
- Claude Code matches your task to relevant agent expertise
- Multiple agents can be used in parallel for complex tasks

**Explicit request:**
- Mention the agent name in your request
- Example: "Use the typescript-type-expert to help with this generic"

---

## TypeScript Agents

### `typescript-expert`

General TypeScript language expertise.

**Use for:**
- Type annotations and inference
- Module organization
- Compiler configuration
- Language features (decorators, generics, etc.)
- Migration from JavaScript

### `typescript-type-expert`

Advanced type system specialist.

**Use for:**
- Complex generics
- Conditional types
- Template literal types
- Type inference optimization
- Utility type authoring
- Type-level programming

### `typescript-build-expert`

Build and compilation specialist.

**Use for:**
- tsconfig.json optimization
- Module resolution issues
- Build tool integration (webpack, esbuild, swc)
- Declaration file generation
- Incremental compilation

---

## React Agents

### `react-expert`

React component patterns and best practices.

**Use for:**
- Component architecture
- Hook patterns
- State management
- Re-rendering issues
- Event handling
- Ref management

### `react-performance-expert`

React performance optimization specialist.

**Use for:**
- DevTools Profiler analysis
- Memoization strategies (memo, useMemo, useCallback)
- Core Web Vitals
- Bundle optimization
- Virtualization for large lists
- Memory leak detection

---

## Framework Agents

### `nextjs-expert`

Next.js 13-15 framework specialist.

**Use for:**
- App Router patterns
- Server Components
- Routing and layouts
- Data fetching (Server Actions, RSC)
- Build optimization
- Deployment configuration

---

## Database Agents

### `database-expert`

General database architecture and design.

**Use for:**
- Schema design
- Query optimization
- Connection management
- Transaction handling
- ORM integration (Prisma, TypeORM, etc.)

### `postgres-expert`

PostgreSQL-specific expertise.

**Use for:**
- Advanced indexing strategies
- JSONB operations
- Partitioning
- Query plans (EXPLAIN ANALYZE)
- Connection pooling
- PostgreSQL-specific features

---

## DevOps & Infrastructure Agents

### `devops-expert`

DevOps practices and tooling.

**Use for:**
- CI/CD pipeline design
- Infrastructure as code
- Monitoring and observability
- Security hardening
- Deployment strategies

### `docker-expert`

Docker containerization specialist.

**Use for:**
- Dockerfile optimization
- Multi-stage builds
- Image size reduction
- Security hardening
- Docker Compose orchestration
- Production patterns

### `github-actions-expert`

GitHub Actions CI/CD specialist.

**Use for:**
- Workflow design
- Custom actions
- Matrix builds
- Secrets management
- Performance optimization
- Security best practices

---

## Testing Agents

### `playwright-expert`

Playwright E2E testing specialist.

**Use for:**
- Test architecture
- Cross-browser testing
- Visual regression
- CI/CD integration
- Debugging flaky tests
- Page Object patterns

### `quick-check-expert`

Quick functionality verification.

**Use for:**
- Ephemeral tests that don't persist
- Quick validation of changes
- Smoke testing
- One-off verification

---

## Code Quality Agents

### `code-review-expert`

Comprehensive code review across 6 aspects.

**Reviews:**
1. Architecture & Design
2. Code Quality
3. Security & Dependencies
4. Performance & Scalability
5. Testing Coverage
6. Documentation & API Design

### `linting-expert`

Code linting and static analysis.

**Use for:**
- ESLint configuration
- Prettier setup
- Custom rule creation
- Tool integration
- Standards enforcement

### `refactoring-expert`

Systematic code refactoring.

**Use for:**
- Code smell detection
- Refactoring patterns
- Safe transformations
- Maintaining behavior while improving structure

---

## Other Agents

### `git-expert`

Git version control expert.

**Use for:**
- Merge conflict resolution
- Branching strategies
- History rewriting
- Repository recovery
- Performance optimization

### `documentation-expert`

Technical documentation specialist.

**Use for:**
- Documentation structure
- Content organization
- Audience targeting
- Information architecture
- Readability optimization

### `css-styling-expert`

CSS and styling specialist.

**Use for:**
- CSS architecture
- Responsive design
- CSS-in-JS optimization
- Theme implementation
- Cross-browser compatibility
- Design systems

### `research-expert`

Deep research methodology.

**Use for:**
- Information gathering
- Source evaluation
- Structured analysis
- Citation management

### `triage-expert`

Issue diagnosis and prioritization.

**Use for:**
- Error analysis
- Performance issue diagnosis
- Initial problem assessment
- Routing to appropriate specialist

### `code-search`

Codebase navigation specialist.

**Use for:**
- Finding specific files
- Locating implementations
- Pattern matching
- Dependency tracking

---

## Agent Selection Tips

### Let Claude Choose

For most tasks, Claude Code automatically selects appropriate agents:
- "Fix this type error" → typescript-expert
- "This component re-renders too much" → react-performance-expert
- "Help me write tests" → playwright-expert

### Explicit Selection

Request specific agents for:
- Complex specialized tasks
- When automatic selection isn't optimal
- Learning about domain expertise

Examples:
- "Use the postgres-expert to optimize this query"
- "Have the refactoring-expert review this code"
- "Let the docker-expert help with this Dockerfile"

### Multiple Agents

Complex tasks may use multiple agents:
- "Review this PR" → code-review-expert (parallel aspects)
- "Add a new feature" → multiple specialists as needed

---

## Creating Custom Agents

See `.claude/agents/` for examples. Basic structure:

```markdown
---
name: my-custom-expert
description: What this agent specializes in
tools: Read, Grep, Glob, Bash, Edit
---

# Agent Name

You are an expert in [domain].

## Core Expertise
- Skill 1
- Skill 2
- Skill 3

## Common Tasks
- Task type 1
- Task type 2

## Best Practices
- Practice 1
- Practice 2

## Anti-patterns
- What NOT to do
```

Use `/create-subagent` for guided agent creation.
