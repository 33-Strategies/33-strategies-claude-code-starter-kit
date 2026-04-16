---
allowed-tools: Task, Read, Grep
description: Analyzes a specification for completeness and context alignment
category: validation
argument-hint: "<path-to-spec-file>"
---

# Specification Completeness & Context Alignment Check

Analyze the specification at: $ARGUMENTS

## Analysis Framework

This command will analyze the provided specification document to determine if it contains sufficient detail for successful autonomous implementation, while also verifying alignment with the project's context ecosystem and identifying overengineering.

### Context Ecosystem Loading

**Before analyzing the spec, load the project's context foundation:**

1. Check for context documents:
   - `.claude/context/PROJECT_CONTEXT.md`
   - `.claude/context/SOUL_DOCUMENT.md`
   - `.claude/context/PROJECT_OWNER.md`

2. If found, extract:
   - Current phase and acceptance criteria
   - Project vision and target users
   - Decision log
   - Constraints and non-goals

3. Use this context as a **validation lens** — the spec should be consistent with the project's stated direction.

4. If no context documents are found:
   - Note this in the output
   - Skip context alignment checks
   - Proceed with standard validation

### Domain Expert Consultation

When analyzing specifications that involve specific technical domains:
- **Use specialized subagents** when analysis involves specific domains (TypeScript, React, testing, databases, etc.)
- Run `claudekit list agents` to see available specialized experts
- Match specification domains to expert knowledge for thorough validation
- Use general-purpose approach only when no specialized expert fits

### What This Check Evaluates:

The analysis evaluates four fundamental aspects, each with specific criteria:

#### 1. **WHY - Intent and Purpose**
- Background/Problem Statement clarity
- Goals and Non-Goals definition
- User value/benefit explanation
- Justification vs alternatives
- Success criteria

#### 2. **WHAT - Scope and Requirements**
- Features and functionality definition
- Expected deliverables
- API contracts and interfaces
- Data models and structures
- Integration requirements:
  - External system interactions?
  - Authentication mechanisms?
  - Communication protocols?
- Performance requirements
- Security requirements

#### 3. **HOW - Implementation Details**
- Architecture and design patterns
- Implementation phases/roadmap
- Technical approach:
  - Core logic and algorithms
  - All functions and methods fully specified?
  - Execution flow clearly defined?
- Error handling:
  - All failure modes identified?
  - Recovery behavior specified?
  - Edge cases documented?
- Platform considerations:
  - Cross-platform compatibility?
  - Platform-specific implementations?
  - Required dependencies per platform?
- Resource management:
  - Performance constraints defined?
  - Resource limits specified?
  - Cleanup procedures documented?
- Testing strategy:
  - Test purpose documentation (each test explains why it exists)
  - Meaningful tests that can fail to reveal real issues
  - Edge case coverage and failure scenarios
  - Follows project testing philosophy: "When tests fail, fix the code, not the test"
- Deployment considerations

#### 4. **ALIGNMENT - Context Ecosystem Consistency** *(new)*

**Only evaluated if context documents are present.**

- **Vision alignment**: Does this spec serve the project's stated vision and target users?
- **Phase fit**: Does the work described belong in the current phase? Are there items that should be deferred to a later phase?
- **Decision consistency**: Does the spec conflict with any logged decisions in the SOUL_DOCUMENT or PROJECT_CONTEXT?
- **Non-goal violation**: Does any part of the spec overlap with stated project-level non-goals?
- **Constraint respect**: Does the spec honor the project's stated constraints (technical, business, design)?
- **Project Owner gate**: Would the PROJECT_OWNER agent flag anything in this spec as off-vision or misaligned?

### Additional Quality Checks:

**Completeness Assessment**
- Missing critical sections
- Unresolved decisions
- Open questions

**Clarity Assessment**
- Ambiguous statements
- Assumed knowledge
- Inconsistencies

**Overengineering Assessment**
- Features not aligned with core user needs
- Premature optimizations
- Unnecessary complexity patterns

### Overengineering Detection:

**Core Value Alignment Analysis**
Evaluate whether features directly serve the core user need:
- Does this feature solve a real, immediate problem?
- Is it being used frequently enough to justify complexity?
- Would a simpler solution work for 80% of use cases?

