# Feature Index

Master list of all features in the speckit, organized by domain.

**Last Updated:** 2025-01-20

## How to Use This Index

- ✅ = Completed & implemented
- 🚧 = In progress
- 📋 = Spec written, not started
- 💡 = Planned, spec needed

## Authentication (`/auth`)

| Status | Feature | Priority | Files |
|--------|---------|----------|-------|
| 📋 | User Login | P0 | [Feature](features/auth/login.md) • [API](api-contracts/auth/auth-login.md) • [Data](data-requirements/auth/user-auth.md) • [Flow](user-flows/auth/login-flow.md) • [Tests](acceptance/auth/login-acceptance.md) |
| 💡 | User Logout | P0 | - |
| 💡 | Password Reset | P1 | - |
| 💡 | Two-Factor Auth | P2 | - |
| 💡 | Session Management | P1 | - |

## Dashboard (`/dashboard`)

| Status | Feature | Priority | Files |
|--------|---------|----------|-------|
| 💡 | Main Dashboard | P0 | - |
| 💡 | Overview Widgets | P1 | - |
| 💡 | Activity Feed | P1 | - |

## Agents (`/agents`)

| Status | Feature | Priority | Files |
|--------|---------|----------|-------|
| 💡 | Agent Creation | P0 | - |
| 💡 | Agent Configuration | P0 | - |
| 💡 | Agent List/Browse | P0 | - |
| 💡 | Agent Edit/Update | P0 | - |
| 💡 | Agent Delete | P1 | - |
| 💡 | Prompt Templates | P1 | - |
| 💡 | Model Selection | P0 | - |

---

## Priority Levels

- **P0 (Critical)**: Must have for MVP, blocks launch
- **P1 (High)**: Important for user experience, ship soon after MVP
- **P2 (Medium)**: Nice to have, can wait
- **P3 (Low)**: Future enhancement

## Adding New Features

When adding a new feature:

1. Add entry to this index with 💡 status
2. Create specs in appropriate domain folders
3. Update status to 📋 when spec is complete
4. Link all 5 spec files in the "Files" column
5. Update status to 🚧 when development starts
6. Update status to ✅ when feature is shipped

## Navigation

For complete domain organization, see:
- [Features](features/)
- [API Contracts](api-contracts/)
- [Data Requirements](data-requirements/)
- [User Flows](user-flows/)
- [Acceptance Tests](acceptance/)
