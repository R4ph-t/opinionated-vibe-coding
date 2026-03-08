# Ruby Stack Rules

Additional rules for Ruby projects.

## Style and Structure
- Add `# frozen_string_literal: true` to the top of every Ruby file.
- Follow Rails conventions when in a Rails project. Do not fight the framework.
- Prefer symbols over strings for hash keys.
- Keep methods short. If a method exceeds ~15 lines, extract helpers.

## Types
- If the project uses Sorbet or RBS, maintain type annotations on public methods.
- Use `T.let`, `T.nilable`, and `sig` blocks consistently when Sorbet is present.

## Error Handling
- Rescue specific exceptions, never bare `rescue`.
- Use custom exception classes for domain errors.
- Use `raise` with a message, not `fail`.

## Rails
- Follow the standard Rails directory layout. Do not introduce custom top-level directories without approval.
- Keep controllers thin. Business logic belongs in models or service objects.
- Use Active Record callbacks sparingly. Prefer explicit service objects for complex workflows.
- Use Strong Parameters for all controller inputs.
- Run migrations with `rails db:migrate` and keep `schema.rb` committed.

## Testing
- Use RSpec or Minitest (match the project's existing framework).
- Use FactoryBot or fixtures for test data. Do not create records with raw SQL in tests.
- Test behavior, not implementation. Avoid over-mocking.

## Environment
- Use `Rails.application.credentials` or `dotenv` for secrets.
- Validate required environment variables at boot in an initializer.

## Common Gems to Prefer
- Background jobs: Sidekiq or the project's existing queue
- Auth: Devise or the project's existing auth
- Serialization: ActiveModel::Serializer or Blueprinter
- HTTP client: Faraday
