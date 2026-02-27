# Scoring Rubric

Criteria for each score level across all eight categories. Use these to assign consistent, repeatable scores.

A score of **3** is a reasonable baseline -- it means the codebase does the basics right but has room for improvement. Most real projects land between 2 and 4 on most categories. Reserve 5 for genuinely solid work, and 1 for codebases with serious problems in that area.

---

## Architecture

**5 - Intentional and consistent.**
Clear, consistent project structure. Separation of concerns is maintained throughout. One pattern per concern (not two competing approaches). Files and modules have single, well-defined responsibilities. The architecture would scale cleanly to 10x the current size.

**4 - Solid with minor gaps.**
Good structure overall. Separation of concerns is mostly maintained. One or two areas where patterns are inconsistent or boundaries are blurry, but the overall direction is clear and intentional.

**3 - Functional but inconsistent.**
Recognizable structure, but some areas are well-organized while others drift. Some files take on too many responsibilities. A new developer could figure it out but would have questions about why certain things are where they are.

**2 - Disorganized with some structure.**
Some attempt at organization, but competing patterns, unclear boundaries, and files that mix concerns. Evidence of decisions made by default (likely by an AI agent) rather than intentionally.

**1 - No discernible structure.**
Files in the root with no organization. No separation of concerns. Business logic mixed with routing, database access, and presentation. Would require a significant rewrite to impose structure.

---

## Code Quality

**5 - Clean and DRY.**
No meaningful duplication. Naming is consistent and descriptive. Shared logic is properly extracted. Code reads naturally and communicates intent. A new developer would immediately understand the conventions.

**4 - Good with minor issues.**
Little duplication. Naming is mostly consistent. One or two places where logic could be better extracted, but nothing that creates real maintenance risk.

**3 - Acceptable but shows wear.**
Some duplicated logic across files. Naming is mostly consistent but has drift (different conventions in different areas). Some utility code that could be extracted but hasn't been.

**2 - Noticeable problems.**
Significant duplication. Copy-pasted blocks with minor variations. Inconsistent naming conventions. Logic repeated in multiple places that should be shared. Signs of code generated in separate sessions without cross-referencing.

**1 - Pervasive issues.**
Widespread duplication across files. No naming consistency. Functions that are hundreds of lines long. Code that no one would want to maintain. Every change risks breaking something unexpected.

---

## Security

**5 - Secure by design.**
No hardcoded secrets. All inputs validated consistently. Auth on every endpoint that needs it. Parameterized queries exclusively. CORS, rate limiting, and security headers configured properly. Error messages don't leak internals. Dependencies free of known CVEs.

**4 - Strong with minor gaps.**
No hardcoded secrets. Most inputs validated. Auth present on protected routes. Parameterized queries. One or two endpoints or patterns where validation could be tighter, but no exploitable issues.

**3 - Basic security present.**
Environment variables for secrets (mostly). Some input validation but inconsistent. Auth exists but coverage is incomplete. Database queries are parameterized. CORS configured but may be overly permissive. Some security headers present.

**2 - Significant gaps.**
Some hardcoded values that look like secrets or defaults left in place. Input validation is spotty or missing on several endpoints. Auth present but bypassable or inconsistent. CORS set to wildcard. No rate limiting. Error messages may expose internal details.

**1 - Insecure.**
Hardcoded secrets in source code. No input validation. Endpoints exposed without authentication. SQL built with string concatenation. No CORS configuration. No security headers. Known vulnerabilities in dependencies.

---

## Dependencies

**5 - Well-managed.**
Established, actively maintained packages for all non-trivial functionality. No hand-rolled implementations of solved problems. APIs used correctly per documentation. No deprecated packages. Dependencies pinned appropriately. No unused packages in the manifest.

**4 - Good with minor issues.**
Mostly uses established packages. One or two cases where something could have used a library instead of custom code, but nothing egregious. Dependencies are reasonably current.

**3 - Mixed.**
Uses libraries for most things but has some hand-rolled implementations of solved problems (custom validation, custom date formatting, custom auth). Some dependencies are outdated. Mostly correct API usage with occasional deprecated methods.

**2 - Problematic.**
Multiple hand-rolled implementations of common functionality. Deprecated packages still in use. Some incorrect or outdated API usage. Unused packages in the manifest. Multiple packages solving the same problem.

