# User Flow: Login

**Last Updated:** 2025-01-20

## Primary Flow: Successful Login

```
1. User navigates to login page
   └─> Page displays login form (dark mode)
       - Email input field
       - Password input field (masked)
       - Password visibility toggle (eye icon)
       - "Login" button

2. User enters email and password
   └─> Frontend validates format
       - Email must be valid format
       - Password must be non-empty

3. User clicks "Login" button
   └─> Frontend actions:
       - Disable form
       - Show loading state on button
       - Call POST /bo/login

4. Backend processes request
   └─> Validates credentials
       - Looks up user by email
       - Compares password hash
       - Checks account status
       - Checks rate limits

5. Backend returns success (200)
   └─> Response contains:
       - JWT token
       - User data (id, email, name)
       - Token expiry
       - Response format: `{status: 'success', data: {...}}`

6. Frontend receives success
   └─> Actions:
       - Store token in localStorage (30-day session)
       - Store user data in localStorage
       - Update AuthContext state (single source of truth)
       - Redirect to dashboard (/dashboard)

7. User sees dashboard
   └─> Now authenticated and can use platform
```

## Alternative Flow: Wrong Credentials

```
1-3. [Same as primary flow]

4. Backend checks credentials
   └─> Password doesn't match OR email not found

5. Backend returns 401 error
   └─> Error code: INVALID_CREDENTIALS
       Message: "Invalid email or password"

6. Frontend receives error
   └─> Actions:
       - Re-enable form immediately
       - Show error message below form (red text)
       - Keep email field filled with entered value
       - Clear password field completely
       - Set cursor focus to password input field
       - Allow unlimited retry attempts (rate limiting handled separately)

7. User sees error message
   └─> Can retry immediately with correct credentials
```

## Alternative Flow: Account Locked

```
1-3. [Same as primary flow]

4. Backend checks account status
   └─> Account status is "locked"

5. Backend returns 403 error
   └─> Error code: ACCOUNT_LOCKED
       Message: "Account locked. Try again in 15 minutes."

6. Frontend receives error
   └─> Actions:
       - Re-enable form
       - Show error message prominently
       - Display retry time if available

7. User sees lockout message
   └─> Must wait before trying again
```

## Alternative Flow: Rate Limited

```
1-3. [Same as primary flow]

4. Backend checks rate limit
   └─> User has exceeded 5 attempts in 15 minutes

5. Backend returns 429 error
   └─> Error code: RATE_LIMITED
       Message: "Too many attempts. Try again in X minutes."
       retryAfter: seconds until can retry

6. Frontend receives error
   └─> Actions:
       - Disable form completely
       - Show countdown timer
       - Explain rate limit reason

7. User sees rate limit message
   └─> Must wait for timer to expire
```

## Alternative Flow: Validation Error

```
1. User navigates to login page

2. User enters invalid email format
   └─> Example: "notanemail"

3. User clicks "Login" button
   └─> Frontend validation catches error
       - Shows inline error: "Please enter a valid email"
       - Does NOT call backend API
       - Form stays enabled
       - Focus stays on email field

4. User corrects email format
   └─> Error message disappears
       Can proceed with login
```

## Alternative Flow: Server Error

```
1-3. [Same as primary flow]

4. Backend encounters error
   └─> Database connection fails OR
       Internal server error

5. Backend returns 500 error
   └─> Error code: INTERNAL_ERROR
       Message: "An error occurred. Please try again."

6. Frontend receives error
   └─> Actions:
       - Re-enable form
       - Show generic error message
       - Log error details (for debugging)
       - Provide "Try Again" option

7. User sees error message
   └─> Can retry immediately
```

## UI States

### Initial State
- Form enabled
- No errors shown
- Button enabled
- Fields empty

### Loading State
- Form disabled (grayed out)
- Button shows loading spinner
- "Login" text changes to "Logging in..."
- User cannot submit again

### Error State
- Form re-enabled
- Error message visible (red text)
- Password field cleared
- Email field retains value
- Focus on password field

### Success State
- Brief success indicator (optional)
- Immediate redirect to dashboard
- No lingering on login page

## Navigation

**Entry Points:**
- Direct URL: `/login`
- Redirect from protected pages
- "Login" link from homepage
- After logout

**Exit Points:**
- Success: Redirect to `/dashboard` using TanStack Router
- Already logged in: Auto-redirect to dashboard (protected route)
- Error: Show specific error messages, form re-enabled
- Network Error: Show "Unable to connect. Please try again."

## Password Visibility Toggle

**When user clicks eye icon:**
- Toggle password field between masked and visible text
- Show "eye" icon when password is masked
- Show "eye-slash" icon when password is visible
- Maintain cursor position in password field
- Preserve password value during toggle
- Allow immediate visibility testing without affecting form submission

**Security Requirements:**
- Default state: Password must be masked
- Clear visibility after form submission or timeout
- Never store password visibility preference for security
- Prevent password value visibility in page source code
- Use secure input methods even when visible

## Mobile Considerations

- All form fields must be easily tappable (min 44px height)
- Password field visibility toggle must be easily tappable
- Keyboard should show email type for email field
- Password field should show secure keyboard
- Error messages should be visible above keyboard
- Form should not be obscured by keyboard
- Eye icon must have minimum 44x44px tap area

## Accessibility

- All form fields must have labels
- Error messages must be announced by screen readers
- Focus management: error -> focus appropriate field
- Keyboard navigation: Tab through fields, including password visibility toggle
- Button must be keyboard accessible (Enter to submit)
- Password visibility toggle must be keyboard accessible (Space to toggle, Enter to confirm)
- Eye icon must have descriptive alt text ("Show password"/"Hide password")
- Toggle state must be announced by screen readers
- Sufficient color contrast in dark mode

## Performance

- Page should load in < 1 second
- API call should complete in < 500ms (p95)
- Form validation should be instant (< 50ms)
- No blocking during form entry
