# Acceptance Tests

Test scenarios and acceptance criteria in Given/When/Then format.

## Purpose

Defines how to verify the feature is complete and working correctly. Both frontend and backend teams use these to write tests and verify implementation.

## What Goes Here

- Test scenarios in Given/When/Then format
- Expected behavior for each scenario
- UI/UX acceptance criteria
- Performance requirements
- Security requirements
- Cross-browser testing requirements
- Test data
- Definition of done checklist

## Organization

Acceptance tests are organized by **domain** in subdirectories matching the features structure.

## File Naming

`{domain}/{feature-name}-acceptance.md`

Examples:
- `auth/login-acceptance.md`
- `dashboard/main-dashboard-acceptance.md`
- `agents/agent-creation-acceptance.md`

## Template Structure

```markdown
# Acceptance Tests: [Feature Name]

**Feature:** [Name]
**Last Updated:** YYYY-MM-DD

## Test Scenarios

### Scenario N: [Name]

**Given** [initial context]
**When** [action]
**Then:**
- [ ] Expected outcome 1
- [ ] Expected outcome 2

## UI/UX Acceptance Criteria
## Performance Requirements
## Security Requirements
## Test Data
## Definition of Done
```

## Example

See `auth/login-acceptance.md` for a complete example.

## Format: Given/When/Then

```
**Given** user is on login page
**When** user enters valid credentials
**Then:**
- [ ] API returns 200
- [ ] Token is stored
- [ ] User redirected to dashboard
```

## Tips

- One scenario per test case
- Be specific and testable
- Include both frontend and backend checks
- Cover success and failure cases
- Add UI/UX criteria separate from functional tests
- Include performance benchmarks
- Provide test data examples
- Checkboxes make it easy to track completion
- Definition of done = when feature is shippable
