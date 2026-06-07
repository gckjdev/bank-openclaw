# Backend Code Guideline

## Scope

Applies to:

- `apps/api`
- `apps/cli`
- Server-side `packages/*`

Default stack:

- `TypeScript`
- `Node.js`
- `Fastify`
- `@fastify/websocket`
- `Commander.js`
- `Zod`
- `Drizzle ORM`
- `PostgreSQL`

---

## AI Usage

When the LLM generates backend code, use this decision order by default:

1. Reuse the existing module boundaries and directory structure
2. Define schemas and types before writing routes, commands, or services
3. Keep routes and commands thin
4. Put business orchestration in services
5. Put data access in repositories

Do not introduce a new framework, dependency injection container, or heavy runtime abstraction unless there is a clear reason.

---

## Must Rules

- API, CLI, and WebSocket inputs and outputs must have explicit schemas
- `Zod` owns boundary validation; business rules belong in services
- Routes and commands should only parse input, validate, invoke, and format responses
- Services should not directly depend on HTTP request/reply or CLI I/O
- Repositories should not carry complex business workflows
- `--json` output must be stable and must not include extra text
- Errors must be categorized, and logs must keep key context
- Do not use broad `any` types, implicit return shapes, or vague message protocols

---

## Default Structure

Default API structure:

```text
apps/api/src
  /plugins
  /routes
  /modules
  /db
  /lib
```

Default CLI structure:

```text
apps/cli/src
  /commands
  /services
  /formatters
  /schemas
  /lib
```

Within a module, prefer these file splits:

- `*.schema.ts`
- `*.service.ts`
- `*.repository.ts`
- `*.route.ts` or `*.command.ts`

---

## API Rules

`Fastify` route handlers should only do:

- Parameter reading
- `Zod` validation
- Service invocation
- Status code and response formatting
- Registration of required auth / permission hooks

Do not use routes to:

- Implement complex business workflows
- Write SQL directly
- Perform large amounts of data transformation

Services are responsible for:

- Business rules
- Coordination across repositories
- Transaction boundaries
- Idempotency and audit coordination

Repositories are responsible for:

- Reads and writes
- Clearly named business-query semantics
- Reusable data access methods

---

## WebSocket Rules

- Each message type must have an explicit schema
- Separate connection, subscription, push, and error message types
- WebSocket should push real-time state only, not replace database authority
- Connections need authentication, authorization, and subscription boundaries
- Do not use vague "catch-all event payload" style protocols

---

## CLI Rules

Each `Commander.js` command should clearly define:

- Input arguments
- Options
- Output modes
- Exit codes

Default output rules:

- Human-readable text in the default mode
- Stable JSON in `--json` mode
- Errors written to `stderr`
- Non-zero exit codes for unexpected failures

For complex commands, prefer splitting into:

- parser
- handler
- formatter

---

## Schema And Validation

Use `Zod` by default for:

- HTTP request / response
- WebSocket messages
- Normalized CLI input
- Environment variable parsing

Layering rules:

- Schemas own structure and basic constraints
- Services own business rules
- Database constraints provide the final consistency backstop

Do not push all business logic into schemas, and do not rely entirely on database errors for input validation.

---

## Database Access

Default responsibilities of `Drizzle ORM`:

- Define schema
- Express clear queries
- Support stable writes within transactions

Rules:

- Organize queries around real business access paths
- Do not leak ORM details into the route layer
- Keep complex queries readable before optimizing them
- Use raw SQL only when ORM expressions are unclear or in performance-critical cases

---

## Errors And Logging

At minimum, distinguish:

- validation error
- auth / permission error
- not found error
- conflict error
- external dependency error
- unexpected internal error

Required:

- API error responses must follow a consistent shape
- CLI `--json` errors must follow a consistent shape
- Logs must include request id, entity id, and trace context
- Do not swallow errors or collapse everything into a generic 500

---

## Testability

New code should, by default, be verifiable in these ways:

- Services can be unit tested
- Routes can be tested through API tests
- Repositories can be integration tested
- Command handlers can be tested without booting the full CLI
- WebSocket message handling can be tested independently

Time, randomness, and external requests should be replaceable or injectable where possible.

---

## Style And Quality

- Use `ESLint` and `Prettier` as the shared baseline
- Prefer splitting files when they approach `250-300` lines
- Function names should express the action and object clearly
- Break complex flows into smaller steps instead of writing very long functions

---

## Avoid

- Turning routes or commands into the business core
- Letting services directly operate on HTTP objects or stdout
- Mixing permission and workflow logic into repositories
- Relying on frontend-backend "mutual understanding" for WebSocket payloads
- Mixing logs or debug text into `--json` output
- Introducing overly heavy architecture for a local need

---

## Summary

**The default backend model is: `Fastify` and `Commander.js` own the entry points, `Zod` owns boundary validation, services own business orchestration, repositories own data access, `Drizzle ORM` owns clear persistence, and all output protocols must remain stable and testable.**
