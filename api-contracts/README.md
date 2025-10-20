# API Contracts

Detailed specifications of API endpoints - the contract between frontend and backend.

## Purpose

Documents exactly how APIs should behave. Backend implements these contracts, frontend consumes them. This is the agreement between both teams.

## What Goes Here

- Endpoint URL and HTTP method
- Request format (headers, body, query params)
- Response format for success and errors
- All possible error codes
- Rate limiting rules
- Authentication requirements
- Behavior requirements
- Implementation notes for both teams

## Organization

API contracts are organized by **domain** in subdirectories matching the features structure.

## File Naming

`{domain}/{service-name}-{action}.md`

Examples:
- `auth/auth-login.md`
- `auth/auth-logout.md`
- `dashboard/dashboard-data.md`
- `agents/agent-create.md`

## Template Structure

```markdown
# API Contract: [Endpoint Name]

**Endpoint:** `METHOD /path`
**Version:** 1.0
**Last Updated:** YYYY-MM-DD

## Purpose
## Request
## Response
## Error Responses
## Error Codes
## Behavior Requirements
## Frontend Implementation Notes
## Backend Implementation Notes
## Testing Checklist
```

## Example

See `auth/auth-login.md` for a complete example.

## Format

Use JSON for request/response examples. Include:
- All required fields marked clearly
- Field types and constraints
- Example values
- Optional fields

## Error Codes

Always document:
- HTTP status code
- Error code constant
- Human-readable message
- What frontend should do

## Tips

- Be exhaustive with error cases
- Include rate limiting details
- Specify exact field validation rules
- Add implementation notes for both teams
- Include testing checklist
- Use tables for error codes (easy to scan)
