# zai-speckit Constitution

## Code Quality & Maintainability

### Principle: Simplicity Over Cleverness
Write straightforward, readable code, with succinct and clear comments. Avoid clever tricks or obscure patterns. Code should be immediately understandable by team members of varying experience levels. Variable and function names should be descriptive and self-explanatory yet very smart and concise.

For complex code blocks, use comments to explain the code. Comments should be concise and to the point.

### Principle: Small, Focused Functions
Functions should do one thing well. Keep functions under 50 lines. If a function grows larger, break it into smaller, well-named helper functions. Don't break functions into smaller functions when unnecessary, as it can make the code more difficult to understand.

### Principle: Clear Naming
Use descriptive names that reveal intent. Prefer `calculateOrderFillPrice` over `calc` or `process`. Boolean variables should be prefixed with `is`, `has`, `should`, or `can`.

### Principle: No Dead Code
Remove unused code immediately. Don't comment out code "just in case" - that's what version control is for. Keep the codebase lean.

### Principle: DRY with Judgment
Eliminate meaningful duplication, but don't abstract too early. Two similar pieces of code don't need to be unified if they serve fundamentally different purposes or may diverge.

### Principle: Dependencies Are Liabilities
Minimize external dependencies. Every dependency must justify its inclusion. Prefer standard library solutions when reasonable. Regularly audit and remove unused dependencies.

## Testing Standards

### Principle: Test Behavior, Not Implementation
Tests should verify what the code does, not how it does it. This makes tests resilient to refactoring.

### Principle: Fast Test Suite
Unit tests must run in milliseconds. The entire test suite should complete in under 10 seconds. Slow tests break the development flow. No need to test everything, just test main and critical paths.

### Principle: Test Coverage Requirements
- Critical paths (authentication, data validation, financial calculations): 100% coverage
- Business logic: minimum 80% coverage
- Utility functions: minimum 80% coverage
- UI components: test user interactions and edge cases

### Principle: Tests Are Documentation
Test names should read like specifications: `test_rejects_orders_exceeding_account_balance()`. Someone should understand requirements by reading test names alone.

### Principle: No Flaky Tests
Tests must be deterministic. Fix or delete flaky tests immediately. Use dependency injection and mocking to isolate external dependencies.

### Principle: Test Pyramid
- Many fast unit tests (70%)
- Some integration tests (20%)
- Few end-to-end tests (10%)

## User Experience Consistency

### Principle: Consistent API Design
All public APIs must follow the same patterns:
- Consistent parameter ordering (required first, optional last)
- Consistent naming conventions
- Consistent error handling approach
- Consistent return types

### Principle: Fail Fast with Clear Messages
Validate inputs immediately. Error messages must be actionable and specify what went wrong and how to fix it. No cryptic error codes without explanatory messages.

### Principle: Backwards Compatibility
Breaking changes require major version bumps. Deprecated features must have clear migration paths and warnings for at least one minor version before removal.

### Principle: Sensible Defaults
APIs should work with minimal configuration. Common use cases should be one-liners. Advanced configuration should be opt-in.

### Principle: Consistent Error Handling
Use the same error handling pattern throughout:
- Return error objects, don't throw for expected failures
- Reserve exceptions for truly exceptional circumstances
- All errors should be typed/documented

## Performance Requirements

### Principle: Performance Budgets
Define and enforce performance budgets:
- API response time: p95 < 100ms, p99 < 500ms
- Memory usage: monitored and capped per service
- Bundle size: track and prevent bloat

### Principle: Measure Before Optimizing
No premature optimization. Use profiling tools to identify actual bottlenecks. Optimization without measurement is guesswork.

### Principle: Algorithmic Efficiency
Choose appropriate data structures and algorithms from the start. O(n²) is unacceptable for production code operating on user data.

### Principle: Lazy Loading
Load resources only when needed. Support pagination, streaming, and incremental loading for large datasets.

### Principle: Caching Strategy
Cache aggressively but invalidate correctly. All caches must have clear invalidation rules and TTLs.

## Code Review Standards

### Principle: Small Pull Requests
PRs should be under 400 lines of code changes. Large PRs are split into smaller, reviewable chunks.

### Principle: Every PR Needs Tests
Code without tests is not done. Tests should be included in the same PR as the feature.

### Principle: Self-Reviewing First
Review your own PR before requesting review. Check for typos, console.logs, TODOs, and commented code.

### Principle: Constructive Feedback
Code reviews should be respectful and educational. Suggest alternatives, don't just criticize. Approve when code meets standards, even if you would have done it differently.

## Documentation

### Principle: Self-Documenting Code First
Code should be clear enough that it doesn't need comments. Comments explain *why*, not *what*.

### Principle: README for Every Module
Each significant module/package should have a README covering:
- Purpose and scope
- Installation/setup
- Basic usage examples
- Links to detailed docs

### Principle: Keep Docs in Sync
Documentation updates are part of the feature. Outdated docs are worse than no docs.

### Principle: Examples Over Walls of Text
Show, don't just tell. Include code examples for all public APIs.

## Security

### Principle: Validate All Inputs
Never trust user input. Validate, sanitize, and type-check all data crossing boundaries.

### Principle: Secure by Default
Security features should be enabled by default. Disabling security should require explicit opt-in.

### Principle: No Secrets in Code
API keys, passwords, and secrets belong in environment variables or secure vaults, never in code or version control.

## Continuous Improvement

### Principle: Leave It Better
Every file you touch should be slightly better than you found it. Fix obvious issues nearby, improve naming, add missing tests.

### Principle: Regular Refactoring
Dedicate time to technical debt. Refactoring is not optional - it's maintenance.

### Principle: Postmortems for Bugs
Production bugs warrant postmortems. Learn from failures and update processes to prevent recurrence.
