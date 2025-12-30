---
description: Generate or update an E2E testing plan for a feature or application part
allowed-tools: Read, Grep, Glob, Write, Edit
argument-hint: "<feature-or-component-name>"
category: workflow
---

Generate a comprehensive E2E (End-to-End) testing plan for: `<ARGS>`

## Investigation Phase

Before writing the test plan, systematically analyze:

1. **Identify Components**: Use Glob to find relevant files (API routes, UI components, database queries)
2. **Trace Data Flow**: Map user action → frontend → API → database → response
3. **Find Endpoints**: Grep for route definitions, API handlers, HTTP methods
4. **Identify Dependencies**: Check what external services, databases, or APIs are involved
5. **Review Existing Tests**: Check if similar test plans exist in `e2e-testing-plans/`

## Test Plan Structure

The test plan MUST follow this exact structure and be saved as `e2e-testing-plans/<feature-name>-e2e-plan.md`:

### File Structure Template

```markdown
# <Feature Name> E2E Testing Plan

## Overview
[2-3 sentence description of what this feature does and why testing it matters]

## Test Environment
- **Application URL**: [e.g., http://localhost:3006]
- **API Base URL**: [e.g., http://localhost:3006/api]
- **Database**: [e.g., PostgreSQL via DATABASE_URL]
- **Browser**: [e.g., Latest Chrome/Firefox]
- **Prerequisites**: [e.g., Scraper must have run at least once]

---

## Test Plan Structure

### Phase 1: API Endpoint Validation
Verify all backend endpoints return correct data structure and values.

### Phase 2: UI Component Validation
Verify frontend correctly displays data from API responses.

### Phase 3: User Flow Validation
Test complete user workflows and interactions.

### Phase 4: Error Handling
Verify graceful degradation and error states.

---

## Phase 1: API Endpoint Validation

### Test 1.1: [HTTP METHOD] /api/[endpoint]
**Purpose**: [What this test verifies]

**Request**:
\`\`\`bash
curl -s http://localhost:XXXX/api/endpoint | jq .
\`\`\`

**Expected Response Structure**:
\`\`\`json
{
  "field_name": "expected_type_or_value",
  "another_field": 123
}
\`\`\`

**Validation Checklist**:
- [ ] Response contains `field_name` (type: string/number/array)
- [ ] Field validates against expected format/range
- [ ] [Add 5-10 specific validation items]

**Failure Investigation**:
If validation fails:
1. Check [specific file:line] for query logic
2. Verify database table schema: `SELECT * FROM table_name LIMIT 1;`
3. Inspect actual values: [provide diagnostic SQL/command]
4. Check field mapping between [source] and [destination]

---

[Repeat for each API endpoint - typically 3-6 endpoints per feature]

---

## Phase 2: UI Component Validation

### Test 2.1: [Component Name] Display
**Purpose**: [What UI element this verifies]

**Steps**:
1. Navigate to [URL]
2. Wait for [specific element/data to load]
3. Inspect [component name]

**Validation Checklist**:
- [ ] Component displays data from `/api/[endpoint]`
- [ ] [Specific field] shows formatted value (not empty/null)
- [ ] [Interactive element] responds to user action
- [ ] [Add 5-8 specific UI validation items]

**Debugging Steps**:
1. Open browser DevTools → Network tab
2. Inspect `/api/[endpoint]` request response
3. Open Console tab → check for JavaScript errors
4. Inspect React/Vue component state (if applicable)
5. Check rendering logic in [specific file]

---

[Repeat for each major UI component - typically 3-5 components]

---

## Phase 3: User Flow Validation

### Test 3.1: [User Action Flow]
**Purpose**: [End-to-end user scenario]

**Steps**:
1. [Action 1: e.g., Click button X]
2. [Action 2: e.g., Fill form field Y]
3. [Action 3: e.g., Submit and verify response]

**Validation Checklist**:
- [ ] [Expected outcome 1]
- [ ] [Expected outcome 2]
- [ ] [State persistence/update verification]
- [ ] [Navigation/routing works correctly]

---

[Repeat for 2-4 critical user flows]

---

## Phase 4: Error Handling

### Test 4.1: [Error Scenario]
**Purpose**: [What failure condition this tests]

**Steps**:
1. [How to trigger error: e.g., Stop backend server]
2. [Action to observe: e.g., Reload page]
3. Observe behavior

**Validation Checklist**:
- [ ] Shows user-friendly error message (not blank page)
- [ ] Provides recovery action (retry button, navigation)
- [ ] No JavaScript console errors
- [ ] [Specific error handling behavior]

---

[Repeat for 3-5 error scenarios: API unavailable, empty state, invalid input, network timeout, etc.]

---

## Known Issues to Fix

### Issue 1: [Descriptive Issue Name]
**Symptoms**: [What the user sees when this breaks]

**Root Cause Investigation**:
1. [Diagnostic step 1]
2. [Diagnostic step 2]
3. [How to confirm root cause]

**Diagnostic Commands**:
\`\`\`bash
# [Command 1 with explanation]
# [Command 2 with explanation]
\`\`\`

**Potential Fixes**:
- **If [condition A]**: [Fix approach A with file:line reference]
- **If [condition B]**: [Fix approach B with file:line reference]
- **If [condition C]**: [Fix approach C]

---

[Document 2-5 known issues or common failure points]

---

## Success Criteria

This feature is **production-ready** when:
- ✅ All Phase 1 API tests pass (100%)
- ✅ All Phase 2 UI tests pass (100%)
- ✅ All Phase 3 user flows work correctly
- ✅ All Phase 4 error states handled gracefully
- ✅ No console errors during normal operation
- ✅ [Feature-specific criterion 1]
- ✅ [Feature-specific criterion 2]

---

## Test Execution Log

### Run 1: [YYYY-MM-DD]
**Tester**: [Name or "Automated"]
**Results**:
- Phase 1: X/Y passed
- Phase 2: X/Y passed
- Phase 3: X/Y passed
- Phase 4: X/Y passed

**Issues Found**:
1. [Issue description]
2. [Issue description]

**Action Items**:
1. [Fix needed]
2. [Fix needed]

---

[Add entry for each test execution run]

---

## Appendix: Manual Testing Checklist

Quick visual inspection checklist:

- [ ] [Feature] loads without errors
- [ ] [Component A] displays correctly
- [ ] [Component B] shows expected data
- [ ] [Action X] triggers [expected result]
- [ ] [Error state Y] shows graceful message
- [ ] [Add 10-15 quick visual checks]
- [ ] Mobile responsive (if applicable)
- [ ] Cross-browser compatibility (Chrome, Firefox, Safari)
```

