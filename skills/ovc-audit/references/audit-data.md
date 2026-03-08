# Data Layer Audit Checklist

Load this checklist when the project uses a database or ORM (Prisma, Drizzle, SQLAlchemy, ActiveRecord, GORM, Diesel, Entity Framework, or has migration files or a `models/` directory).

## N+1 Queries

- [ ] Are there ORM or database calls inside `for`/`forEach`/`map` loops? Each iteration fires a separate query instead of batching.
- [ ] Are associations loaded one at a time in a loop instead of using eager loading or a batch query?

## Unbounded Queries

- [ ] Are there queries that return all rows without `LIMIT` or pagination?
- [ ] Are there `SELECT *` queries on tables that could grow large?
- [ ] Are there queries with `WHERE` clauses that could match most or all rows on large tables?

## Missing Pagination

- [ ] Do queries backing list views or API list endpoints enforce a maximum number of results?
- [ ] Is pagination cursor-based or offset-based and consistent across the project?

## Missing Indexes

Check migration files and schema definitions:

- [ ] Are columns used in `WHERE` clauses indexed?
- [ ] Are columns used in `JOIN` conditions indexed?
- [ ] Are columns used in `ORDER BY` indexed (for large tables)?
- [ ] Are there composite queries that would benefit from a compound index?

## Connection Management

- [ ] Is connection pooling configured (not creating a new connection per request)?
- [ ] Are database queries protected by timeouts?
- [ ] Are connections properly closed or returned to the pool in error paths?

## Async and Sequencing

- [ ] Are there sequential `await` calls for independent queries that could run in parallel (`Promise.all`, `asyncio.gather`)?
- [ ] Are there external HTTP calls without timeouts?
- [ ] Are there blocking I/O operations (synchronous file reads, CPU-intensive computation) in request handlers?
