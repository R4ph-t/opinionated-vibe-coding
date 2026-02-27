# Model Selection

Not all models are interchangeable. Choosing the right model for the task -- and knowing when to switch -- is a skill that most developers never think about. They use whatever their IDE defaults to and blame "AI" when output quality varies.

## When to use what

### Heavy hitters (Claude Sonnet/Opus, GPT-4o, Gemini 2.5 Pro)

Use the most capable model available when:

- **Planning architecture.** You need the model to reason about trade-offs, consider multiple approaches, and produce a coherent design.
- **Complex refactors.** Changes that touch many files with interconnected logic require the model to hold a lot of state.
- **Debugging subtle issues.** Race conditions, edge cases, logic errors that span multiple functions.
- **Greenfield decisions.** When you're starting from scratch and the model's choices will compound.
- **Security-sensitive code.** Auth flows, encryption, input validation where nuance matters.

### Fast models (Claude Haiku, GPT-4o-mini, Gemini Flash)

Use a smaller, faster model when:

- **Boilerplate generation.** CRUD endpoints, test scaffolding, type definitions.
- **Straightforward implementation.** The architecture is decided, you just need it typed out.
- **Simple bug fixes.** Obvious errors with clear fixes.
- **Documentation and comments.** Writing docstrings, README updates, changelog entries.
- **Formatting and cleanup.** Renaming variables, reorganizing imports, applying consistent style.

The cost difference is significant. Using an expensive model for boilerplate is a waste. Using a cheap model for architecture is a false economy.

## When to switch models mid-session

This is the underrated move. Most developers never do it. You should when:

- **The agent is stuck in a loop.** It keeps trying the same approach, rephrasing slightly. Switching to a different model often breaks the loop because the new model has different default patterns.
- **Quality dropped after a long conversation.** Rather than fighting degradation, start fresh with the same or different model.
- **The task changed complexity.** You planned with a heavy model, now you're implementing boilerplate. Switch down. You hit a tricky edge case during boilerplate. Switch up.
- **You need a second opinion.** Different models have different blind spots. If one model's output feels off, ask another to review it.

## Model strengths and blind spots

These are practical observations, not benchmarks. Your mileage varies by task.

### Claude (Sonnet/Opus)
- **Strong at:** Following complex, multi-part instructions. Maintaining consistency across long outputs. Respecting rules files and constraints. Refactoring with awareness of broader context.
- **Watch for:** Can be overly cautious, asking for confirmation when you want it to just proceed. Sometimes adds unnecessary abstractions.

### GPT-4o
- **Strong at:** Generating working code quickly. Broad knowledge of frameworks and APIs. Good at pattern-matching to common solutions.
- **Watch for:** Tends to reach for popular patterns even when they're not the best fit. More likely to hallucinate specific API details. Can be verbose in explanations.

### Gemini 2.5 Pro
- **Strong at:** Processing large amounts of code or documentation at once. Codebase-wide analysis. Understanding complex dependency graphs.
- **Watch for:** Can be less precise on fine-grained implementation details. Instruction following can be less strict than Claude.

## Practical setup

Most modern coding agents let you switch models per-conversation or per-message. Set this up:

- **Cursor:** Model selector in the chat panel. You can switch mid-conversation.
- **Claude Code:** Set `ANTHROPIC_MODEL` env var, or use `/model` to switch.
- **GitHub Copilot:** Model selector in the chat panel.
- **Codex:** Configure in the settings or per-session.

Keep a mental model of where you are on the cost/capability spectrum and adjust as the task demands. The best developers using AI tools are constantly adjusting this dial.

## The "switch models" debugging technique

When you've asked the same model to fix something three times and it keeps failing:

1. Copy the current state of the problem (error message, relevant code, what you've tried).
2. Open a new session with a different model.
3. Paste the context and ask for a fresh approach.

Different models get stuck in different ruts. A fresh model with fresh context solves problems that a degraded session never will.
