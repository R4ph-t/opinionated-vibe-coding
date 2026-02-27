---
name: ovc-scorecard
description: Rate a repository against Opinionated Vibe Coding principles. Produces a quantitative scorecard with 1-5 scores across eight categories and an overall grade. Use when the user wants a quick health check, wants to know where they stand, or wants to track improvement over time. Also trigger on "rate this repo", "score my codebase", "OVC scorecard", "how does my project measure up", "grade this project".
metadata:
  author: renderinc
  version: "1.0"
license: MIT
---

# OVC Scorecard

Rate a repository against the Opinionated Vibe Coding principles. Unlike the full `ovc-audit` (which produces a detailed qualitative report), the scorecard is a quick quantitative check: scores per category, an overall grade, and the top actions to improve.

Run it periodically to track progress. Run it on a new codebase to get a fast read on its health.

## Scoring categories

Eight categories, each mapped to the OVC principles they reflect:

| Category | OVC Principles | What to evaluate |
|---|---|---|
| Architecture | 1 (Vigilant Architect), 2 (Fundamentals) | Clear structure, separation of concerns, consistent patterns, no competing approaches |
| Code Quality | 2 (Fundamentals) | DRY, naming consistency, no duplication, readable structure |
| Security | 2 (Fundamentals) | No hardcoded secrets, input validation, auth on endpoints, parameterized queries, secure defaults |
| Dependencies | 4 (Don't Reinvent), 5 (RTFM) | Uses established packages, no hand-rolled solutions, correct API usage, no deprecated methods |
| Testing | 6 (No Regress) | Tests exist, critical paths covered, tests assert real behavior, suite passes |
| Tooling | 7 (Your Rules), 10 (Agent Upgraded) | Rules file present, linter configured, strict type checking, MCP or tool integrations |
| Error Handling | 2 (Fundamentals) | Consistent pattern, no swallowed errors, proper status codes, context in messages |
| Deployment Readiness | 12 (Ship It) | Env var handling, config management, Dockerfile/IaC/CI present, no secrets in repo |

Principles 3 (Know Your Assistant), 8 (Plan), 9 (Audit Often), and 11 (Take the Wheel) are process/behavior principles that can't be scored from code alone. Note whether the repo has tooling support for them (rules file, audit skills, plan review) but do not score them numerically.

## How to score

### 1. Scan the project

Start by understanding the project:
- Read the root directory listing and identify the tech stack
- Read any rules files, linter configs, CI configs, Dockerfiles
- Read the main entry point, key route/handler files, and test files
- Get a sense of the project's size and complexity

### 2. Score each category

Use the rubric in [references/scoring-rubric.md](references/scoring-rubric.md). For each category:
- Examine the relevant parts of the codebase
- Assign a score from 1 to 5 based on the rubric criteria
- Write 2-3 sentences justifying the score with specific observations

Be honest. A 3 is average, not bad. A 5 means genuinely solid, not just "no obvious problems." Most real codebases land between 2 and 4 on most categories.

### 3. Calculate the overall grade

Average the eight category scores and map to a letter grade:

| Average | Grade |
|---|---|
| 4.5 - 5.0 | A |
| 3.5 - 4.4 | B |
| 2.5 - 3.4 | C |
| 1.5 - 2.4 | D |
| 1.0 - 1.4 | F |

### 4. Identify the top 3 actions

Pick the three changes that would most improve the score. Prioritize by:
1. Impact on score (which low-scoring categories can move the most?)
2. Effort required (quick wins first)
3. Risk reduction (security and testing improvements outweigh cosmetic ones)

### 5. Output the scorecard

Use this exact format:

```
# OVC Scorecard

## Overall: [GRADE] ([AVERAGE]/5)

| Category | Score | |
|---|---|---|
| Architecture | [N]/5 | [BAR] |
| Code Quality | [N]/5 | [BAR] |
| Security | [N]/5 | [BAR] |
| Dependencies | [N]/5 | [BAR] |
| Testing | [N]/5 | [BAR] |
| Tooling | [N]/5 | [BAR] |
| Error Handling | [N]/5 | [BAR] |
| Deployment | [N]/5 | [BAR] |

## Category Details

### Architecture: [N]/5
[2-3 sentence justification with specific observations from the codebase]

### Code Quality: [N]/5
[2-3 sentence justification]

### Security: [N]/5
[2-3 sentence justification]

### Dependencies: [N]/5
[2-3 sentence justification]

### Testing: [N]/5
[2-3 sentence justification]

### Tooling: [N]/5
[2-3 sentence justification]

### Error Handling: [N]/5
[2-3 sentence justification]

### Deployment Readiness: [N]/5
[2-3 sentence justification]

## Process Principle Support
[Note which process principles (3, 8, 9, 11) have tooling support and which don't]

## Top 3 Actions to Improve Your Score
1. [Most impactful change, expected score impact]
2. [Second most impactful]
3. [Third most impactful]
```

For the bar visualization, use filled and empty blocks proportional to the score:
- 1/5: `██░░░░░░░░`
- 2/5: `████░░░░░░`
- 3/5: `██████░░░░`
- 4/5: `████████░░`
- 5/5: `██████████`