## Length & Detail Guidelines

- **Total Length**: 8-20 KB (approximately 300-700 lines)
- **API Tests**: 1 detailed test per endpoint (5-15 validation items each)
- **UI Tests**: 1 test per major component (5-10 validation items each)
- **User Flows**: 2-4 complete end-to-end scenarios
- **Error Tests**: 3-5 failure conditions with recovery verification
- **Known Issues**: Document 2-5 common problems with diagnostic steps

## Writing Guidelines

1. **Be Specific**: Include exact curl commands, file paths with line numbers, SQL queries
2. **Provide Diagnostics**: Every test should have "Failure Investigation" or "Debugging Steps"
3. **Use Checklists**: Break validation into checkbox items (easy to track progress)
4. **Include Examples**: Show actual request/response samples, not just schemas
5. **Reference Code**: Link to specific files and line numbers for debugging
6. **Actionable Fixes**: Don't just identify issues—provide concrete fix approaches

## Output Location

Save the test plan as: `e2e-testing-plans/<feature-name>-e2e-plan.md`

If updating an existing plan:
- Preserve existing test execution logs
- Update sections that changed
- Add new tests for new functionality
- Mark deprecated tests clearly

## Example Feature Names

- `dashboard-validation-plan.md` (already exists)
- `scraper-execution-plan.md`
- `etl-pipeline-plan.md`
- `api-authentication-plan.md`
- `data-export-plan.md`
