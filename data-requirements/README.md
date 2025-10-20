# Data Requirements

What data is needed, how it flows, and validation rules.

## Purpose

Documents data requirements for features. Backend uses this to design database schema and APIs. Frontend uses this to understand what data they'll receive and send.

## What Goes Here

- Data fields and types
- Validation rules
- Constraints (min/max length, format, etc.)
- Data flow diagrams
- Relationships between entities
- Privacy/security notes
- Storage requirements (backend)
- Display requirements (frontend)

## Organization

Data requirements are organized by **domain** in subdirectories matching the features structure.

## File Naming

`{domain}/{entity-name}.md`

Examples:
- `auth/user-auth.md`
- `dashboard/dashboard.md`
- `agents/agent.md`

## Template Structure

```markdown
# Data Requirements: [Entity Name]

**Last Updated:** YYYY-MM-DD

## [Entity] Data

Table of fields, types, constraints, and purpose

## Related Data

Other entities referenced

## Data Flow

How data moves through the system

## Validation Rules

Field-specific validation

## Data Privacy

Security and privacy considerations

## Backend Notes

Storage implementation guidance

## Frontend Notes

Display/input guidance
```

## Example

See `auth/user-auth.md` for a complete example.

## Format

Use tables for data fields:

| Field | Type | Constraints | Purpose |
|-------|------|-------------|---------|
| id | string | Unique, indexed | Identifier |

## Tips

- Focus on **what** data is needed, not **how** to store it
- Include validation rules that both teams need
- Document privacy requirements
- Show data flow between frontend and backend
- Specify which fields are required vs optional
- Include format requirements (dates, emails, etc.)
