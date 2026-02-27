# Context Window Management

Your AI coding agent has a finite context window. Everything in the conversation -- your prompts, the agent's responses, code it reads, tool outputs -- consumes tokens from that window. When you exceed it, the model silently drops earlier context, and output quality degrades in ways that are hard to spot.

Managing your context window is one of the highest-leverage habits you can build.

## How context windows work

Every model has a maximum token limit. Tokens are roughly 3-4 characters of English text. Code is less efficient: a 100-line file might be 500-1500 tokens depending on verbosity.

| Model | Context window | Practical limit |
|---|---|---|
| Claude Sonnet 4 | 200K tokens | ~120K before noticeable degradation |
| GPT-4o | 128K tokens | ~80K before noticeable degradation |
| Gemini 2.5 Pro | 1M tokens | ~500K before noticeable degradation |

The "practical limit" matters more than the maximum. Models don't fall off a cliff at the edge of their context window -- they degrade gradually. Attention to earlier parts of the conversation weakens. Instructions you gave at the start get fuzzy. The agent starts contradicting decisions it made 20 messages ago.

## Signs your context is degrading

Watch for these signals:

- **The agent repeats work it already did.** It reimplements a function it wrote 10 messages ago, or re-reads a file it already analyzed.
- **Earlier instructions stop being followed.** You said "use Zod for validation" at the start, and now it's writing manual validation logic.
- **Architectural drift within a session.** The code style or patterns shift partway through the conversation.
- **Confident hallucinations increase.** The agent starts asserting things about your codebase that aren't true.
- **It loses track of file state.** It references code that it already changed, or forgets modifications it made.

## When to start fresh

Start a new session when:

- The conversation exceeds ~50 back-and-forth exchanges
- You're switching to a fundamentally different task
- The agent starts showing degradation signals
- You've completed a logical unit of work (feature, fix, refactor)

Before starting fresh, capture the state: commit your changes, note any open decisions, and write a brief summary you can paste into the new session as context.

## Strategies for staying within limits

### Front-load important context
Put your rules file, architectural decisions, and critical constraints at the top of the conversation. The model pays strongest attention to the beginning and end of its context.

### Be selective about what you paste in
Don't dump an entire file when the agent only needs to see one function. Don't paste full error logs when the relevant error is three lines.

### Use your agent's tools, not manual pasting
If your agent can read files, search code, or access documentation through tools, let it pull what it needs rather than you pasting everything upfront. Tool results are more targeted and waste less context.

### Summarize before continuing
If you're deep into a long conversation and need to keep going, ask the agent to summarize the current state: what's been done, what's in progress, what decisions were made. Then start a new session with that summary.

### Scope your sessions
One feature per session. One bug per session. One refactor per session. If a task naturally breaks into phases (plan, implement, test, review), each phase can be its own session. Resist the urge to keep adding "one more thing" to a long conversation.

## Rules file context cost

Your rules file is loaded into every session. Make it count, but keep it efficient.

- A well-written rules file is typically 100-200 lines (~500-1000 tokens). That's a negligible fraction of most context windows.
- Avoid verbose explanations in the rules file. Directives are cheaper than paragraphs.
- If your rules file grows past 300 lines, split project-specific rules into a separate file and only include what's relevant to each session.

## Model-specific notes

Different models handle long context differently:

- **Claude** maintains strong instruction-following across long contexts but can lose track of specific code details from early in the conversation.
- **GPT-4o** tends to lose earlier context more aggressively. Keep critical instructions closer to recent messages if the conversation is long.
- **Gemini** has the largest window by far but can struggle with precision on very long inputs. Useful for bulk analysis; less ideal for detailed multi-step implementation across a giant context.

These are generalizations. Test with your actual workload to calibrate.
