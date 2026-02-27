---
name: ovc-setup
description: Set up Opinionated Vibe Coding in a project. Use when the user wants to initialize OVC, add agent rules, configure linters, or bootstrap their project with the OVC methodology. Triggers on phrases like "set up OVC", "add opinionated vibe coding", "configure agent rules", "initialize project rules".
metadata:
  author: renderinc
  version: "1.0"
license: MIT
---

# OVC Setup

Bootstrap a project with Opinionated Vibe Coding rules, linter configs, and agent skills.

## What this does

1. Detects the project's tech stack (language, framework, package manager)
2. Detects which AI coding agent is being used
3. Generates the appropriate rules file (CLAUDE.md, .cursorrules, etc.) with project-specific customizations
4. Optionally installs and configures an opinionated linter (Biome or ESLint)
5. Installs the other OVC skills (audit, security review, etc.)

## Steps

### 1. Detect the environment

Examine the project root for:
- `package.json` (Node.js ecosystem: check for framework in dependencies)
- `requirements.txt` / `pyproject.toml` / `setup.py` (Python ecosystem)
- `go.mod` (Go)
- `Cargo.toml` (Rust)
- Existing rules files (CLAUDE.md, .cursorrules, etc.)
- Existing linter configs (biome.json, .eslintrc, etc.)

### 2. Ask what to set up

Present the user with what you detected and ask:
- Confirm the tech stack
- Which rules file format to generate (or auto-detect from agent)
- Whether to set up a linter
- Whether to install OVC skills

### 3. Generate the rules file

Use the base template from [references/rules-base.md](references/rules-base.md) and customize it:
- Fill in the tech stack section with detected technologies
- Fill in file structure based on the actual project layout
- Fill in naming conventions based on existing code patterns
- Add framework-specific rules from the appropriate reference file

Available stack references:
- [references/stack-node.md](references/stack-node.md) for Node.js/TypeScript projects
- [references/stack-python.md](references/stack-python.md) for Python projects
- [references/stack-go.md](references/stack-go.md) for Go projects
- [references/stack-rust.md](references/stack-rust.md) for Rust projects

### 4. Configure the linter (if requested)

For JavaScript/TypeScript projects:
- Prefer Biome over ESLint (faster, less config)
- Install with the project's package manager
- Use the opinionated config from the repo's `linters/biome.json`

For Python projects:
- Use Ruff (fast, replaces flake8 + isort + pyupgrade + bandit in one tool)
- Install with `pip install ruff` or add to dev dependencies
- Use the opinionated config from the repo's `linters/ruff.toml`

### 5. Confirm with the user

Show a summary of what was created/modified and ask for approval before writing any files.
