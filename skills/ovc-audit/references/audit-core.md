# Core Audit Checklist

These checks apply to every project regardless of stack. Work through each section and note specific findings with file paths, line numbers, and severity.

## Architecture

- [ ] Is there a clear, consistent project structure?
- [ ] Are there competing patterns (e.g., some routes use one approach, others use another)?
- [ ] Is separation of concerns maintained (routing, business logic, data access, presentation in distinct layers)?
- [ ] Are there files or modules doing too many things?
- [ ] Are there circular dependencies?
- [ ] Is there a clear data flow through the application?
- [ ] Would this architecture hold up at 10x the current size?

## Code Quality / DRY

Look for duplication by comparing clusters of similar files (all route handlers, all services, all models).

- [ ] Are there code blocks that are identical or differ only in variable names?
- [ ] Are there copy-pasted functions with minor modifications?
- [ ] Are multiple files implementing the same pattern (e.g., similar error handling, validation, response structure)?
- [ ] Are configuration values repeated instead of centralized?
- [ ] Are there repeated utility functions (string formatting, date manipulation, data transformation) that should be extracted?
- [ ] Are error handling or auth checks duplicated instead of using shared middleware?

## Security

- [ ] Are there hardcoded secrets, API keys, or tokens? Search for strings matching `sk-`, `key-`, `token`, `password`, `secret` in source files.
- [ ] Is input validation present and consistent across all endpoints?
- [ ] Are database queries parameterized (no string concatenation for SQL)?
- [ ] Are endpoints properly authenticated? Check for routes missing auth middleware.
- [ ] Is CORS configured specifically (not just wildcard `*`)?
- [ ] Are there rate limits on public endpoints and authentication endpoints?
- [ ] Are error messages leaking internal details (stack traces, file paths, SQL, column names)?
- [ ] Are dependencies free of known vulnerabilities? Run `npm audit`, `pip audit`, or equivalent.

## Dependencies

- [ ] Are there outdated dependencies with available updates?
- [ ] Are there unused dependencies in the manifest (package.json, requirements.txt)?
- [ ] Are dependency versions pinned appropriately (not floating on `latest`)?
- [ ] Are there dependencies with known vulnerabilities?
- [ ] Are there multiple packages solving the same problem (e.g., both axios and got, both dayjs and moment)?
- [ ] Are there hand-rolled implementations of things well-maintained packages solve (custom date parsing, custom validation, custom auth, custom HTTP clients)?

## Error Handling

Search for these anti-patterns:

- [ ] **Swallowed errors:** empty catch blocks (`catch (e) {}`, `catch (_)`, `except: pass`, `except Exception: pass`, `_ = err`), `.catch(() => {})` on promises
- [ ] **Inconsistent error types:** mix of custom error classes and generic `Error`/`Exception` with string messages, different error response shapes across endpoints
- [ ] **Missing logging context:** errors logged with just the message, no operation name or input, no correlation/request ID
- [ ] **Leaked internals:** stack traces, file paths, SQL queries, or column names in API error responses
- [ ] **Missing error handling:** async functions without try/catch, database operations without error handling, external API calls without timeout or error handling

## Environment & Config

- [ ] Does `.env.example` or `.env.sample` exist with all required variables?
- [ ] Is `.env` listed in `.gitignore`?
- [ ] Are there hardcoded URLs, ports, or credentials that should be environment variables? Search for `http://localhost`, hardcoded port numbers, email addresses used as defaults.
- [ ] Is configuration validated at startup (using Zod, Pydantic, envalid, or similar)?
- [ ] Are there any `.env` files (other than `.env.example`) committed to the repository?

## File Organization

- [ ] Are there files over 300 lines with multiple distinct responsibilities?
- [ ] Are there vaguely named files (`utils.ts`, `helpers.py`, `misc.go`, `common.rb`) that bundle unrelated logic?
- [ ] Are there region comments (`// --- Auth ---`, `# ===== Validation =====`) acting as section boundaries that should be file boundaries?

## Testing

- [ ] Is there meaningful test coverage (not just test files that exist but test real behavior)?
- [ ] Are critical paths tested (auth, payments, data mutations, external integrations)?
- [ ] Do tests assert behavior or are they trivial (testing that a function exists, testing default values)?
- [ ] Are error cases and edge cases covered, not just the happy path?
- [ ] Are there integration tests, not just unit tests?
- [ ] Is the test suite actually passing?

## Reinvented Wheels

- [ ] Custom date parsing or formatting when date-fns, dayjs, or Luxon exists?
- [ ] Custom validation when Zod, Joi, Yup, or Pydantic exists?
- [ ] Custom HTTP clients when fetch, axios, or httpx exists?
- [ ] Custom auth implementations when Passport, NextAuth, or Django Auth exists?
- [ ] Custom retry/backoff logic when established libraries exist?

## What Looks Accidental

- [ ] What decisions look like AI defaults rather than intentional choices?
- [ ] Are there patterns that started one way and drifted into another approach partway through?
- [ ] What would be the first thing a new team member would question?
- [ ] What would break if the codebase grew 10x?
