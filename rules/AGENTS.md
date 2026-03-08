# Opinionated Vibe Coding Rules

You are working in a codebase that follows the Opinionated Vibe Coding methodology.
These rules are non-negotiable. Follow them on every task.

## Before Writing Any Code

### Plan First
Before writing or modifying code, outline your approach. Include:
- What files you'll create or modify
- What architectural decisions you're making and why
- What existing code might be affected
- What packages or libraries you'll use

Wait for approval before proceeding. Do not start coding until the plan is reviewed.

### Check for Existing Solutions
Before implementing any functionality, check if a well-maintained package already exists for it.
Do not hand-roll solutions for problems that established libraries solve. This includes but is not limited to:
- Date/time manipulation
- HTTP clients
- Validation
- Authentication
- CSV/JSON/XML parsing
- Encryption/hashing
- Email sending
- Image processing

If a suitable package exists with active maintenance and reasonable adoption, use it.

### Read the Documentation
Before using any library, framework, or API, read the official documentation.
Do not guess at function signatures, API shapes, or configuration options.
Do not use deprecated methods or outdated patterns.
If you are unsure about an API, say so and look it up rather than hallucinating.

## While Writing Code

### Architecture and Structure
- Maintain clear separation of concerns. Each file and module should have a single, well-defined responsibility.
- Do not make architectural decisions silently. If you're introducing a new pattern, a new abstraction layer, or changing the structure of the codebase, flag it explicitly.
- Follow established patterns already present in the codebase. Do not introduce competing patterns.

### DRY (Don't Repeat Yourself)
- Do not duplicate logic across files. If logic exists elsewhere in the codebase, reference or import it.
- If you find yourself writing similar code in multiple places, extract it into a shared module.
- When you see existing duplication, flag it.

### Security
- Never hardcode secrets, API keys, tokens, or credentials. Use environment variables.
- Validate all inputs. Never trust user-provided data.
- Do not expose endpoints without authentication unless explicitly instructed.
- Use parameterized queries for database operations. No string concatenation for SQL.
- Set secure defaults. CORS, rate limiting, HTTPS, secure headers.
- If you're unsure about a security implication, flag it rather than guessing.

### Error Handling and Logging
- Never swallow errors. No empty catch blocks, no bare `except: pass`, no ignored error returns.
- Use structured error types with context, not generic Error/Exception with string messages.
- Log errors with context: what operation was attempted, what input triggered it, what failed.
- Use structured logging (JSON) in production. Human-readable format in development.
- Never log secrets, tokens, passwords, or PII.
- Do not expose internal error details to clients. Return user-safe messages; log the full trace server-side.

### Database Safety
- Use migrations for all schema changes. Never run raw DDL in production.
- Migrations must be reversible unless explicitly approved otherwise.
- Add indexes for columns used in WHERE clauses and foreign keys.
- Never delete columns or tables without confirming no code references them.
- Use transactions for multi-step data operations.
- Validate data at the application layer before writing; do not rely solely on database constraints.

### Testing
- After modifying existing code, run the existing test suite to check for regressions.
- When adding new functionality, write tests for it.
- When fixing a bug, write a test that would have caught it.
- Do not delete or modify existing tests without explicit approval.

### Linting and Formatting
- All code must pass the project's linter and formatter before being considered done.
- If the linter flags an issue, fix it. Do not disable linter rules without explicit approval.
- Follow the project's formatting configuration exactly.

### Commits
- Use Conventional Commits: `feat:`, `fix:`, `refactor:`, `test:`, `chore:`, `docs:`.
- Keep the subject line under 72 characters. Use the imperative mood ("add auth middleware", not "added auth middleware").
- Use the commit body to explain *why*, not *what*. The diff shows the what. Keep it to 1-2 sentences.
- Commit at logical breakpoints. Do not let work accumulate across multiple unrelated changes.
- After completing a feature, fixing a bug, finishing a refactor, or getting tests to pass, suggest a commit. Do not wait to be asked.
- Each commit should represent one coherent change. If you need to describe two unrelated things, that's two commits.

## Project-Specific Rules

[CUSTOMIZE: Add your project-specific rules below. The more specific you are, the better the output. Examples:]

### Tech Stack
[CUSTOMIZE: List your specific technologies, e.g.]
<!-- 
- Runtime: Node.js 20+
- Framework: Hono
- Database: PostgreSQL with Drizzle ORM
- Validation: Zod
- Testing: Vitest
- Package manager: pnpm
-->

### File Structure
[CUSTOMIZE: Define your file organization, e.g.]
<!--
- API routes: /src/routes/
- Database schemas: /src/db/
- Shared utilities: /src/lib/
- Types: /src/types/
- Tests: colocated with source files as *.test.ts
-->

### Naming Conventions
[CUSTOMIZE: Define your naming patterns, e.g.]
<!--
- Files: kebab-case
- Functions: camelCase
- Types/Interfaces: PascalCase
- Constants: SCREAMING_SNAKE_CASE
- Database tables: snake_case
- API endpoints: kebab-case
-->

### API Design
[CUSTOMIZE: Define your API patterns, e.g.]
<!--
- All API routes have a corresponding schema in /src/schemas/
- Validate all inputs with Zod before processing
- Never return raw database objects, always map to response types
- Error responses follow the format: { error: string, code: string }
- Use appropriate HTTP status codes
-->

### Dependencies
[CUSTOMIZE: List preferred and banned packages, e.g.]
<!--
Preferred:
- HTTP: use the built-in fetch API, not axios
- Dates: use date-fns, not moment
- Validation: use zod

Banned:
- lodash (use native JS methods)
- moment (deprecated, use date-fns)
-->

## When You're Unsure

- If you're about to make a decision that affects the project's architecture, ask first.
- If you're not confident about a library's API, say so rather than guessing.
- If a task feels too large or ambiguous, propose breaking it into smaller steps.
- If you notice something wrong with the existing code (bugs, security issues, tech debt), flag it even if it's not part of the current task.
