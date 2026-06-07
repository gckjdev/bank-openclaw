# Backend Code Guideline
## Scope
Applies to `apps/api`, `apps/cli`, and server-side `packages/*`.

Default stack:
- `TypeScript`
- `Node.js`
- `Fastify`
- `@fastify/websocket`
- `Commander.js`
- `Zod`
- `Drizzle ORM`
- `PostgreSQL`

## Priority
1. Reuse existing module boundaries and directory structure
2. Define schemas and types before routes, commands, or services
3. Keep routes and commands thin
4. Put business orchestration in services
5. Put data access in repositories

Do not introduce a new framework, DI container, or heavy runtime abstraction unless the task clearly requires it.

## Hard Rules
- API, CLI, and WebSocket inputs and outputs must have explicit schemas
- `Zod` owns boundary validation; business rules belong in services
- Routes and commands only parse input, validate, invoke, and format responses
- Keep each code file under a hard maximum of `500` lines; start splitting around `250-300` lines
- Services should not depend directly on HTTP request/reply or CLI I/O
- Repositories should not carry business workflows
- `--json` output must be stable and free of extra text
- Errors must be categorized, and logs must keep key context
- Avoid broad `any`, implicit return shapes, and vague message protocols

## Default Structure
API:
```text
apps/api/src
  /plugins
  /routes
  /modules
  /db
  /lib
```
CLI:
```text
apps/cli/src
  /commands
  /services
  /formatters
  /schemas
  /lib
```
Within modules, prefer `*.schema.ts`, `*.service.ts`, `*.repository.ts`, and `*.route.ts` or `*.command.ts`.

## API
`Fastify` route handlers should only do parameter reading, `Zod` validation, service invocation, response formatting, and required auth / permission hooks.

Do not use routes for:
- Business workflows
- Direct SQL
- Large data transformation

## Service, Repository, WebSocket
Services own business rules, cross-repository coordination, transactions, idempotency, and audit coordination.

Repositories own reads, writes, clear query semantics, and reusable data access methods.

WebSocket rules:
- Each message type needs an explicit schema
- Separate connection, subscription, push, and error message types
- WebSocket pushes real-time state; it does not replace database authority
- Connections need auth, authorization, and subscription boundaries

## CLI
Each command should define input arguments, options, output modes, and exit codes.

Defaults:
- Human-readable text in normal mode
- Stable JSON in `--json`
- Errors in `stderr`
- Non-zero exit codes for unexpected failures

Split complex commands into parser, handler, and formatter.

## Validation And Persistence
Use `Zod` for HTTP request / response, WebSocket messages, normalized CLI input, and environment variable parsing.

Use `Drizzle ORM` for schema definition, clear queries, and transaction-safe writes.

Do not push all business rules into schemas, and do not leak ORM details into routes.

## Errors, Testing, Style
- Distinguish validation, auth, permission, not-found, conflict, dependency, and internal errors
- API and CLI error shapes must be consistent
- Logs must include request id, entity id, and trace context
- Services, routes, repositories, commands, and WebSocket handlers should be independently testable
- Use `ESLint` and `Prettier`
- Start splitting files around `400` lines, with a hard maximum of `500` lines per code file

## Avoid
- Heavy routes or commands
- Services directly operating on HTTP objects or stdout
- Repositories mixed with permission or workflow logic
- Frontend-backend "mutual understanding" payloads
- Logs mixed into `--json`
- Overweight architecture for local needs
