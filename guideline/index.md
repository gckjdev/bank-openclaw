# Engineering Guidelines Index

## Purpose

This directory is the entry point for engineering guidelines used by both AI LLMs and human developers.

Goals:

- Serve as the default constraint set when generating code
- Provide a shared standard for code review
- Act as supporting context to reduce style drift and architectural sprawl

---

## How To Use

When an LLM uses these documents, apply them in this priority order:

1. Explicit user requirements for the current task
2. Existing implementation patterns and directory structure in the repository
3. The topic-specific guidelines in this directory
4. General best practices

If a guideline conflicts with the existing codebase:

- Prefer the stable patterns already present in the codebase
- Do not widen the refactor scope just to "match the guideline"
- Move toward the guideline gradually through new code or local refactors only

If there is no stronger constraint, default to the rules in this directory.

---

## Stack Defaults

Current default stack:

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
- Engineering baseline: `ESLint` + `Prettier`

---

## Global Rules

All topic-specific documents share these default rules:

- Prefer clear, local, maintainable code over premature abstraction
- Reuse existing patterns before introducing a new architecture
- Prefer explicit types, schemas, and interface boundaries over implicit conventions
- Each module should serve a single responsibility
- New code should favor testability, debuggability, and auditability
- Do not introduce heavy infrastructure for hypothetical future needs

---

## Which File To Read

- Frontend code: `guideline/frontend.md`
- Backend code: `guideline/backend.md`
- UI design: `guideline/ui.md`
- Database: `guideline/database.md`
- Testing: `guideline/testing.md`

Read only the most relevant topic documents for the task. Do not inject all documents into context every time.

Recommended combinations:

- For `apps/web` work: read frontend + UI + testing
- For `apps/cli` or `apps/api` work: read backend + database + testing
- For schema / migration / repository work: read database + backend

---

## AI-Specific Guidance

When generating or modifying code, the LLM should default to:

- Making the smallest viable change
- Preserving the existing naming and directory style
- Producing directly applicable structure rather than long explanations
- Making conservative assumptions when information is missing
- Avoiding invented infrastructure that does not already exist
- Avoiding "option dump" outputs unless the user explicitly asks for comparisons

---

## Summary

**The purpose of these guidelines is to help the LLM make consistent, stable, low-ambiguity engineering decisions across `web`, `cli`, `api`, database, and testing work.**
