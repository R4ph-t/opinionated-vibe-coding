---
name: ovc-audit
description: Run a full codebase audit following Opinionated Vibe Coding principles. Checks architecture, DRY violations, security issues, test coverage, dependency health, error handling, environment config, file organization, and accidental decisions. Conditionally audits API design, frontend accessibility, and data layer. Use when the user asks to audit their code, review codebase health, check for tech debt, or do a project review. Also trigger on "OVC audit", "codebase review", "what's wrong with my code", "tech debt check".
metadata:
  author: r4ph-t
  version: "2.0"
license: MIT
---

# OVC Codebase Audit

Run a comprehensive audit of the codebase. This skill is designed to be run with fresh eyes, ideally in a new conversation with no prior context about the project.

The goal is to surface issues that accumulate invisibly during AI-assisted development: accidental architectural decisions, duplicated logic, security gaps, missing tests, and dependency problems.

For a quick quantitative score instead of a detailed report, use `ovc-scorecard`.

## How to run the audit

### 1. Scan the project structure

Start by understanding the project layout:
- Read the project root directory listing
- Identify the tech stack (package.json, requirements.txt, go.mod, etc.)
- Read any existing rules files (CLAUDE.md, .cursorrules, AGENTS.md, etc.)
- Read the main entry point and configuration files

### 2. Detect the stack

Determine which domain checklists to load based on what the project contains.

**Always load:**
- [references/audit-core.md](references/audit-core.md)

**Load if frontend detected** (React, Vue, Svelte, Next.js, Nuxt, SvelteKit, Astro, or `*.tsx`/`*.jsx`/`*.vue`/`*.svelte` files):
- [references/audit-frontend.md](references/audit-frontend.md)

**Load if API layer detected** (route definitions via Express, Hono, FastAPI, Flask, Django, Rails, Gin, etc., or `routes/`/`controllers/`/`api/` directories):
- [references/audit-api.md](references/audit-api.md)

**Load if database or ORM detected** (Prisma, Drizzle, SQLAlchemy, ActiveRecord, GORM, Diesel, Entity Framework, migration files, or `models/` directory):
- [references/audit-data.md](references/audit-data.md)

### 3. Scope the audit

Not every file needs to be read. Prioritize strategically:

**Always read in full:**
- Entry points (main.py, index.ts, app.py, etc.)
- Auth and middleware files
- Configuration and environment setup
- Package manifests and lock files (for dependency audit)
- Any existing rules or CI config files

**Spot-check (read a representative sample):**
- One or two route handlers or controllers
- One or two service or model files
- One or two test files

**For codebases with 50+ source files**, sample strategically rather than reading everything. Focus on the areas above and expand if specific patterns raise concerns.

### 4. Check each area

Work through each loaded checklist. For every finding, note:
- The specific file path and line number
- Severity: **CRITICAL**, **HIGH**, **MEDIUM**, or **LOW**
- A brief, concrete description of the problem

### 5. Generate the report

Structure the output as follows. Omit sections where no issues were found. Include domain-specific sections only if the corresponding checklist was loaded.

```
## Audit Summary
[One paragraph overview of codebase health]

## Critical Issues
[Issues that need immediate attention -- security vulnerabilities, data loss risks, broken auth]

## Architecture
[Architectural concerns, competing patterns, unclear boundaries]

## Code Quality
[DRY violations, hand-rolled solutions, naming issues]

## Security
[Security gaps, hardcoded secrets, missing validation]

## Testing
[Coverage gaps, missing regression tests, trivial tests]

## Dependencies
[Outdated, vulnerable, unused, or missing packages]

## Error Handling
[Swallowed errors, inconsistent patterns, missing context, leaked internals]

## Environment & Config
[Missing .env.example, hardcoded values, no startup validation, committed secrets]

## File Organization
[Oversized files, vague names, files with multiple responsibilities]

## API Design (if applicable)
[Naming inconsistencies, wrong HTTP methods, missing pagination, inconsistent error shapes]

## Data Layer (if applicable)
[N+1 queries, unbounded fetches, missing indexes, connection issues]

## Accessibility (if applicable)
[Missing alt text, unlabeled inputs, non-semantic markup, focus issues]

## What Looks Accidental
[Decisions that appear to have been made by default rather than intentionally]

## Recommendations
[Top 5 things to fix first, in priority order]

## Go Deeper
For any area where issues were found, these specialized skills provide
a more thorough analysis and actionable fixes:

- DRY violations: ovc-dry-check
- Test coverage gaps: ovc-test-coverage
- Error handling: ovc-error-handling
- Performance: ovc-performance-check
- API design: ovc-api-review
- Environment and config: ovc-env-check
- Accessibility: ovc-accessibility-review
- Large files: ovc-file-split
- Quick quantitative score: ovc-scorecard
```

### 6. Be direct

Do not soften findings. Do not add qualifiers like "this might be intentional" unless there is clear evidence it was. The value of this audit is honesty.
