---
name: ovc-api-review
description: Review API design for consistency, correctness, and completeness. Checks endpoint naming, HTTP methods, error responses, pagination, validation, and missing best practices. Use when the user wants to review their API, check endpoint design, or audit REST conventions. Also trigger on "API review", "review my endpoints", "API design check", "REST review", "endpoint audit".
metadata:
  author: r4ph-t
  version: "1.0"
license: MIT
---

# OVC API Review

Audit the API layer for consistency, correctness, and completeness. Inconsistent APIs are painful for consumers and a sign of organic growth without design review.

## Steps

### 1. Inventory all endpoints

Scan route definitions and build a table:

| Method | Path | Auth | Input validation | Response shape | Status codes |
|---|---|---|---|---|---|

For frameworks with file-based routing (Next.js, Nuxt, SvelteKit), scan the route directories. For explicit routing (Express, Hono, FastAPI, Rails), scan the route registration files.

### 2. Check against the API checklist

Use [references/api-checklist.md](references/api-checklist.md) to evaluate each area:

**Naming and structure**
- Resources are plural nouns (`/users`, not `/user` or `/getUsers`)
- Paths use kebab-case, not camelCase or snake_case
- No verbs in paths (use HTTP methods instead: `POST /users`, not `POST /createUser`)
- Consistent nesting for sub-resources (`/users/:id/orders`)
- API version prefix if the project uses versioning (`/v1/users`)

**HTTP methods**
- GET for reads (no side effects, cacheable)
- POST for creates (returns 201 with the created resource)
- PUT for full replacements, PATCH for partial updates
- DELETE for removals (returns 204 or 200)
- No state-changing operations on GET

**Error responses**
- Consistent error shape across all endpoints (e.g., `{ error: string, code: string }`)
- Appropriate HTTP status codes (400 for bad input, 401 for unauthenticated, 403 for unauthorized, 404 for not found, 409 for conflicts, 422 for validation failures, 500 for server errors)
- No stack traces or internal details in production error responses
- Validation errors include field-level detail

**Input handling**
- All endpoints validate input before processing
- Input size limits (max body size, max array length)
- Appropriate content-type enforcement

**Pagination, filtering, sorting**
- List endpoints support pagination (cursor-based or offset-based)
- Consistent pagination response shape (`{ data: [], total: number, next_cursor: string }`)
- Filter and sort parameters follow a consistent pattern

**Auth and rate limiting**
- All non-public endpoints require authentication
- Rate limiting on public endpoints and authentication endpoints
- Appropriate scoping of permissions

### 3. Report findings

For each issue:
- The endpoint(s) affected
- What the inconsistency or gap is
- How to fix it
- Priority: HIGH (security or correctness issue), MEDIUM (inconsistency that hurts DX), LOW (convention nit)

Group by category from the checklist.
