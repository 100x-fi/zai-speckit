# Data Requirements: User Authentication

**Last Updated:** 2025-01-20

## User Data

### User Entity

Required data for authentication:

| Field | Type | Constraints | Purpose |
|-------|------|-------------|---------|
| id | string/uuid | Unique, indexed | User identifier |
| email | string | Unique, indexed, max 255 chars, valid email format | Login credential |
| passwordHash | string | Min 60 chars (bcrypt) | Secure password storage |
| name | string | Max 255 chars | Display name |
| status | enum | `active`, `locked`, `suspended` | Account status |
| createdAt | timestamp | ISO 8601 | Account creation date |
| updatedAt | timestamp | ISO 8601 | Last modification date |

**Note:** Password must never be stored in plain text. Only hashed version.

## Session/Token Data

### JWT Token Claims

Required claims in JWT:

| Claim | Type | Description |
|-------|------|-------------|
| sub | string | User ID |
| email | string | User email |
| iat | number | Issued at (Unix timestamp) |
| exp | number | Expiry (Unix timestamp) |
| type | string | `access` or `refresh` |

### Token Storage

**Frontend Requirements:**
- Must store token securely
- Prefer HttpOnly cookies (backend sets)
- Or secure storage (not localStorage for sensitive tokens)
- Must include token in Authorization header: `Bearer {token}`

**Backend Requirements:**
- Generate cryptographically secure tokens
- Sign with secret key
- Set appropriate expiry based on rememberMe flag
- Validate token on every protected endpoint

## Login Attempt Tracking

### Failed Login Attempt

For rate limiting and security monitoring:

| Field | Type | Purpose |
|-------|------|---------|
| ip | string | Client IP address |
| email | string | Attempted email |
| timestamp | timestamp | When attempt occurred |
| success | boolean | Whether login succeeded |
| errorCode | string | Error code if failed |

**Retention:** Keep for 30 days for security analysis.

## Data Flow

```
Frontend                    Backend                     Database
--------                    -------                     --------
User submits form
  |
  v
Send email + password  -->  Validate format
                            Query user by email  -->   Find user
                                                       Return passwordHash
                            Compare password hash
                            Check account status
                            Check rate limit
                            Generate JWT token
Return token + user   <--   Save login attempt   -->   Insert attempt record
  |
  v
Store token
Use for subsequent requests
```

## Validation Rules

### Email
- Must match regex: `^[^\s@]+@[^\s@]+\.[^\s@]+$`
- Case insensitive
- Trim whitespace
- Max length: 255 characters

### Password
- Min length: 8 characters
- Max length: 128 characters (before hashing)
- No other complexity requirements on frontend validation
- Backend enforces complexity during registration

### Remember Me
- Boolean value only
- Default: false if not provided

## Data Privacy

- Password never sent in plain text over non-HTTPS
- Password hash never returned to frontend
- Token must not contain sensitive information
- Email should be treated as PII

## Backend Data Storage Notes

**For Backend Team:**
- Use bcrypt with cost factor 12 for password hashing
- Index email field for fast lookup
- Index ip + timestamp for rate limiting queries
- Consider separate table for login attempts to keep user table lean
- Soft delete users (set status to deleted) rather than hard delete

## Frontend Data Handling Notes

**For Frontend Team:**
- Never log or display password
- Clear password field after submission
- Store only token and basic user info (id, email, name)
- Clear all auth data on logout
- Validate email format before submitting to reduce unnecessary API calls
