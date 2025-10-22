# Feature: Backoffice Admin Login

**Status:** Draft
**Priority:** P0 (Critical)
**Target Release:** v1.0
**Last Updated:** 2025-01-20

## Overview

Zai internal team needs to authenticate to access the backoffice platform. Login should be simple, secure, and work seamlessly between zai-bo-frontend and zai-backend.

## User Stories

### US-001: Login with Email and Password

**As a** backoffice admin
**I want to** log in with my email and password
**So that** I can access the backoffice and manage the platform

**Acceptance Criteria:**
- User can enter email and password
- System validates credentials
- On success: User is redirected to dashboard with valid 30-day session
- On failure: Clear error message is shown
- Session persists across browser refreshes

### US-002: Error Handling

**As a** user
**I want to** see clear error messages when login fails
**So that** I know what went wrong and how to fix it

**Acceptance Criteria:**
- Invalid email format: "Please enter a valid email address"
- Wrong credentials: "Invalid email or password"
- Server error: "Unable to connect. Please try again"

## Requirements

### Frontend Requirements

- Clean, minimal dark mode UI
- Form with email and password fields
- Submit button (disabled during loading)
- Error message display area
- Loading state during API call
- Mobile responsive
- No external UI libraries (Tailwind only)

### Backend Requirements

- POST /bo/login endpoint to authenticate backoffice admins
- Validate email format and password
- Return JWT token on success (30-day expiry)
- Return appropriate error codes on failure
- Log failed login attempts for security monitoring

### Security Requirements

- Password must not be visible in network logs
- Use HTTPS only
- JWT tokens must be HttpOnly cookies or secure storage
- Implement CSRF protection
- Hash passwords with bcrypt (backend)
- No credential storage in localStorage

## Out of Scope

- OAuth/Social login (future)
- 2FA (future)
- Password recovery (separate feature)
- Sign up (separate feature)

## Dependencies

- Backend API must be deployed
- Database with backoffice_admins table must exist

## References

- API Contract: `/api-contracts/auth-login.md`
- Data Requirements: `/data-requirements/user-auth.md`
- User Flow: `/user-flows/login-flow.md`
- Acceptance Tests: `/acceptance/login-acceptance.md`
