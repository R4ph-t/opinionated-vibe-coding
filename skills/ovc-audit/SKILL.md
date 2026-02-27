---
name: ovc-audit
description: Run a full codebase audit following Opinionated Vibe Coding principles. Checks architecture, DRY violations, security issues, test coverage, dependency health, and accidental decisions. Use when the user asks to audit their code, review codebase health, check for tech debt, or do a project review. Also trigger on "OVC audit", "codebase review", "what's wrong with my code", "tech debt check".
metadata:
  author: renderinc
  version: "1.0"
license: MIT
---

# OVC Codebase Audit

Run a comprehensive audit of the codebase. This skill is designed to be run with fresh eyes, ideally in a new conversation with no prior context about the project.

The goal is to surface issues that accumulate invisibly during AI-assisted development: accidental architectural decisions, duplicated logic, security gaps, missing tests, and dependency problems.

## How to run the audit

### 1. Scan the project structure

Start by understanding the project layout:
- Read the project root directory listing
- Identify the tech stack (package.json, requirements.txt, etc.)
- Read any existing rules files (CLAUDE.md, .cursorrules, etc.)
- Read the main entry point and configuration files

### 2. Check each area

Work through the checklist in [references/audit-checklist.md](references/audit-checklist.md). For each area:
- Examine the relevant files
- Note specific issues with file paths and line numbers
- Rate severity: CRITICAL, HIGH, MEDIUM, LOW

### 3. Generate the report

Structure the output as:

```
## Audit Summary
[One paragraph overview of codebase health]

## Critical Issues
[Issues that need immediate attention]

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

## What Looks Accidental
[Decisions that appear to have been made by default rather than intentionally]

## Recommendations
[Top 5 things to fix first, in priority order]
```

### 4. Be direct

Do not soften findings. Do not add qualifiers like "this might be intentional" unless there's clear evidence it was. The value of this audit is honesty.
