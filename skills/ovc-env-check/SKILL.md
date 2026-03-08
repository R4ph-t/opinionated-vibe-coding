---
name: ovc-env-check
description: Audit environment variable usage and configuration hygiene. Finds undocumented vars, missing .env.example, hardcoded values, and missing validation. Use when the user wants to check their config setup, find missing env vars, or review secrets handling. Also trigger on "env check", "environment check", "check my config", "missing env vars", "secrets check", "config audit".
metadata:
  author: renderinc
  version: "1.0"
license: MIT
---

# OVC Environment Check

Audit how the project handles configuration and environment variables. Misconfigured environments are a top cause of deployment failures and security incidents.

## Steps

### 1. Find all environment variable references

Search the codebase for environment variable access patterns:

- JavaScript/TypeScript: `process.env.`, `import.meta.env.`
- Python: `os.environ`, `os.getenv`, `settings.` (Django), pydantic-settings fields
- Go: `os.Getenv`, `viper.Get`, `envconfig`
- Rust: `std::env::var`, `env::var`
- Ruby: `ENV[`, `ENV.fetch`
- C#: `Environment.GetEnvironmentVariable`, `IConfiguration`, `IOptions<T>`

Build a list of every environment variable name used in the code.

### 2. Check documentation

Look for environment variable documentation:
- `.env.example` or `.env.sample` at the project root
- A "Configuration" or "Environment Variables" section in README
- Comments in Docker Compose or deployment config files

For each variable found in step 1, check if it is documented. Flag undocumented variables.

### 3. Check for hardcoded values

Search for values that should be environment variables:
- URLs with `http://` or `https://` (API endpoints, service URLs)
- Port numbers in code (not in config files)
- Email addresses used as sender/recipient defaults
- File paths that are environment-specific
- Feature flags set as constants

Exclude: URLs in documentation, test fixtures, and comments.

### 4. Check configuration validation

Determine if the project validates its configuration at startup:
- Is there a central config module/file that loads and validates all env vars?
- Are required variables checked for presence before the application starts?
- Are values validated for format (e.g., URLs are valid, ports are numbers)?
- Do missing required variables produce a clear error message?

### 5. Check secret safety

- Is `.env` in `.gitignore`?
- Are there any `.env` files committed to the repository (other than `.env.example`)?
- Are secrets passed as environment variables rather than hardcoded?
- Are there any secrets in `docker-compose.yml`, CI config, or deployment manifests that should be references to secrets managers?

### 6. Report findings

Organize by category:

**Undocumented variables**
Variables used in code but missing from `.env.example` or documentation.

**Missing .env.example**
If no example file exists, recommend creating one with all required variables and placeholder values.

**Hardcoded values**
Values in code that should be configurable. Include file and line number.

**No startup validation**
If the project does not validate configuration at startup, recommend adding a validation step with the project's validation library (Zod, Pydantic, etc.).

**Secret safety issues**
Any committed secrets, missing `.gitignore` entries, or secrets in plaintext config files.

For each finding, include severity (CRITICAL for committed secrets, HIGH for missing validation, MEDIUM for undocumented vars, LOW for hardcoded non-sensitive values) and a specific fix.
