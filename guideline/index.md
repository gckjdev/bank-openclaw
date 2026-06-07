# Engineering Guidelines Index

Use this directory as the primary engineering context for the repository.

## Priority Order

Apply guidance in this order:

1. Explicit user request
2. Existing repository patterns
3. Relevant guideline file in this directory
4. General best practices

If a guideline conflicts with stable existing code, prefer the existing code unless the task is explicitly a refactor.

## Global Defaults

- Make the smallest viable change
- Reuse existing patterns before introducing new ones
- Prefer explicit types, schemas, and boundaries
- Keep modules focused on one responsibility
- Keep each code file under a hard maximum of `500` lines; start splitting around `400` lines
- Prefer code that is testable, debuggable, and easy to review
- Do not invent infrastructure that does not already exist
- Do not widen scope just to satisfy a guideline

## Stack Defaults

- `apps/web`: `TypeScript` + `React` + `Vite`
- Routing: `TanStack Router`
- Data fetching: `TanStack Query`
- UI state: `Zustand`
- UI components: `shadcn/ui` or a lightweight custom design system
- `apps/cli`: `TypeScript` + `Node.js CLI` + `Commander.js`
- `apps/api`: `TypeScript` + `Fastify` + `@fastify/websocket`
- Validation: `Zod`
- ORM / SQL: `Drizzle ORM`
- Database: `PostgreSQL`
- Testing: `Vitest` + `supertest` + `Playwright`
- Baseline tooling: `ESLint` + `Prettier`

## Which File To Read

- Frontend: `guideline/frontend.md`
- Backend: `guideline/backend.md`
- UI: `guideline/ui.md`
- Database: `guideline/database.md`
- Testing: `guideline/testing.md`

Only read the files relevant to the task.

Recommended combinations:

- `apps/web`: frontend + UI + testing
- `apps/api` or `apps/cli`: backend + database + testing
- schema / migration / repository work: database + backend

## LLM Behavior

Default to:

- Short, directly applicable output
- Conservative assumptions when information is missing
- Local edits over broad rewrites
- Stable naming consistent with the codebase
- One clear approach, not an option dump
