---
name: ovc-security-review
description: Run a security-focused review of the codebase or a specific change. Checks for hardcoded secrets, input validation gaps, SQL injection, auth issues, CORS misconfig, and dependency vulnerabilities. Use when the user asks for a security review, security audit, vulnerability check, or mentions concerns about security. Also trigger on "is this secure", "check for vulnerabilities", "security scan".
metadata:
  author: renderinc
  version: "1.0"
license: MIT
---

# OVC Security Review

Run a focused security review. Can target the full codebase or a specific set of changes.

## Steps

### 1. Determine scope

Ask the user: full codebase review, or specific files/changes?

For specific changes, focus on the diff. For full codebase, work through all categories below.

### 2. Check each category

**Secrets and Credentials**
- Search for hardcoded strings that look like API keys, tokens, passwords, or connection strings
- Check for secrets in configuration files committed to version control
- Look for secrets in error messages or log statements
- Check .env files are in .gitignore

**Input Validation**
- Trace user input from entry points (API routes, form handlers) through to where it's used
- Check for unvalidated input reaching database queries, file operations, shell commands, or external API calls
- Look for missing or incomplete sanitization
- Check for path traversal vulnerabilities in file operations

**Authentication and Authorization**
- Identify endpoints that should be protected but aren't
- Check for authorization gaps (can user A access user B's resources?)
- Review session/token management (expiration, rotation, storage)
- Check for authentication bypass paths

**Database**
- Search for string concatenation or interpolation in SQL queries
- Verify all queries use parameterized statements or an ORM
- Check database connection credentials handling

**API and Network**
- Review CORS configuration (reject wildcard in production)
- Check for rate limiting on public and authentication endpoints
- Look for sensitive data in URL parameters or query strings
- Verify HTTPS enforcement

**Dependencies**
- Run `npm audit` / `pip audit` / equivalent for the stack
- Check for dependencies with known CVEs
- Identify unmaintained dependencies (no updates in 12+ months)

**Error Handling**
- Check that stack traces are not exposed to clients
- Verify error messages don't reveal system internals
- Look for information leakage in API responses

### 3. Report findings

For each issue:
- File and line number
- Severity: CRITICAL, HIGH, MEDIUM, LOW
- What the vulnerability is
- How to fix it

Sort by severity. Be specific and actionable.
