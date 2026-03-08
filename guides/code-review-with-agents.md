# Code Review with Agents

AI coding agents can be effective code reviewers, but only if you use them correctly. They catch different things than humans do, miss different things than humans do, and respond to different kinds of prompts.

## What agents are good at

**Mechanical consistency.** Agents catch naming inconsistencies, style violations, missing imports, and pattern deviations across large files without getting fatigued. If your route handlers should all validate input with Zod before processing, the agent will catch the one that doesn't.

**Known anti-patterns.** Agents reliably flag common issues: SQL injection risks, hardcoded secrets, missing error handling, empty catch blocks, N+1 query patterns. They've seen these patterns millions of times in training data.

**Cross-referencing.** Agents can check that a new endpoint follows the same pattern as existing ones, that a new test covers the same cases as similar tests, or that a migration matches the ORM schema.

**Tedious checks.** Type correctness, missing null checks, off-by-one errors, missing edge cases in switch statements. The stuff humans glaze over on the third file in a review.

## What agents miss

**Business logic correctness.** An agent can verify that your discount calculation function handles negative values, but it can't tell you whether a 15% discount on bulk orders is the right business rule. It doesn't know your domain.

**UX judgment.** An agent can check that your error message exists, but it can't tell you whether "Invalid input" is a helpful message for your users or whether the loading spinner is in the right place.

**Performance at scale.** Agents can flag obvious N+1 queries, but they can't predict whether your query will be slow with 10 million rows or whether your caching strategy will cause thundering herd problems. Real-world performance requires load testing, not code review.

**Organizational context.** The agent doesn't know that this module is being deprecated next quarter, that the team decided to stop using Redux, or that the intern wrote the original code and it needs extra scrutiny.

**Security in context.** Agents catch pattern-level security issues (hardcoded secrets, SQL injection) but miss logic-level security issues (authorization checks that are present but wrong, rate limits that are too generous, CORS configurations that are technically valid but too permissive for your use case).

## How to prompt for useful reviews

### Bad: "Review this code"

This produces a laundry list of generic observations. The agent will comment on naming, formatting, and obvious patterns without any focus.

### Good: Ask specific questions

- "Review this PR for security issues. Focus on the new authentication endpoints and the session management changes."
- "Check if the error handling in `src/routes/orders.ts` is consistent with how we handle errors in `src/routes/users.ts`."
- "This migration adds a new index. Check if there are any queries in the codebase that would benefit from it, and flag any queries that might be slowed down by it."
- "Review the test coverage for the payment flow. Are there edge cases we're missing?"

Scoped questions get better answers than open-ended ones.

### Structure your review request

For large changes, guide the agent through areas of concern:

1. "First, check the database migration for safety (reversibility, index usage, data integrity)."
2. "Then check the API endpoints for input validation and error handling consistency."
3. "Finally, check the test coverage. Are the critical paths tested?"

## Combining agent and human review

The most effective code review uses both. Here's a practical workflow:

### Agent first, human second

1. **Agent review for mechanical issues.** Run the agent over the PR first. Let it catch consistency violations, missing error handling, security anti-patterns, and test coverage gaps. Fix these before human review.
2. **Human review for judgment calls.** The human reviewer focuses on architecture, business logic, design decisions, naming quality, and whether the approach is right, not just correct. The human doesn't waste time on things the agent already caught.

This workflow respects everyone's time. The human reviewer sees clean code and can focus on the high-value feedback.

### Using OVC skills for review

OVC skills provide structured review workflows that are more thorough than ad-hoc prompts:

| Review need | Skill to use |
|---|---|
| Full codebase health check | `ovc-audit` |
| Quick quantitative assessment | `ovc-scorecard` |
| Security-focused review | `ovc-security-review` |
| Error handling patterns | `ovc-error-handling` |
| API design consistency | `ovc-api-review` |
| Test coverage gaps | `ovc-test-coverage` |
| Performance anti-patterns | `ovc-performance-check` |
| Duplication and DRY | `ovc-dry-check` |
| Accessibility issues | `ovc-accessibility-review` |
| Environment and config | `ovc-env-check` |
| Dependency health | `ovc-dependency-check` |

Use these as part of your regular review cycle, not just when problems are suspected.

## Review cadence

- **Every PR:** Quick agent review for mechanical issues (can be automated in CI).
- **Weekly or per-sprint:** Run `ovc-scorecard` to track trends.
- **Monthly or per-milestone:** Full `ovc-audit` on a fresh session.
- **Before major releases:** `ovc-security-review` + `ovc-performance-check`.
- **When onboarding to a new codebase:** `ovc-audit` + `ovc-scorecard` to get a baseline.
