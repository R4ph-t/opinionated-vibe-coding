# Opinionated Vibe Coding

_A method for developers who give a sh!t._

Agent rules and skills that implement the [Opinionated Vibe Coding](https://render.com/blog/opinionated-vibe-coding) methodology. Built on the [Agent Skills](https://agentskills.io) spec.

## What is this?

Three layers that make your AI coding agent more disciplined without slowing it down:

**Rules** set always-on behavior: plan before coding, don't reinvent the wheel, read the docs, flag security issues, don't duplicate logic.

**Skills** are on-demand actions: audit the codebase, score it against OVC principles, run a security review, check for DRY violations, review a plan before execution, audit dependencies, set up MCP servers.

**Guides** are human-side references: context window management, model selection, knowing when to take the wheel.

Based on [twelve principles](https://render.com/blog/opinionated-vibe-coding) for getting better output from AI-assisted development.

## Quick start

### 1. Set up rules

Run the setup skill (`"set up OVC"`), or manually copy the rules file for your agent into your project root:

| Agent           | File                            | Destination      |
| --------------- | ------------------------------- | ---------------- |
| Claude Code     | `rules/CLAUDE.md`               | Project root     |
| Cursor          | `rules/cursor/*.mdc`            | `.cursor/rules/` |
| Cursor (legacy) | `rules/.cursorrules`            | Project root     |
| GitHub Copilot  | `rules/copilot-instructions.md` | Project root     |
| Windsurf        | `rules/.windsurfrules`          | Project root     |
| Codex           | `rules/AGENTS.md`               | Project root     |

**Cursor users:** Copy the `.mdc` files from `rules/cursor/` into your project's `.cursor/rules/` directory. The core and project rules always apply; the stack-specific rules (`ovc-stack-node.mdc`, `ovc-stack-python.mdc`, etc.) activate automatically when you're working on matching file types. Only keep the stack files you need.

Then customize `ovc-project.mdc` (or the `[CUSTOMIZE]` sections in single-file formats) for your project.

### 2. Install skills

Copy the `skills/` directory into your project. Each skill follows the [Agent Skills spec](https://agentskills.io/specification) and works with any compatible agent.

### 3. Use them

- Start a task normally. The rules shape agent behavior automatically.
- Say "audit this codebase" to trigger `ovc-audit`.
- Say "security review" to trigger `ovc-security-review`.
- Say "check for duplication" to trigger `ovc-dry-check`.
- Say "review this plan" to trigger `ovc-plan-review`.
- Say "check my dependencies" to trigger `ovc-dependency-check`.
- Say "score this repo" to trigger `ovc-scorecard`.
- Say "set up MCP" to trigger `ovc-mcp-setup`.
- Say "check error handling" to trigger `ovc-error-handling`.
- Say "test coverage gaps" to trigger `ovc-test-coverage`.
- Say "performance check" to trigger `ovc-performance-check`.
- Say "review my API" to trigger `ovc-api-review`.
- Say "env check" to trigger `ovc-env-check`.
- Say "accessibility review" to trigger `ovc-accessibility-review`.

## Repo structure

```
opinionated-vibe-coding/
├── rules/                          # Always-on agent behavior
│   ├── CLAUDE.md                   # Claude Code
│   ├── cursor/                     # Cursor (split .mdc rules)
│   │   ├── ovc-core.mdc           #   Core rules (always apply)
│   │   ├── ovc-project.mdc        #   Project-specific (customize this)
│   │   ├── ovc-stack-node.mdc     #   Node.js/TypeScript (globs: *.ts, *.js)
│   │   ├── ovc-stack-python.mdc   #   Python (globs: *.py)
│   │   ├── ovc-stack-go.mdc       #   Go (globs: *.go)
│   │   ├── ovc-stack-rust.mdc     #   Rust (globs: *.rs)
│   │   ├── ovc-stack-react.mdc    #   React/Frontend (globs: *.tsx, *.jsx)
│   │   ├── ovc-stack-ruby.mdc     #   Ruby (globs: *.rb, *.erb)
│   │   ├── ovc-stack-jvm.mdc      #   Java/Kotlin (globs: *.java, *.kt)
│   │   ├── ovc-stack-dotnet.mdc   #   C#/.NET (globs: *.cs, *.csproj)
│   │   └── ovc-stack-swift.mdc    #   Swift (globs: *.swift)
│   ├── .cursorrules                # Cursor (legacy single file)
│   ├── copilot-instructions.md     # GitHub Copilot
│   ├── .windsurfrules              # Windsurf
│   └── AGENTS.md                   # Codex
│
├── skills/                         # On-demand actions (Agent Skills spec)
│   ├── ovc-setup/                  # Bootstrap OVC in a project
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── rules-base.md       # Base rules template
│   │       ├── stack-node.md       # Node.js/TypeScript rules
│   │       ├── stack-python.md     # Python rules
│   │       ├── stack-go.md         # Go rules
│   │       ├── stack-rust.md       # Rust rules
│   │       ├── stack-react.md      # React/Frontend rules
│   │       ├── stack-ruby.md       # Ruby rules
│   │       ├── stack-jvm.md        # Java/Kotlin rules
│   │       ├── stack-dotnet.md     # C#/.NET rules
│   │       └── stack-swift.md      # Swift rules
│   │
│   ├── ovc-audit/                  # Full codebase audit
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── audit-checklist.md
│   │
│   ├── ovc-security-review/        # Security-focused review
│   │   └── SKILL.md
│   │
│   ├── ovc-dry-check/              # Find duplication and DRY violations
│   │   └── SKILL.md
│   │
│   ├── ovc-plan-review/            # Review approach before coding
│   │   └── SKILL.md
│   │
│   ├── ovc-dependency-check/       # Audit dependencies
│   │   └── SKILL.md
│   │
│   ├── ovc-scorecard/              # Rate repo against OVC principles
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── scoring-rubric.md
│   │
│   ├── ovc-mcp-setup/             # Set up MCP servers
│   │   └── SKILL.md
│   │
│   ├── ovc-file-split/            # Split large files
│   │   └── SKILL.md
│   │
│   ├── ovc-error-handling/        # Audit error handling patterns
│   │   └── SKILL.md
│   │
│   ├── ovc-test-coverage/         # Find test coverage gaps
│   │   └── SKILL.md
│   │
│   ├── ovc-performance-check/     # Find performance anti-patterns
│   │   └── SKILL.md
│   │
│   ├── ovc-api-review/            # Review API design
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── api-checklist.md
│   │
│   ├── ovc-env-check/             # Audit environment config
│   │   └── SKILL.md
│   │
│   └── ovc-accessibility-review/  # Review frontend accessibility
│       └── SKILL.md
│
├── linters/                        # Opinionated linter configs
│   ├── biome.json                  # JavaScript/TypeScript (Biome)
│   └── ruff.toml                   # Python (Ruff)
│
└── guides/                         # Human-side guides
    ├── context-window-management.md
    ├── model-selection.md
    ├── when-to-take-the-wheel.md
    ├── prompt-engineering.md
    └── code-review-with-agents.md
```

## The Twelve Principles

1. **The Vigilant Architect.** Own the architectural decisions, even if the agent helps you get there.
2. **Know your fundamentals.** Good architecture, DRY, security, separation of concerns.
3. **Know your assistant.** Understand model capabilities, context windows, when to switch.
4. **Agents will reinvent the wheel.** Push back. Use existing packages.
5. **RTFM.** Make the agent read the docs before implementing.
6. **No regress.** Tests catch what the agent breaks.
7. **Your project, your rules.** Encode your opinions into rules files and linters.
8. **Plan, plan, plan.** Outline the approach before writing code.
9. **Audit often.** Fresh eyes on a fresh agent, regularly.
10. **Agent upgraded.** MCP servers, tools, documentation access.
11. **Sometimes, just take the wheel.** Know when a decision should be yours.
12. **Ship it.** Be deliberate about your tooling from start to finish.

Read the full methodology: [Opinionated Vibe Coding](https://render.com/blog/opinionated-vibe-coding)

## Customization

The rules files are starting points. The whole philosophy is that YOUR opinions matter. Fork this, modify it, make it yours. The `[CUSTOMIZE]` sections are where your project-specific decisions go.

## Contributing

Contributions welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines. Whether it's a new skill, a stack-specific rules template, or a better audit checklist: every addition should include a brief explanation of _why_, not just _what_.

## License

[MIT](LICENSE)
