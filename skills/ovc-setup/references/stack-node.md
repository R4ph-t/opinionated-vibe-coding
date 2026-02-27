# Node.js / TypeScript Stack Rules

Additional rules for Node.js and TypeScript projects.

## TypeScript
- Use strict TypeScript configuration. Enable `strict: true` in tsconfig.
- Define explicit return types on exported functions.
- Prefer `interface` over `type` for object shapes unless union types are needed.
- Do not use `any`. Use `unknown` if the type is truly unknown, then narrow it.

## Imports
- Use ES module imports, not CommonJS require.
- Prefer named exports over default exports.
- Group imports: external packages first, then internal modules, then relative imports.

## Async
- Use async/await, not raw promises with .then().
- Always handle errors in async functions. Unhandled promise rejections crash the process.
- Use try/catch at the appropriate level, not around every single await.

## Error Handling
- Create custom error classes for domain-specific errors.
- Never swallow errors silently (empty catch blocks).
- Return appropriate HTTP status codes, not just 500 for everything.

## Environment
- Access environment variables through a single config module, not scattered process.env calls.
- Validate required environment variables at startup, not at first use.

## Common Packages to Prefer
- Validation: zod
- HTTP framework: check what the project already uses (Express, Hono, Fastify)
- Testing: vitest or jest (check project config)
- Dates: date-fns (not moment, it's deprecated)
- Use native fetch, not axios (unless the project already uses axios)
