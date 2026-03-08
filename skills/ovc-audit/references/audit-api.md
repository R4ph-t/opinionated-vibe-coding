# API Audit Checklist

Load this checklist when the project has an API layer (Express, Hono, FastAPI, Flask, Django, Rails, Gin, or `routes/`/`controllers/`/`api/` directories).

## Naming and Structure

- [ ] Are resources plural nouns (`/users`, not `/user` or `/getUsers`)?
- [ ] Do paths use kebab-case (not camelCase or snake_case)?
- [ ] Are there verbs in paths (`/createUser`, `/deleteItem`) instead of using HTTP methods?
- [ ] Is nesting consistent for sub-resources (`/users/:id/orders`)?
- [ ] If the project uses API versioning, is the prefix consistent (`/v1/...`)?

## HTTP Methods

- [ ] Is GET used for reads only (no side effects)?
- [ ] Does POST return 201 with the created resource?
- [ ] Is there a clear distinction between PUT (full replacement) and PATCH (partial update)?
- [ ] Does DELETE return 204 or 200 consistently?
- [ ] Are there state-changing operations on GET endpoints?

## Error Responses

- [ ] Is the error response shape consistent across all endpoints (e.g., `{ error: string, code: string }`)?
- [ ] Are HTTP status codes used correctly (400 bad input, 401 unauthenticated, 403 unauthorized, 404 not found, 409 conflict, 422 validation failure, 500 server error)?
- [ ] Do validation errors include field-level detail?
- [ ] Are stack traces or internal details hidden in production error responses?

## Input Handling

- [ ] Do all endpoints validate input before processing?
- [ ] Are there input size limits (max body size, max array length)?
- [ ] Is content-type enforced where it matters?

## Pagination and Filtering

- [ ] Do list endpoints support pagination (cursor-based or offset-based)?
- [ ] Is the pagination response shape consistent across endpoints?
- [ ] Do filter and sort parameters follow a consistent pattern?

## Auth and Rate Limiting

- [ ] Do all non-public endpoints require authentication? Check for routes missing auth middleware.
- [ ] Is there rate limiting on public endpoints and authentication endpoints (login, signup, password reset)?
