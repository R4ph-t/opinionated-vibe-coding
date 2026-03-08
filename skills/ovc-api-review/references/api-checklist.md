# API Review Checklist

Use this checklist to evaluate API design. Each item is scored as PASS, FAIL, or N/A.

## Naming and Structure

- [ ] Resources use plural nouns (`/users`, `/orders`)
- [ ] Paths use kebab-case for multi-word segments
- [ ] No verbs in resource paths (HTTP methods convey the action)
- [ ] Sub-resources are nested logically (`/users/:id/orders`)
- [ ] Consistent use of API version prefix if versioning is used
- [ ] No deeply nested paths (3+ levels of nesting)

## HTTP Methods

- [ ] GET is used for reads and has no side effects
- [ ] POST is used for creates and returns 201
- [ ] PUT/PATCH are used for updates with correct semantics
- [ ] DELETE is used for removals and returns 204 or 200
- [ ] No state-changing operations use GET
- [ ] PATCH accepts partial payloads and only updates provided fields

## Request Handling

- [ ] All endpoints validate input before processing
- [ ] Request body size limits are configured
- [ ] Content-Type is enforced on endpoints that accept bodies
- [ ] Path and query parameters are validated and typed
- [ ] Array inputs have maximum length limits
- [ ] File upload endpoints have size and type restrictions

## Response Design

- [ ] Successful responses use consistent envelope or direct shape
- [ ] Created resources are returned in POST responses
- [ ] List endpoints return consistent pagination metadata
- [ ] Responses do not include unnecessary internal fields
- [ ] Date/time fields use ISO 8601 format
- [ ] IDs use a consistent format (UUID, integer, etc.)

## Error Handling

- [ ] All endpoints return errors in the same shape
- [ ] HTTP status codes are appropriate (400, 401, 403, 404, 409, 422, 500)
- [ ] Validation errors include field-level detail
- [ ] No stack traces or internal paths in production responses
- [ ] Error codes are machine-readable (string codes, not just messages)
- [ ] 500 errors are logged server-side with full context

## Pagination

- [ ] List endpoints support pagination
- [ ] Pagination style is consistent across all endpoints (cursor or offset)
- [ ] Response includes total count or has-more indicator
- [ ] Default page size is reasonable (10-100)
- [ ] Maximum page size is enforced

## Authentication and Authorization

- [ ] All non-public endpoints require authentication
- [ ] Auth token is passed in Authorization header (not query string)
- [ ] Endpoints enforce authorization (user can only access their own resources)
- [ ] Rate limiting is applied to authentication endpoints
- [ ] Rate limiting is applied to public endpoints

## Documentation and Discoverability

- [ ] OpenAPI/Swagger spec exists and matches implementation
- [ ] Endpoints have descriptions and example requests/responses
- [ ] Breaking changes follow a deprecation process
