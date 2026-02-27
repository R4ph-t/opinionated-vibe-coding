# Audit Checklist

## Architecture
- [ ] Is there a clear, consistent architecture?
- [ ] Are there competing patterns (e.g., some routes use one approach, others use another)?
- [ ] Is separation of concerns maintained?
- [ ] Are there files or modules doing too many things?
- [ ] Are there circular dependencies?
- [ ] Is there a clear data flow through the application?

## DRY Violations
- [ ] Is logic duplicated across files?
- [ ] Are there multiple implementations of the same utility function?
- [ ] Are there copy-pasted code blocks with minor variations?
- [ ] Are configuration values repeated instead of centralized?

## Reinvented Wheels
- [ ] Are there hand-rolled implementations of things well-maintained packages solve?
- [ ] Custom date parsing when date-fns/dayjs exists?
- [ ] Custom validation when zod/joi exists?
- [ ] Custom HTTP clients when fetch/axios exists?
- [ ] Custom auth implementations when passport/next-auth exists?

## Security
- [ ] Are there hardcoded secrets, API keys, or tokens?
- [ ] Is input validation present and consistent?
- [ ] Are database queries parameterized?
- [ ] Are endpoints properly authenticated?
- [ ] Is CORS configured (not just wildcard)?
- [ ] Are there rate limits on public endpoints?
- [ ] Are error messages leaking internal details?
- [ ] Are dependencies free of known vulnerabilities?

## Testing
- [ ] Is there meaningful test coverage?
- [ ] Are critical paths tested (auth, payments, data mutations)?
- [ ] Do tests actually assert behavior or are they trivial?
- [ ] Are there integration tests, not just unit tests?
- [ ] Is the test suite actually passing?

## Dependencies
- [ ] Are there outdated dependencies?
- [ ] Are there unused dependencies in package.json/requirements.txt?
- [ ] Are dependency versions pinned appropriately?
- [ ] Are there dependencies with known vulnerabilities?
- [ ] Are there multiple packages solving the same problem?

## Error Handling
- [ ] Is error handling consistent across the codebase?
- [ ] Are there empty catch blocks swallowing errors?
- [ ] Do async operations have proper error handling?
- [ ] Are errors logged with enough context to debug?
- [ ] Are appropriate status codes returned?

## What Looks Accidental
- [ ] What decisions look like AI defaults rather than intentional choices?
- [ ] Are there patterns that started one way and drifted?
- [ ] What would be the first thing a new team member would question?
- [ ] What would break if the codebase grew 10x?
