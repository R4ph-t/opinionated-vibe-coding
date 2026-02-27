# When to Take the Wheel

The hardest skill in AI-assisted development isn't prompting. It's knowing when to stop prompting and start coding yourself.

The agent is fast. It's fluent. It produces code that looks right. And sometimes that fluency is exactly the problem: it moves with confidence in the wrong direction, and the speed makes it harder to notice.

This guide is about developing the instinct for when a decision should be yours, not the AI's.

## Signals that you should take over

### You can't explain what it did

If the agent produced code and you couldn't explain the approach to a colleague, that's the signal. It doesn't matter that it works. It doesn't matter that the tests pass. If you don't understand the code you're shipping, you can't debug it when it breaks and you can't build on it confidently.

This doesn't mean you need to understand every line. You trust abstraction layers all the time. But you should understand the *decisions* -- why this pattern, why this structure, why this library.

### The agent is looping

It tried one approach, it didn't work, it tried a variation, that didn't work, it tried a third variation that's basically the first one again. This is the model equivalent of hammering a nail with a screwdriver. It doesn't have the context or reasoning to find the actual fix.

When you see a loop: stop. Read the error yourself. Understand the problem yourself. Then either fix it manually or give the agent specific guidance on what the actual issue is.

### The change is architectural

Adding a new abstraction layer. Choosing a state management approach. Deciding how services communicate. Introducing a new pattern that the rest of the codebase will follow.

These decisions have compounding consequences. The agent treats them the same as any other code generation task. It'll pick something reasonable-looking, but "reasonable-looking" and "right for your project" aren't the same thing. Architectural decisions need to be yours because you'll live with them.

### Something feels off

This is vague on purpose. Experienced developers have a pattern-matching instinct built from years of reading and writing code. When generated code triggers that feeling -- something about the structure, the naming, the approach, the complexity -- don't ignore it.

You don't need to articulate *why* it feels off before acting on it. The feeling is the signal. Investigation is the response.

### The agent is making security decisions

Auth flows, permission models, encryption choices, input validation strategy, CORS policy, rate limiting. These are not code generation tasks. They're security architecture, and getting them almost right is the same as getting them wrong.

Review every security-adjacent decision manually. If the agent generated an auth middleware, read every line. If it set up CORS, check the policy. If it's validating input, verify coverage.

### The problem requires system-level understanding

The agent sees code. It doesn't see your deployment topology, your traffic patterns, your team's on-call rotation, your user demographics, your compliance requirements, your infrastructure constraints. When the right answer depends on context that isn't in the codebase, the agent can't have it.

Database schema design, caching strategy, background job architecture, API versioning policy -- these need human judgment informed by system-level understanding.

## Types of decisions that should always be human

- What data to store and how to structure it
- Authentication and authorization model
- What errors to expose to users vs. log internally
- Service boundaries and communication patterns
- Data migration strategy
- What to cache and for how long
- Rate limiting and abuse prevention thresholds
- Third-party service selection
- Breaking changes and versioning

## How to hand back control

Taking the wheel doesn't mean abandoning the agent. It means directing it more specifically:

1. **Diagnose the problem yourself.** Read the code, understand the error, form a hypothesis.
2. **Give the agent specific instructions.** Instead of "fix this bug," say "the issue is that the middleware runs after validation instead of before. Move the auth check to run first."
3. **Write the critical part yourself and let the agent do the rest.** Sketch the architecture, write the core logic, then let the agent fill in the boilerplate, tests, and peripheral code.
4. **Use plan mode.** If your agent supports it, have it outline the approach first. Review the plan. Correct it. Then let it execute.

The goal isn't to do everything manually. It's to keep human judgment on the decisions that matter while letting the agent handle the work that doesn't require judgment.

## The "could I explain this?" test

After every significant piece of generated code, ask yourself:

> Could I explain this to a teammate? Not the syntax -- the *decisions*. Why this structure? Why this pattern? What are the trade-offs?

If the answer is yes, keep moving. If the answer is no, slow down. Read the code. Understand it. Then decide whether it's right.

This test takes ten seconds. It catches problems that take hours to fix later.
