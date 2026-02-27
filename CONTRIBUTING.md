# Contributing

Contributions welcome. This repo is community-maintained and benefits from diverse perspectives, stacks, and workflows.

## What we're looking for

- **New skills** that implement opinionated vibe coding principles (audits, reviews, setup automation)
- **Stack-specific rules** for languages and frameworks not yet covered
- **Linter configs** for additional tools
- **Improvements to existing content** (clearer wording, better examples, coverage gaps)
- **Guides** that help developers work more effectively with AI coding agents

## How to contribute

1. Fork the repo.
2. Create a branch for your change.
3. Make your changes (see structure guidelines below).
4. Open a pull request with a clear description of *what* you changed and *why*.

## Structure guidelines

### Skills

Every skill lives in `skills/ovc-<name>/` and must include a `SKILL.md` file following the [Agent Skills spec](https://agentskills.io/specification).

The SKILL.md frontmatter must include:
- `name`: the skill identifier (e.g., `ovc-audit`)
- `description`: what it does and when to trigger it, including trigger phrases
- `metadata.author`: your name or organization
- `metadata.version`: semver string
- `license`: MIT (to match the repo)

Reference files go in a `references/` subdirectory within the skill folder.

### Rules

Rules files in `rules/` should all contain the same core content, adapted for each agent's format. If you modify one, update all of them:
- `CLAUDE.md` (Claude Code)
- `.cursorrules` (Cursor)
- `copilot-instructions.md` (GitHub Copilot)
- `.windsurfrules` (Windsurf)
- `AGENTS.md` (Codex)

### Stack references

Stack-specific rules go in `skills/ovc-setup/references/` as `stack-<language>.md`. Follow the pattern established by `stack-node.md` and `stack-python.md`:
- Language-specific idioms and conventions
- Error handling patterns
- Import/module organization
- Common packages to prefer

### Linter configs

Linter configurations go in `linters/`. Include a comment at the top explaining what the config is for and how to use it.

### Guides

Guides go in `guides/`. These are human-facing (not agent-facing) and should be practical and opinionated. Explain the *why*, not just the *what*.

## Style

- Be direct. Don't hedge with "you might want to consider" when you mean "do this."
- Explain *why* a rule exists, not just what it is. The repo is educational, not just a toolbox.
- Keep it practical. Real examples over abstract principles.
- No fluff. If a sentence doesn't add information, cut it.

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