**1 - Unmanaged.**
Extensive hand-rolled code for problems with battle-tested solutions. Dependencies severely outdated or abandoned. Known CVEs in dependency tree. No apparent awareness of the package ecosystem.

---

## Testing

**5 - Comprehensive and meaningful.**
Tests exist for all critical paths (auth, data mutations, business logic). Tests assert real behavior, not just "doesn't throw." Both unit and integration tests present. Test suite passes. Tests serve as reliable documentation of expected behavior.

**4 - Solid coverage of important paths.**
Critical paths are tested. Tests assert meaningful behavior. Some gaps in edge cases or less critical paths. Test suite passes.

**3 - Tests exist but gaps are notable.**
Some tests present. Happy paths are mostly covered. Missing tests for critical error cases or edge cases. Test quality varies -- some assert behavior, others are trivial. Test suite passes (or mostly passes).

**2 - Minimal testing.**
A few tests exist but coverage is sparse. Critical paths like auth or payments may be untested. Some tests are trivial (testing that a function exists). Test suite may have failing tests.

**1 - No meaningful tests.**
No test files, or tests that exist are empty/trivial/broken. No test runner configured. No CI test step. Changes have no regression protection.

---

## Tooling

**5 - Fully equipped.**
Rules file present and customized (not just the template). Linter configured with strict settings. Type checking enabled (strict mode). Formatter configured and enforced. CI runs linting and type checks. MCP or tool integrations configured for the agent.

**4 - Well-configured.**
Rules file present with some customization. Linter configured with reasonable strictness. Type checking enabled. Formatter present. One or two areas where tooling could be stricter.

**3 - Basic tooling.**
Some tooling present -- either a linter or formatter, but not fully configured. Type checking exists but not in strict mode. No rules file, or using the default template without customization. No agent tool integrations.

**2 - Minimal tooling.**
One basic tool (e.g., default ESLint config). No type checking, or type checking with permissive settings. No rules file. No formatter enforcement.

**1 - No tooling.**
No linter, no formatter, no type checking, no rules file, no CI. The agent is operating with zero guardrails.

---

## Error Handling

**5 - Consistent and informative.**
A clear, consistent error handling pattern throughout the codebase. Domain-specific error types. Errors propagated with context. Appropriate status codes returned. No swallowed errors. Error messages helpful for debugging without leaking internals.

**4 - Good with minor inconsistencies.**
Error handling is mostly consistent. Status codes are appropriate. Errors are logged with useful context. One or two places where error handling could be better, but no silent failures.

**3 - Functional but inconsistent.**
Error handling exists but the pattern varies across the codebase. Some areas return proper status codes, others default to 500. Some logging present. No obviously swallowed errors, but error context is sometimes insufficient.

**2 - Spotty.**
Inconsistent error handling. Some empty catch blocks. Generic error messages. Status codes don't always match the error. Some errors are swallowed or logged without context. Debugging a production issue would be painful.

**1 - Missing or broken.**
No error handling pattern. Widespread empty catch blocks. Errors swallowed silently. Generic 500 responses. Stack traces potentially exposed to clients. No error logging.

---

## Deployment Readiness

**5 - Production-ready.**
Environment variables for all configuration. A clear distinction between dev/staging/production config. Dockerfile, infrastructure-as-code (render.yaml, terraform, etc.), or CI/CD pipeline present. Health check endpoint. No secrets in the repository. README covers how to deploy.

**4 - Close to production-ready.**
Environment variables for sensitive config. Deployment config exists (Dockerfile or CI pipeline). Most things are in place; one or two items missing (health check, staging config, or deployment documentation).

**3 - Deployable with effort.**
Basic environment variable usage. No Dockerfile or IaC, but the project could be deployed with some manual setup. Configuration is mostly externalized. Some deployment steps would need to be figured out.

**2 - Needs significant work.**
Some config hardcoded. No deployment configuration. Unclear how to get this running in production. Environment variables used inconsistently. May have development-only defaults that would be insecure in production.

**1 - Not deployable.**
Configuration hardcoded throughout. Secrets potentially in source. No environment variable handling. No deployment config of any kind. Deploying this would require substantial rework.
