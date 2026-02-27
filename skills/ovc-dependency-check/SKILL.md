---
name: ovc-dependency-check
description: Audit project dependencies for outdated packages, known vulnerabilities, unused imports, and duplicated purpose. Use when the user asks to check dependencies, find outdated packages, audit for vulnerabilities, or clean up the dependency list. Also trigger on "dependency check", "npm audit", "outdated packages", "unused dependencies", "are my deps up to date".
metadata:
  author: renderinc
  version: "1.0"
license: MIT
---

# OVC Dependency Check

Audit the project's dependencies for health, security, and hygiene. AI-assisted codebases accumulate dependency problems faster than traditional ones because the agent adds packages without considering the existing dependency graph.

## Why this matters

Agents introduce dependencies in ways that create specific problems:
- They add a package for one function when the project already has a package that does the same thing.
- They reach for popular defaults (axios, lodash, moment) instead of checking what the project already uses or whether a native solution exists.
- They never run `npm audit` or `pip audit` after adding a package.
- They don't check whether a dependency is maintained, deprecated, or has known CVEs.

## Steps

### 1. Identify the stack and dependency files

Look for:
- `package.json` + `package-lock.json` / `yarn.lock` / `pnpm-lock.yaml` (Node.js)
- `requirements.txt` / `pyproject.toml` / `Pipfile` / `poetry.lock` (Python)
- `go.mod` + `go.sum` (Go)
- `Cargo.toml` + `Cargo.lock` (Rust)

### 2. Check for vulnerabilities

Run the appropriate audit command:
- Node.js: `npm audit` or `pnpm audit`
- Python: `pip audit` (requires `pip-audit` package)
- Go: `govulncheck ./...`
- Rust: `cargo audit`

Report each vulnerability with severity, affected package, and fix (usually a version bump).

### 3. Check for outdated dependencies

Run the appropriate outdated check:
- Node.js: `npm outdated` or `pnpm outdated`
- Python: `pip list --outdated`
- Go: `go list -u -m all`
- Rust: `cargo outdated` (requires `cargo-outdated`)

Flag any dependency more than one major version behind. Note dependencies with no updates in 12+ months as potentially unmaintained.

### 4. Check for unused dependencies

Scan the codebase for actual import/require/use statements and compare against the dependency list:
- Are there packages listed in dependencies that are never imported?
- Are there packages in `dependencies` that should be in `devDependencies` (or vice versa)?
- Are there test-only packages in production dependencies?

For Node.js, tools like `depcheck` can help automate this. For other stacks, search the codebase for import statements.

### 5. Check for duplicated purpose

Look for multiple packages that solve the same problem:
- Two HTTP clients (e.g., both `axios` and `node-fetch`)
- Two validation libraries (e.g., both `zod` and `joi`)
- Two date libraries (e.g., both `date-fns` and `dayjs`)
- Two testing frameworks
- Two logging libraries

When found, recommend consolidating on one. Prefer the one already more widely used in the codebase.

### 6. Check for packages that should be replaced

Flag packages that have better alternatives:
- `moment` (deprecated, use `date-fns` or `dayjs`)
- `request` (deprecated, use native `fetch` or `undici`)
- `lodash` for functions available natively in modern JS
- Any package with a CVE and no patch available

### 7. Generate the report

```
## Dependency Health Summary
[One paragraph overview]

## Vulnerabilities
[CVEs and security issues, sorted by severity]

## Outdated
[Packages behind latest, grouped by severity of version gap]

## Unused
[Packages installed but not imported]

## Duplicated Purpose
[Multiple packages solving the same problem]

## Recommended Replacements
[Deprecated or problematic packages with suggested alternatives]

## Action Items
[Prioritized list of what to fix first]
```

Be specific. Include exact commands to fix issues where possible (e.g., `npm install package@latest`).
