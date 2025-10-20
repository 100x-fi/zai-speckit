# Features

High-level feature specifications with user stories and requirements.

## Purpose

Each feature file describes **what** needs to be built from a business/user perspective. Both frontend and backend teams read these to understand the complete feature.

## Organization

Features are organized by **domain** in subdirectories:
- `auth/` - Authentication & authorization
- `dashboard/` - Dashboard & overview
- `agents/` - AI agent configuration

## What Goes Here

- User stories (As a... I want... So that...)
- Business requirements
- Frontend requirements
- Backend requirements
- Security requirements
- Out of scope items
- Dependencies
- References to other spec files

## File Naming

`{domain}/{feature-name}.md`

Examples:
- `auth/login.md`
- `dashboard/main-dashboard.md`
- `agents/agent-creation.md`

## Template Structure

```markdown
# Feature: [Name]

**Status:** Draft | In Progress | Completed
**Priority:** P0 (Critical) | P1 (High) | P2 (Medium) | P3 (Low)
**Target Release:** v1.0
**Last Updated:** YYYY-MM-DD

## Overview
Brief description of the feature

## User Stories
### US-XXX: [Title]
**As a** [role]
**I want to** [action]
**So that** [benefit]

**Acceptance Criteria:**
- [ ] Criterion 1
- [ ] Criterion 2

## Requirements
### Frontend Requirements
### Backend Requirements
### Security Requirements

## Out of Scope
## Dependencies
## References
```

## Example

See `auth/login.md` for a complete example.

## Tips

- Write user-centric, not tech-centric
- Be specific about acceptance criteria
- Call out both frontend and backend needs
- Link to detailed specs in other folders
- Keep it concise - details go in other folders
