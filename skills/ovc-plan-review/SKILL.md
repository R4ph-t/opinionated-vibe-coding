---
name: ovc-plan-review
description: Review an agent's proposed plan or approach before code is written. Checks for scope creep, architectural fit, missing considerations, and unnecessary complexity. Use when the user has a plan they want reviewed, when an agent has proposed an approach, or before starting a significant piece of work. Also trigger on "review this plan", "check this approach", "does this make sense", "before I start".
metadata:
  author: renderinc
  version: "1.0"
license: MIT
---

# OVC Plan Review

Review a proposed plan or approach before any code is written. The goal is to catch problems when they're cheap to fix: before implementation.

## What to check

### Scope
- Does the plan match what was actually asked for?
- Is it doing more than necessary? Scope creep is the default failure mode.
- Is it missing anything that was requested?

### Architecture
- Does the proposed approach fit the existing codebase patterns?
- Is it introducing new patterns or abstractions that aren't necessary?
- Is there a simpler approach that achieves the same result?
- Will this be easy to modify or extend later?

### Dependencies
- Is the plan using existing packages where appropriate?
- Is it proposing to build something from scratch that a library handles?
- Are the proposed packages well-maintained and appropriate?
- Has the agent checked the documentation for the libraries it plans to use?

### Impact
- What existing code will be affected?
- Are there potential regressions?
- Does the plan account for edge cases?
- What could go wrong?

### Missing considerations
- Is testing included in the plan?
- Is error handling addressed?
- Are security implications considered?
- Will documentation or types need updating?

## How to respond

Be specific about what should change before proceeding. Don't just say "looks good" unless it actually does. The value of this review is catching the twelve decisions the agent was about to make silently.
