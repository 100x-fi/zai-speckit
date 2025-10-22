# API Contract: Backoffice Login

**Endpoint:** `POST /bo/login`
**Version:** 1.0
**Last Updated:** 2025-01-20
**Status:** NEW - To be implemented

## Purpose

Authenticate Zai internal team (backoffice admins) and return a session token.

**Note:** This is separate from `/login` (for ClientAdmins).

## Request

### Headers

```
Content-Type: application/json
```

### Body

```json
{
  "email": "user@example.com",
  "password": "securepassword123",
  "rememberMe": false
}
```

**Field Requirements:**

| Field | Type | Required | Constraints |
|-------|------|----------|-------------|
| email | string | Yes | Valid email format, max 255 chars |
| password | string | Yes | Min 8 chars, max 128 chars |
| rememberMe | boolean | No | Default: false |

## Response

### Success (200 OK)

```json
{
  "success": true,
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": "user-123",
      "email": "user@example.com",
      "name": "John Doe"
    },
    "expiresAt": "2025-02-20T10:30:00Z"
  }
}
```

### Error Responses

#### 400 Bad Request - Invalid Input

```json
{
  "success": false,
  "error": {
    "code": "INVALID_INPUT",
    "message": "Please enter a valid email address",
    "field": "email"
  }
}
```

#### 401 Unauthorized - Wrong Credentials

```json
{
  "success": false,
  "error": {
    "code": "INVALID_CREDENTIALS",
    "message": "Invalid email or password"
  }
}
```

#### 403 Forbidden - Account Locked

```json
{
  "success": false,
  "error": {
    "code": "ACCOUNT_LOCKED",
    "message": "Account locked due to multiple failed login attempts. Try again in 15 minutes."
  }
}
```

#### 429 Too Many Requests - Rate Limited

```json
{
  "success": false,
  "error": {
    "code": "RATE_LIMITED",
    "message": "Too many login attempts. Try again in 12 minutes.",
    "retryAfter": 720
  }
}
```

#### 500 Internal Server Error

```json
{
  "success": false,
  "error": {
    "code": "INTERNAL_ERROR",
    "message": "An error occurred. Please try again later."
  }
}
```

## Error Codes

| Code | HTTP Status | Meaning | Frontend Action |
|------|-------------|---------|-----------------|
| INVALID_INPUT | 400 | Bad request format | Show field error |
| INVALID_CREDENTIALS | 401 | Wrong email/password | Show generic error |
| ACCOUNT_LOCKED | 403 | Too many failed attempts | Show lockout message |
| RATE_LIMITED | 429 | Too many requests | Show retry timer |
| INTERNAL_ERROR | 500 | Server error | Show generic error, allow retry |

## Behavior Requirements

### Rate Limiting

- Maximum 5 attempts per 15 minutes per IP address
- After 5 failed attempts, return 429 with retry time
- Counter resets after successful login or timeout

### Token Expiry

- Default token: 24 hours
- "Remember me" token: 30 days
- Backend must validate token expiry on protected routes

### Security

- Password must be transmitted over HTTPS only
- Response must not reveal whether email exists
- Log all failed attempts with IP and timestamp
- Token must be cryptographically secure (JWT with HS256 minimum)

## Frontend Implementation Notes

**For Frontend Team:**
- Call this endpoint when user submits login form
- Store token securely (HttpOnly cookie preferred, or secure storage)
- Include token in `Authorization: Bearer {token}` header for subsequent requests
- Handle all error codes with appropriate UI feedback
- Implement loading state while request is in flight
- Disable form during submission

## Backend Implementation Notes

**For Backend Team:**
- Validate email format before database lookup
- Use constant-time comparison for password check
- Return generic error for invalid credentials (don't reveal if email exists)
- Implement rate limiting per IP address
- Generate JWT with user ID and expiry
- Log failed login attempts for security monitoring

## Testing Checklist

- [ ] Valid login returns 200 with token
- [ ] Invalid email format returns 400
- [ ] Wrong password returns 401
- [ ] Non-existent email returns 401 (not 404)
- [ ] 6th attempt within 15 min returns 429
- [ ] Token is valid JWT with correct expiry
- [ ] Remember me extends token expiry
- [ ] Rate limit resets after 15 minutes
