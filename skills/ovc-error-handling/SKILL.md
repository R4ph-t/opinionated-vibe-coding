---
name: ovc-error-handling
description: Audit error handling patterns across the codebase. Finds swallowed errors, inconsistent error types, missing logging context, and leaked internal details. Use when the user wants to review exception handling, find silent failures, or improve error consistency. Also trigger on "error handling check", "find swallowed errors", "error patterns", "exception handling review".
metadata:
  author: renderinc
  version: "1.0"
license: MIT
---

# OVC Error Handling Check

Scan the codebase for error handling anti-patterns and inconsistencies. Poor error handling is one of the hardest problems to debug in production because the evidence is missing by the time you look.

## What to look for

### Swallowed errors
- Empty catch blocks: `catch (e) {}`, `except: pass`, `_ = err`
- Catch blocks that only log without re-throwing or returning an error response
- `.catch(() => {})` on promises
- Ignored return values from functions that return errors (Go: `result, _ := doThing()`)

### Inconsistent error types
- Mix of custom error classes and generic `Error`/`Exception` with string messages
- No error codes or categories for client-facing errors
- Different error response shapes across API endpoints
- Throwing strings instead of Error objects

### Missing logging context
- Errors logged without context (just the error message, no operation or input info)
- Stack traces discarded during error wrapping
- No correlation IDs or request context in error logs
- Missing structured fields (logging bare strings instead of key-value pairs)

### Leaked internals
- Stack traces, file paths, or SQL queries exposed in API error responses
- Internal error messages shown to users
- Database error details (column names, constraint names) in responses
- Environment variable names or config paths in error messages

### Missing error handling
- Async functions without try/catch or .catch()
- Database operations without error handling
- External API calls without timeout or error handling
- File operations without existence checks or error handling

## Steps

### 1. Scan for anti-patterns

Search the codebase for the patterns listed above. Start with the highest-severity issues (swallowed errors, leaked internals) and work down.

### 2. Check error type consistency

Examine how errors are defined and used:
- Is there a consistent custom error class/type hierarchy?
- Do API endpoints return errors in a consistent shape?
- Are HTTP status codes used correctly and consistently?

### 3. Check logging practices

Review how errors are logged:
- Are errors logged with context (operation, input, user/request ID)?
- Are stack traces preserved when errors are wrapped?
- Is structured logging used (JSON with fields) or just string concatenation?
- Are secrets or PII at risk of being logged in error context?

### 4. Report findings

For each issue:
- File and line number
- Severity: CRITICAL (swallowed errors in critical paths, leaked secrets), HIGH (missing error handling, leaked internals), MEDIUM (inconsistent patterns, missing context), LOW (style issues)
- What the problem is
- How to fix it

Sort by severity. Group related issues (e.g., all instances of the same anti-pattern together).
