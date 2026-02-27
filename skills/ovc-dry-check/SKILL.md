---
name: ovc-dry-check
description: Find DRY violations and duplicated logic in the codebase. Identifies copy-pasted code, repeated patterns, multiple implementations of the same functionality, and opportunities to extract shared modules. Use when the user asks to check for duplication, find repeated code, clean up the codebase, or refactor. Also trigger on "DRY check", "find duplicates", "duplicated code", "refactor for reuse".
metadata:
  author: renderinc
  version: "1.0"
license: MIT
---

# OVC DRY Check

Scan the codebase for duplicated logic and DRY violations. This is especially important in AI-assisted codebases where agents frequently duplicate logic across files.

## What to look for

### Exact or near-exact duplicates
- Code blocks that are identical or differ only in variable names
- Copy-pasted functions with minor modifications
- Repeated configuration blocks

### Pattern duplication
- Multiple files implementing the same pattern (e.g., similar API route handlers with the same error handling, validation, response structure)
- Repeated utility logic that should be extracted (string formatting, date manipulation, data transformation)
- Similar database queries that could use a shared query builder or repository pattern

### Configuration duplication
- Same values hardcoded in multiple places
- Environment variable access scattered instead of centralized
- Repeated schema definitions or type declarations

### Cross-file concerns
- Error handling patterns repeated across routes/handlers
- Authentication/authorization checks duplicated instead of using middleware
- Validation logic repeated instead of using shared schemas

## Steps

### 1. Scan the codebase structure
Get a high-level view of the project. Identify clusters of similar files (route handlers, models, services, utilities).

### 2. Compare similar files
Within each cluster, compare files for duplicated patterns. Focus on files that serve the same role (e.g., all API route files, all service files).

### 3. Check utilities
Look for utility functions that duplicate logic available in existing packages or in other utility files.

### 4. Report findings

For each violation:
- The duplicated code/pattern, with file paths and line numbers for each instance
- What a refactored version would look like (extract to shared module, use middleware, create base class, etc.)
- Severity: how much maintenance risk does this duplication create?

Group by category. Prioritize violations that create the most maintenance risk.
