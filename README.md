# zai-speckit

Centralized **requirements and specifications** repository for the Zai platform. Pure requirements, user stories, and API contracts - no implementation code.

## Purpose

This speckit serves as the **single source of truth** for what to build, designed to be included as a Git submodule in both frontend and backend repositories.

**This is NOT implementation code** - it contains:
- Business requirements and user stories
- API contracts (what the API should do)
- Data requirements (what data is needed)
- User flows (how users interact)
- Acceptance criteria (how we know it's done)

## Structure

```
zai-speckit/
├── INDEX.md                     # Master list of all features
├── features/
│   ├── auth/                    # Authentication & authorization
│   ├── dashboard/               # Dashboard & overview
│   └── agents/                  # AI agent configuration
├── api-contracts/
│   ├── auth/
│   ├── dashboard/
│   └── agents/
├── data-requirements/
│   ├── auth/
│   ├── dashboard/
│   └── agents/
├── user-flows/
│   ├── auth/
│   ├── dashboard/
│   └── agents/
└── acceptance/
    ├── auth/
    ├── dashboard/
    └── agents/
```

**Organization:** Features are grouped by domain to keep the repo organized as it grows. Domains are specific to the AI backoffice configuration system.

## Usage as Submodule

### In Frontend Repository
```bash
git submodule add git@github.com:100x-fi/zai-speckit.git specs
```

Frontend developers:
- Read feature specs to understand what to build
- Reference API contracts to know how to call backend
- Follow user flows for UI/UX implementation
- Use acceptance criteria to verify completeness

### In Backend Repository
```bash
git submodule add git@github.com:100x-fi/zai-speckit.git specs
```

Backend developers:
- Read feature specs to understand requirements
- Implement API contracts as specified
- Ensure data requirements are met
- Use acceptance criteria for testing

### Updating Specs
```bash
cd specs
git pull origin main
cd ..
git add specs
git commit -m "Update specs to latest"
```

## Finding Features

**Start with [INDEX.md](INDEX.md)** - Master list of all features organized by domain with status tracking.

## For Product Managers: Review Order

When reviewing a feature specification, follow this optimal order for comprehensive understanding:

### 🎯 **Recommended Review Order:**

**1. Start with Business Requirements**
**`features/auth/login.md`** - The "what and why"
- User stories and business goals
- High-level requirements
- Scope and out-of-scope
- Priority and target release

**2. Understand the User Experience**
**`user-flows/auth/login-flow.md`** - The "how it feels"
- Step-by-step user journey
- UI/UX expectations
- Error scenarios and recovery
- Mobile and accessibility considerations

**3. Verify Technical Contract**
**`api-contracts/auth/auth-login.md`** - The "how it works"
- Request/response formats
- Error codes and handling
- Security requirements
- Integration points for both teams

**4. Define Data Requirements**
**`data-requirements/auth/user-auth.md`** - The "what data"
- Data models and schemas
- Storage requirements
- Entity relationships
- Data security considerations

**5. Review Testing Criteria**
**`acceptance/auth/login-acceptance.md`** - The "how we know it's done"
- Test scenarios and edge cases
- Success criteria
- Performance and security requirements
- Definition of done

### 📋 **Review Checklist per Document:**

**Features (login.md):**
- [ ] Business goals clear?
- [ ] User stories complete?
- [ ] Scope well-defined?

**User Flow (login-flow.md):**
- [ ] User journey logical?
- [ ] Error handling comprehensive?
- [ ] Accessibility addressed?

**API Contract (auth-login.md):**
- [ ] Request/response clear?
- [ ] Error codes complete?
- [ ] Security requirements sufficient?

**Data Requirements (user-auth.md):**
- [ ] Data models complete?
- [ ] Storage requirements clear?

**Acceptance Tests (login-acceptance.md):**
- [ ] Test coverage adequate?
- [ ] Success criteria measurable?
- [ ] Performance/security requirements met?

### 🔄 **Review Process:**

1. **Top-down approach**: Features → User Flow → API → Data → Acceptance
2. **Cross-reference consistency**: Do all documents align?
3. **Identify gaps**: What's missing or unclear?
4. **Team readiness**: Can both frontend and backend implement from these specs?
5. **Risk assessment**: What could go wrong based on current specs?

This approach ensures you understand **business value first**, then **how users experience it**, then **how it works technically**, and finally **how to verify it's working correctly**.

## Example: Login Feature ✅ COMPLETED

**Status**: ✅ **Fully Implemented & Aligned** (2025-01-22)

See the complete login feature example in the `auth` domain:
- **Feature spec**: [`features/auth/login.md`](features/auth/login.md) - User stories and requirements
- **API contract**: [`api-contracts/auth/auth-login.md`](api-contracts/auth/auth-login.md) - Endpoint behavior
- **Data requirements**: [`data-requirements/auth/user-auth.md`](data-requirements/auth/user-auth.md) - Data needed
- **User flow**: [`user-flows/auth/login-flow.md`](user-flows/auth/login-flow.md) - How it works
- **Acceptance tests**: [`acceptance/auth/login-acceptance.md`](acceptance/auth/login-acceptance.md) - How to verify

### Implementation Summary
**Frontend (zai-bo-frontend)**: ✅ Complete
- React 19.1.1 + TypeScript + TanStack Router
- AuthContext for state management (single source of truth)
- localStorage for 30-day sessions
- Dark mode UI with Tailwind CSS
- Full login → dashboard flow with protected routes

**Backend (zai-backend)**: 🔄 Ready for Implementation
- Endpoint: `POST /bo/login`
- Response format: `{status: 'success', data: {token, user, expiresAt}}`
- JWT tokens with 30-day expiry
- Security: HTTPS, rate limiting, password hashing

### Key Architecture Decisions
- **Simplified from remember me** → Always 30-day localStorage sessions
- **Single source of truth** → AuthContext manages all auth state
- **API format alignment** → Backend returns `{status: 'success'}`, frontend normalizes to `{success: true}`
- **Team collaboration** → Both teams reference identical specifications

## Folder Details

### `/features`
High-level feature specifications with user stories, requirements for both frontend and backend, and out-of-scope items.

### `/api-contracts`
Detailed API requirements: endpoints, request/response formats, error codes, behavior expectations. Both teams implement to this contract.

### `/data-requirements`
What data is needed, validation rules, data flow. Backend implements storage, frontend implements display/input.

### `/user-flows`
Step-by-step user journeys, UI states, navigation. Primarily for frontend but backend needs to understand the flow.

### `/acceptance`
Test scenarios in Given/When/Then format. Both teams use these to verify implementation is complete and correct.

## Contributing

### Adding a New Feature

1. **Add to INDEX.md** with 💡 status and choose appropriate domain
2. Create feature spec in `/features/{domain}/{feature-name}.md`
3. Create API contract in `/api-contracts/{domain}/{endpoint-name}.md`
4. Document data requirements in `/data-requirements/{domain}/{entity-name}.md`
5. Map user flow in `/user-flows/{domain}/{feature-flow}.md`
6. Write acceptance tests in `/acceptance/{domain}/{feature-name}-acceptance.md`
7. **Update INDEX.md** with links to all files and change status to 📋

See `auth/login` example as template.

### Adding a New Domain

If none of the existing domains fit:
1. Create new domain folder in all 5 directories
2. Add domain section in INDEX.md
3. Update this README with the new domain

### Updating Existing Specs

1. Update the relevant files
2. Add "Last Updated" date
3. Notify both frontend and backend teams
4. Update both repositories' submodules

## Principles

- **Requirements, not implementation** - Describe what, not how
- **Single source of truth** - One place for all specs
- **Team-agnostic** - Both frontend and backend reference the same specs
- **Clarity over completeness** - Clear and actionable beats exhaustive
- **Keep it current** - Outdated specs are worse than no specs
- **Examples included** - Show, don't just tell

## Links

- [Constitution](.claude/constitution.md) - Development standards
- [GitHub Repository](https://github.com/100x-fi/zai-speckit)
