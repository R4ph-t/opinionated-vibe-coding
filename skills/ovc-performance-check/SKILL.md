---
name: ovc-performance-check
description: Find common performance anti-patterns in the codebase. Checks for N+1 queries, missing pagination, unbounded fetches, large bundle imports, blocking I/O, and missing indexes. Use when the user wants to optimize performance, find bottlenecks, or review efficiency. Also trigger on "performance check", "find bottlenecks", "performance review", "is this efficient", "slow", "optimize".
metadata:
  author: renderinc
  version: "1.0"
license: MIT
---

# OVC Performance Check

Find performance anti-patterns through static analysis of the codebase. This is not a profiler; it catches the structural issues that cause performance problems before they reach production.

## Categories

### Database
- **N+1 queries:** Loops that execute a query per iteration instead of batching. Look for ORM calls inside `for`/`forEach`/`map` loops.
- **Missing pagination:** Queries that return all rows without LIMIT/OFFSET or cursor-based pagination.
- **Unbounded queries:** SELECT without WHERE on large tables.
- **Missing indexes:** Columns used in WHERE, JOIN, or ORDER BY clauses without corresponding indexes (check migration files and schema).
- **Unnecessary eager loading:** Loading all associations when only the parent record is needed.
- **SELECT \*:** Fetching all columns when only a few are needed, especially on tables with large text/blob columns.

### API
- **Missing pagination:** List endpoints that return all records without pagination support.
- **Over-fetching:** Returning full objects when the client only needs a subset of fields.
- **No caching headers:** Responses for stable data missing `Cache-Control`, `ETag`, or `Last-Modified`.
- **Missing compression:** No gzip/brotli on text-heavy responses.
- **Chatty APIs:** Multiple sequential API calls that could be combined into one.

### Frontend
- **Large bundle imports:** Importing entire libraries when only a single function is needed (`import _ from 'lodash'` vs `import groupBy from 'lodash/groupBy'`).
- **Missing lazy loading:** Heavy routes or components loaded eagerly on initial page load.
- **Unoptimized images:** Images without width/height, missing `loading="lazy"`, no srcset for responsive sizes, large images not using modern formats.
- **Unnecessary re-renders:** State updates that trigger re-renders of large component trees. Missing memoization on expensive derived data.
- **Bundle size:** Large dependencies that have smaller alternatives.

### Async and I/O
- **Blocking the event loop:** Synchronous file I/O, CPU-intensive computation, or `JSON.parse` on large payloads in request handlers.
- **Missing connection pooling:** Creating new database or HTTP connections per request instead of using a pool.
- **Sequential when parallel is possible:** `await a(); await b()` when `a` and `b` are independent and could use `Promise.all`.
- **Missing timeouts:** External HTTP calls or database queries without timeouts.

## Steps

### 1. Identify the stack

Detect the framework, database, ORM, and frontend setup. This determines which categories to focus on.

### 2. Scan by category

Work through each relevant category above. Skip categories that don't apply (e.g., skip Frontend for a pure API project).

### 3. Report findings

For each issue:
- File and line number
- Category (Database, API, Frontend, Async)
- Estimated impact: HIGH (likely causes user-visible latency or scaling issues), MEDIUM (will matter at moderate load), LOW (micro-optimization, fix if convenient)
- What the anti-pattern is
- How to fix it

Sort by estimated impact. Be honest about what's speculative vs. what's clearly a problem. Static analysis can identify structural issues but cannot measure actual latency. Recommend profiling for issues where the real impact is uncertain.
