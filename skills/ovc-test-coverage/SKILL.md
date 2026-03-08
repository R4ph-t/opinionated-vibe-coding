---
name: ovc-test-coverage
description: Analyze test coverage gaps in the codebase. Maps critical code paths, cross-references them with existing tests, and identifies what's untested. Prioritizes gaps by risk. Use when the user wants to improve test coverage, find untested code, or figure out what to test next. Also trigger on "test coverage", "what's untested", "coverage gaps", "what should I test", "missing tests".
metadata:
  author: r4ph-t
  version: "1.0"
license: MIT
---

# OVC Test Coverage

Identify what's tested, what's not, and what matters most. This is not about hitting a coverage percentage; it's about finding the untested code that will hurt you when it breaks.

## Steps

### 1. Identify the test setup

Detect the test framework (Jest, Vitest, pytest, RSpec, Go testing, etc.) and any existing coverage configuration. Check for existing coverage reports or CI coverage gates.

Note the test file naming convention and location (colocated `*.test.ts`, separate `tests/` directory, etc.).

### 2. Map critical code paths

Scan the codebase and categorize code by risk:

**Critical (must be tested)**
- Authentication and authorization logic
- Payment and billing flows
- Data mutations (create, update, delete operations)
- External service integrations (API calls, webhooks)
- Input validation and sanitization
- Middleware that enforces security or access control

**Important (should be tested)**
- Business logic and domain rules
- Data transformations and formatting
- API route handlers (request/response contracts)
- Database queries with complex conditions
- State management logic (reducers, stores)

**Lower priority**
- Pure utility functions
- Configuration and setup code
- Simple CRUD with no business logic
- Static content rendering

### 3. Cross-reference with existing tests

For each critical and important path identified above:
- Check if a corresponding test file exists
- Check if the test actually exercises the critical behavior (not just the happy path)
- Check if error cases and edge cases are covered
- Check if integration points are tested (not just mocked away)

### 4. Report findings

For each gap:
- The file and function/method that's untested
- Risk category (critical, important, lower priority)
- What specifically should be tested (happy path, error path, edge case, integration)
- A brief description of the test to write

Group by risk category. Within each category, sort by the code that changes most frequently (higher churn = higher test value).

### 5. Quick wins

Identify the three to five tests that would provide the most coverage improvement for the least effort. These are typically:
- Untested critical paths with straightforward inputs and outputs
- Functions that are called from many places
- Code that has had recent bugs or frequent changes