**YAGNI Principle (You Aren't Gonna Need It)**
Be aggressive about cutting features:
- If unsure whether it's needed → Cut it
- If it's for "future flexibility" → Cut it
- If only 20% of users need it → Cut it
- If it adds any complexity → Question it, probably cut it

**Common Overengineering Patterns to Detect:**

1. **Premature Optimization**
   - Caching for rarely accessed data
   - Performance optimizations without benchmarks
   - Complex algorithms for small datasets
   - Micro-optimizations before profiling

2. **Feature Creep**
   - "Nice to have" features (cut them)
   - Edge case handling for unlikely scenarios (cut them)
   - Multiple ways to do the same thing (keep only one)
   - Features that "might be useful someday" (definitely cut)

3. **Over-abstraction**
   - Generic solutions for specific problems
   - Too many configuration options
   - Unnecessary plugin/extension systems
   - Abstract classes with single implementations

4. **Infrastructure Overhead**
   - Complex build pipelines for simple tools
   - Multiple deployment environments for internal tools
   - Extensive monitoring for non-critical features
   - Database clustering for low-traffic applications

5. **Testing Extremism**
   - 100% coverage requirements
   - Testing implementation details
   - Mocking everything
   - Edge case tests for prototype features

**Simplification Recommendations:**
- Identify features to cut from the spec entirely
- Suggest simpler alternatives
- Highlight unnecessary complexity
- Recommend aggressive scope reduction to core essentials

### Output Format:

The analysis will provide:
- **Summary**: Overall readiness assessment (Ready/Not Ready)
- **Context Alignment**: Assessment of fit with project context (Aligned/Misaligned/Partially Aligned/Not Evaluated)
- **Critical Gaps**: Must-fix issues blocking implementation
- **Alignment Issues**: Conflicts with project context (phase, vision, decisions, non-goals)
- **Missing Details**: Specific areas needing clarification
- **Risk Areas**: Potential implementation challenges
- **Overengineering Analysis**:
  - Non-core features that should be removed entirely
  - Complexity that doesn't align with usage patterns
  - Suggested simplifications or complete removal
- **Features to Cut**: Specific items to remove from the spec
- **Phase Deferral**: Items that should move to a future phase (with phase reference)
- **Essential Scope**: Absolute minimum needed to solve the core problem
- **Recommendations**: Next steps to improve the spec

### Example Context Alignment Issues:

**Example 1: Phase Mismatch**
- Spec includes: "Build admin dashboard for managing user roles"
- Context says: Current phase is "MVP — core user workflows only"
- Recommendation: Defer admin dashboard to Phase 2; add hardcoded roles for MVP

**Example 2: Non-Goal Violation**
- Spec includes: "Support for custom themes and branding"
- Context says: Non-goal: "White-labeling or multi-tenant customization"
- Recommendation: Remove custom theming; use project's standard design system

**Example 3: Decision Conflict**
- Spec includes: "Store user preferences in Redis for fast access"
- Context says: Decision log: "All state lives in PostgreSQL via Prisma — no additional data stores"
- Recommendation: Use PostgreSQL for preferences; respect the single-datastore decision

### Example Overengineering Detection:

**Example 1: Unnecessary Caching**
- Spec includes: "Cache user preferences with Redis"
- Analysis: User preferences accessed once per session
- Recommendation: Use in-memory storage or browser localStorage for MVP

**Example 2: Premature Edge Cases**
- Spec includes: "Handle 10,000+ concurrent connections"
- Analysis: Expected usage is <100 concurrent users
- Recommendation: Cut this entirely - let it fail at scale if needed

**Example 3: Over-abstracted Architecture**
- Spec includes: "Plugin system for custom validators"
- Analysis: Only 3 validators needed, all known upfront
- Recommendation: Implement validators directly, no plugin system needed

**Example 4: Excessive Testing Requirements**
- Spec includes: "100% code coverage with mutation testing"
- Analysis: Tool used occasionally, not mission-critical
- Recommendation: Focus on core functionality tests (70% coverage)

**Example 5: Feature Creep**
- Spec includes: "Support 5 export formats (JSON, CSV, XML, YAML, TOML)"
- Analysis: 95% of users only need JSON
- Recommendation: Cut all formats except JSON - YAGNI

This comprehensive analysis helps ensure specifications are implementation-ready, aligned with project context, and focused on core user needs.
