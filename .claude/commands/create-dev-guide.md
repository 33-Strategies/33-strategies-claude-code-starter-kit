---
description: Write a comprehensive developer guide for this part of the application
allowed-tools: Read, Grep, Glob, Write
argument-hint: "<area-or-feature-name>"
category: workflow
---

Write a comprehensive developer guide for this part of the application: `<ARGS>`

Focus on surfacing all the essential information a coding agent would need to understand and work on this area safely and effectively, whether directly or when touching related parts of the codebase.

## Investigation Approach

Before writing, systematically discover:

1. **Entry Points**: Use Glob to find main files (*.py, *.js, app.*, index.*, main.*)
2. **Data Flow**: Trace from user action → backend → data source → storage
3. **Read Core Files**: Focus on main entry points, API routes, service layers
4. **Identify Patterns**: Look for repeated structures (dual-source, fallback, validation, caching)
5. **Find Constants**: Grep for UPPERCASE variables, config objects, environment variables
6. **Trace Dependencies**: Map imports, check package.json/requirements.txt/imports

Use Glob/Grep strategically before reading files to narrow scope and reduce token usage.

## Required Sections

### 0. Architecture Overview
- Create a text-based diagram showing component relationships (use ASCII art/boxes)
- Include data flow direction (arrows: →, ↓, ↔)
- Identify primary vs fallback paths (if applicable)
- Show external dependencies (databases, file systems, APIs, services)
- Label each layer (e.g., Frontend → API → Services → Data Sources)

### 1. Dependencies & Key Functions
- **External Dependencies**: Libraries, packages, external services (with versions if critical)
- **Internal Dependencies**: Modules this component imports from other parts of the project
- **Provided Functions**: Core functions/APIs this component exports for other parts
- **Configuration**: Environment variables, config files, feature flags required

### 2. User Experience Flow
- **End-User Perspective**: How functionality appears to the user (UI, CLI, API consumer)
- **Step-by-Step Walkthrough**: Typical use cases with concrete examples
- **Expected Inputs/Outputs**: Data formats, parameters, return values
- **State & Lifecycle**:
  - When is data created? (initialization, first use, triggers)
  - How long does it persist? (session, file-based, permanent)
  - What cleans it up? (garbage collection, manual deletion, TTL)
  - What happens on restart/failure?

### 3. File & Code Mapping
- **Key Files and Responsibilities**: List 5-10 most critical files with one-sentence descriptions
- **Entry Points**: Main execution paths (e.g., `app.py:main()`, `index.js`, CLI commands)
- **UX-to-Code Mapping**: For each major UX flow, list which files deliver that functionality
- **Directory Structure**: Brief overview of folder organization

### 4. Connections to Other Parts
- **Data Sources**: What writes data this component reads? What reads data this component writes?
- **Shared Resources**: Files, databases, environment variables, constants, global state
- **Event Flow**: Does this trigger other components? What triggers this component?
- **Fallback Mechanisms**: If this fails, what takes over? What does this replace on failure?
- **Side Effects**: What external state does this modify? (files, DB, network calls)

### 5. Critical Notes & Pitfalls
Organize by risk type:

- **Security**: Input validation, injection risks (SQL, XSS, path traversal), auth/authz
- **Performance**: Bottlenecks, scaling limits, optimization opportunities, caching strategies
- **Data Integrity**: Race conditions, stale data, consistency issues, transaction boundaries
- **Error Handling**: What errors are expected? How are they surfaced? Retry logic?
- **Known Edge Cases**: Specific gotchas, surprising behaviors, workarounds

### 6. Common Development Scenarios
Provide 3-5 specific examples with step-by-step instructions:

- "Adding a new field/metric/filter/feature"
- "Debugging when X doesn't appear/work as expected"
- "Extending functionality for Y use case"

For each scenario:
1. **What needs to change** (file-by-file modifications)
2. **Common mistakes to avoid**
3. **How to verify it works** (testing approach)

### 7. Testing Strategy
- **Manual Testing Checklist**: Critical user flows to verify manually
- **Automated Testing Opportunities**: What should be unit/integration tested (even if not implemented)
- **Smoke Tests**: Quick commands to confirm this part works
- **Debugging Tips**: Common issues and how to troubleshoot them

### 8. Quick Reference
- **Start/Run Commands**: How to launch this component locally
- **Key Endpoints/Interfaces**: API routes, CLI commands, UI URLs (if applicable)
- **Configuration Summary**: Essential env vars, flags, constants (with defaults)
- **Critical Files Checklist**: 5-10 most important files a developer should know
- **Common Constants**: Important thresholds, timeouts, limits (with rationale)

## Output Format

**File Location**: Save as `developer-guides/<component-name>-guide.md`

**Structure Guidelines**:
- Use h2 (##) for major sections (Architecture, Dependencies, etc.)
- Use h3 (###) for subsections
- Include code blocks with language tags (```python, ```javascript, ```sql)
- Add "Files involved: " or "File: " annotations after each major point
- Keep total length: 5-15KB (comprehensive but focused—prioritize actionable info)
- Use tables for comparisons, lists for sequences, code blocks for examples
- Include actual code snippets from the codebase where helpful (not pseudocode)

**Tone**: Direct, technical, assumption-free. Write for a developer encountering this component for the first time.
