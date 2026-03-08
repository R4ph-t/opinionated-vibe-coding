# Prompt Engineering for Coding Agents

The quality of what your AI coding agent produces is directly proportional to the quality of what you ask for. Vague prompts produce vague code. Specific prompts produce specific, usable code.

This guide covers practical techniques for getting better output from coding agents in day-to-day development.

## Be specific about what you want

Bad: "Add authentication."

Good: "Add JWT-based authentication middleware to the Express app. Use the existing `jsonwebtoken` package. Protect all routes under `/api/` except `/api/auth/login` and `/api/auth/register`. Store the JWT secret in the `JWT_SECRET` environment variable. Return 401 with `{ error: 'Unauthorized' }` for missing or invalid tokens."

The difference is not verbosity for its own sake. The second prompt eliminates ambiguity about the auth method, the library, the routes, the secret storage, and the error format. Each decision you don't specify is a decision the agent will guess at.

### What to include

- **File paths** when you know them: "Modify `src/middleware/auth.ts`"
- **Function and variable names** when they matter: "Add a `verifyToken` function"
- **Expected behavior** for edge cases: "If the token is expired, return 401, not 403"
- **What NOT to do** when there's a common wrong approach: "Do not use cookies for token storage"

## Reference existing code

Agents work best when they have examples to follow. Point them at patterns already in your codebase.

- "Follow the same pattern as `src/routes/users.ts` for the new `src/routes/orders.ts` route."
- "Use the same error handling approach as `src/lib/api-client.ts`."
- "This should match the validation pattern we use in `src/schemas/user.schema.ts`."

This is especially important for maintaining consistency across a growing codebase. Without a reference, the agent will invent its own pattern, and it probably won't match yours.

## Break complex tasks into steps

A single prompt asking the agent to "build a user management system with roles, permissions, invitations, and audit logging" will produce mediocre results. The agent will make dozens of assumptions and cut corners to fit everything into one response.

Instead, sequence the work:

1. "Design the database schema for users, roles, and permissions. Show me the migration files before writing any application code."
2. "Implement the user CRUD endpoints using the schema we just defined."
3. "Add role-based authorization middleware based on the permissions table."
4. "Add the invitation flow: endpoint to invite by email, token generation, and acceptance endpoint."
5. "Add audit logging for all user management actions."

Each step builds on confirmed, reviewed work from the previous step. The agent has less room to go wrong, and you catch problems early.

## Provide constraints

Tell the agent what it cannot do, not just what it should do:

- "Do not add any new dependencies."
- "Do not modify the existing tests."
- "Keep the public API backward-compatible."
- "Do not change the database schema."
- "This must work without JavaScript for the initial render."

Constraints prevent the agent from taking shortcuts that create problems downstream.

## Correct instead of restarting

When the agent produces something wrong, your first instinct might be to start over. Resist that. Correction is usually more effective:

- "The error handling is wrong. Catch `ValidationError` specifically and return 422, not 400."
- "You used a default export, but the project uses named exports. Fix that."
- "This function is doing too much. Extract the email formatting into a separate `formatInvitationEmail` function in `src/lib/email.ts`."

Corrections teach the agent about your preferences in context. Starting over throws away that context.

## Use skills for structured work

When you need a specific type of review or analysis, use OVC skills instead of ad-hoc prompts. "Run a security review" triggers a structured, repeatable process that covers all the categories. "Check if there are any security issues" gets you a surface-level glance.

Skills are especially useful for:
- **Audits and reviews** where completeness matters (security, accessibility, performance)
- **Analysis tasks** where a checklist prevents blind spots (API review, dependency check)
- **Setup tasks** where following a process prevents missing steps (OVC setup, MCP setup)

## Give context about why

Agents make better decisions when they understand the purpose, not just the task:

- "We're adding rate limiting because this endpoint is being hit by bots. Focus on the `/api/auth/login` and `/api/auth/register` endpoints. 10 requests per minute per IP."
- "This refactor is to support multi-tenancy later. Make sure the user queries filter by `organization_id` even though there's only one org right now."

The "why" helps the agent make better judgment calls on the details you didn't specify.

## Know when the prompt is the problem

If the agent keeps producing wrong results, consider that your prompt might be ambiguous rather than the agent being bad. Signs:

- The agent's output is reasonable but not what you meant
- You keep adding corrections that could have been in the original prompt
- The agent asks clarifying questions (good agents do this; answer them)

Take a step back, write a clearer prompt, and try again. A well-written prompt often takes more time to compose than a quick one, but it saves time overall.
