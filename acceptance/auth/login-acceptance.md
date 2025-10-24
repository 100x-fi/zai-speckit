# Acceptance Tests: Login Feature

**Feature:** User Login
**Last Updated:** 2025-01-20

## Test Scenarios

### Scenario 1: Successful Login

**Given** a registered user with:
- Email: `test@example.com`
- Password: `SecurePass123`
- Account status: active

**When** user enters correct credentials and submits

**Then:**
- [ ] API returns HTTP 200
- [ ] Response format: `{status: 'success', data: {...}}`
- [ ] Response contains valid JWT token
- [ ] Response contains user data (id, email, name)
- [ ] Frontend stores token in localStorage (30-day session)
- [ ] Frontend stores user data in localStorage
- [ ] AuthContext state updated (single source of truth)
- [ ] Frontend redirects to `/dashboard` using TanStack Router
- [ ] User can access protected routes with token

---

### Scenario 2: Invalid Email Format

**Given** user is on login page

**When** user enters email: `notanemail` and clicks login

**Then:**
- [ ] Frontend validation catches error before API call
- [ ] Error message shown: "Please enter a valid email address" (red text)
- [ ] Error message appears directly under email field
- [ ] No API request is made
- [ ] Form remains enabled
- [ ] Email field is highlighted/focused

---

### Scenario 3: Wrong Password

**Given** a registered user with email: `test@example.com`

**When** user enters wrong password: `WrongPassword`

