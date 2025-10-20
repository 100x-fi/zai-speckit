# User Flows

Step-by-step user journeys and interaction flows.

## Purpose

Maps how users interact with the system from start to finish. Primarily for frontend to implement UI/UX, but backend needs to understand the flow to support it properly.

## What Goes Here

- Primary happy path flow
- Alternative/error flows
- UI states (loading, error, success, empty)
- Navigation between pages
- User actions and system responses
- Mobile considerations
- Accessibility requirements
- Performance requirements

## Organization

User flows are organized by **domain** in subdirectories matching the features structure.

## File Naming

`{domain}/{feature-name}-flow.md`

Examples:
- `auth/login-flow.md`
- `dashboard/main-dashboard-flow.md`
- `agents/agent-creation-flow.md`

## Template Structure

```markdown
# User Flow: [Feature Name]

**Last Updated:** YYYY-MM-DD

## Primary Flow: [Success Case]

Step-by-step with actions and responses

## Alternative Flow: [Error/Edge Case]

What happens when things go wrong

## UI States

Different states the UI can be in

## Navigation

Entry and exit points

## Mobile Considerations

Mobile-specific requirements

## Accessibility

A11y requirements

## Performance

Performance expectations
```

## Example

See `auth/login-flow.md` for a complete example.

## Format

Write flows as numbered steps:

```
1. User does X
   └─> System responds with Y
       - Detail 1
       - Detail 2

2. User sees Z
   └─> Can take action A or B
```

## Tips

- Cover happy path first, then alternatives
- Include all UI states (loading, error, empty, success)
- Specify what user sees at each step
- Document error flows as thoroughly as success
- Include mobile/accessibility notes
- Add performance expectations
- Focus on user actions, not technical implementation
