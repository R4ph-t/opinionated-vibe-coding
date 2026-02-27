# Python Stack Rules

Additional rules for Python projects.

## Type Hints
- Use type hints on all function signatures.
- Use `from __future__ import annotations` for modern annotation syntax.
- Use Pydantic models for data validation and serialization.

## Imports
- Follow PEP 8 import ordering: standard library, third-party, local.
- Use absolute imports, not relative, unless within the same package.
- Do not use wildcard imports (`from module import *`).

## Async
- Use async/await with asyncio for I/O-bound operations.
- Do not mix sync and async database calls in the same project.
- Use httpx for async HTTP requests, requests for sync.

## Error Handling
- Create custom exception classes for domain-specific errors.
- Never use bare `except:`. Always catch specific exceptions.
- Use `raise ... from ...` to preserve exception chains.

## Project Structure
- Use `pyproject.toml` for project configuration, not setup.py.
- Use a `src/` layout for packages intended for distribution.
- Keep `__init__.py` files minimal.

## Environment
- Use pydantic-settings or python-dotenv for environment variable management.
- Validate required environment variables at startup.
- Never hardcode configuration values.

## Common Packages to Prefer
- Web framework: check what the project uses (FastAPI, Flask, Django)
- Validation: pydantic
- Testing: pytest
- HTTP client: httpx
- Database: SQLAlchemy or the project's existing ORM
- Task queue: check project (Celery, RQ, etc.)