**Then:**
- [ ] API returns HTTP 401
- [ ] Error code: `INVALID_CREDENTIALS`
- [ ] Frontend shows: "Invalid email or password" (red text)
- [ ] Error message appears directly under password field
- [ ] Email field retains entered value for convenience
- [ ] Password field is completely cleared (no masked characters)
- [ ] Form is immediately re-enabled for retry
- [ ] Password input field receives keyboard focus for retry
- [ ] Error message styling is prominent but not alarming
- [ ] User can retry without page refresh or navigation
- [ ] Generic error message (doesn't reveal if email exists)

---

### Scenario 4: Non-existent Email

**Given** email `nonexistent@example.com` is not in system

**When** user attempts to login with this email

**Then:**
- [ ] API returns HTTP 401 (NOT 404)
- [ ] Same error as wrong password: "Invalid email or password"
- [ ] Backend does not reveal whether email exists
- [ ] Failed attempt is logged for security monitoring

---

### Scenario 5: Account Locked

**Given** a user account with status: `locked`

**When** user attempts to login with correct credentials

**Then:**
- [ ] API returns HTTP 403
- [ ] Error code: `ACCOUNT_LOCKED`
- [ ] Frontend shows: "Account locked. Try again in 15 minutes."
- [ ] Form is re-enabled
- [ ] User cannot proceed until unlocked

---

### Scenario 6: Rate Limiting After 5 Attempts

**Given** user has made 5 failed login attempts in last 15 minutes

**When** user attempts 6th login

**Then:**
- [ ] API returns HTTP 429
- [ ] Error code: `RATE_LIMITED`
- [ ] Response includes `retryAfter` in seconds
- [ ] Frontend shows: "Too many attempts. Try again in X minutes."
- [ ] Frontend displays countdown timer
- [ ] Form is disabled until timer expires

---

### Scenario 7: Token Storage and Expiry

**Given** user logs in successfully

**Then:**
- [ ] Token expiry is set to 30 days
- [ ] User remains logged in after browser restart
- [ ] Token is valid for 30 days
- [ ] User can access platform without re-login
- [ ] Token is stored in localStorage

---

### Scenario 9: Server Error

**Given** backend service is experiencing errors

**When** user attempts to login

**Then:**
- [ ] API returns HTTP 500
- [ ] Error code: `INTERNAL_ERROR`
- [ ] Frontend shows: "An error occurred. Please try again."
- [ ] Error details logged for debugging (not shown to user)
- [ ] Form is re-enabled
- [ ] User can retry immediately

---

### Scenario 10: Network Timeout

**Given** network connection is slow/unstable

**When** login request times out (>30 seconds)

**Then:**
- [ ] Frontend shows timeout error
- [ ] Form is re-enabled
- [ ] User can retry
- [ ] Loading state is cleared

---

### Scenario 11: Already Logged In

**Given** user is already authenticated with valid token

**When** user navigates to `/login` page

**Then:**
- [ ] Frontend detects existing valid session
- [ ] User is automatically redirected to `/dashboard`
- [ ] Login form is not shown

---

### Scenario 12: Token Included in Subsequent Requests

**Given** user has logged in successfully

**When** user makes requests to protected endpoints

**Then:**
- [ ] Token is included in `Authorization: Bearer {token}` header
- [ ] Backend validates token
- [ ] Protected resources are accessible
- [ ] Requests without token are rejected (401)

---

## Error Message Acceptance Criteria

### Error Message Positioning

- [ ] All error messages appear directly under relevant input field
- [ ] Email validation errors appear under email field
- [ ] Backend errors (invalid credentials, locked, rate limited) appear under password field
- [ ] Network errors appear under password field
- [ ] Error messages use red text color (dark mode compatible)
- [ ] Error text size is appropriate (smaller than labels, larger than placeholders)
- [ ] Error messages are prominent but not overly alarming
- [ ] Error messages clear when user starts typing again
- [ ] Error messages reappear if same error occurs again
- [ ] Multiple errors stack properly (if applicable)

### Error Message Styling
- [ ] Red text color for all error messages
- [ ] Consistent positioning regardless of error type
- [ ] Error text is readable in dark mode
- [ ] Error messages don't break form layout
- [ ] Error messages have proper spacing from input fields

## UI/UX Acceptance Criteria

### Visual Requirements

- [ ] Page uses dark mode theme
- [ ] Form is centered on page
- [ ] Inputs have clear labels
- [ ] Password field masks characters by default
- [ ] Password field has visibility toggle (eye icon)
- [ ] Eye icon shows "eye" when masked, "eye-slash" when visible
- [ ] Button shows loading spinner during request
- [ ] Error messages are red and clearly visible
- [ ] Form is responsive on mobile (320px - 1920px)

### Interaction Requirements

- [ ] Email field autofocus on page load
- [ ] Tab key moves between fields correctly (including password toggle)
- [ ] Enter key submits form
- [ ] Space or Enter key toggles password visibility
- [ ] Form cannot be submitted while loading
- [ ] Error messages appear near relevant fields
- [ ] Success transition is smooth (no jarring redirect)
- [ ] Password visibility toggle is keyboard accessible
- [ ] Password visibility state is announced by screen readers

### Accessibility Requirements

- [ ] All form fields have proper labels
- [ ] Error messages announced by screen readers
- [ ] Keyboard navigation works completely
- [ ] Color contrast meets WCAG 2.1 AA standards
- [ ] Focus indicators are visible

## Performance Requirements

- [ ] Login page loads in < 1 second
- [ ] API response time p95 < 500ms
- [ ] Form validation instant (< 50ms)
- [ ] No UI blocking during API call

## Security Requirements

- [ ] All requests use HTTPS
- [ ] Password never visible in network logs
- [ ] Token stored in localStorage (30-day session)
- [ ] Failed attempts are rate limited
- [ ] No sensitive data in error messages
- [ ] CSRF protection enabled

## Cross-Browser Testing

- [ ] Chrome (latest)
- [ ] Firefox (latest)
- [ ] Safari (latest)
- [ ] Edge (latest)
- [ ] Mobile Safari (iOS)
- [ ] Mobile Chrome (Android)

## Test Data

### Valid Test User
```
Email: test@example.com
Password: SecurePass123
Status: active
```

### Locked Test User
```
Email: locked@example.com
Password: SecurePass123
Status: locked
```

### Invalid Credentials
```
Email: test@example.com
Password: WrongPassword
```

### Invalid Email Formats
```
- notanemail
- @example.com
- user@
- user @example.com
- user@example
```

## Technical Implementation Details

### Frontend Architecture (zai-bo-frontend)
- **Framework**: React 19.1.1 + TypeScript
- **Routing**: TanStack Router v1.133.20 (file-based routing)
- **State Management**: React Context + TanStack Query
- **Styling**: Tailwind CSS (dark mode)
- **Build Tool**: Vite 7.1.11 + Bun
- **Authentication Flow**:
  - Login component calls API
  - AuthContext manages single source of truth
  - localStorage for persistence (30-day session)
  - Protected routes redirect to `/dashboard`

### Backend Integration Requirements (zai-backend)
- **Endpoint**: `POST /bo/login`
- **Response Format**: `{status: 'success', data: {token, user, expiresAt}}`
- **Token**: JWT with 30-day expiry
- **Security**: HTTPS, password hashing, rate limiting

## Definition of Done

Feature is complete when:
- [ ] All acceptance scenarios pass
- [ ] UI/UX criteria met
- [ ] Performance requirements met
- [ ] Security requirements met
- [ ] Cross-browser testing complete
- [ ] Both frontend and backend teams sign off
- [ ] Documentation updated (current specs)
- [ ] Frontend implementation verified against backend API
