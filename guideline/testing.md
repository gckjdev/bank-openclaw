# Testing Guideline

## Scope

Applies to:

- Unit tests: `Vitest`
- API tests: `Vitest` + `supertest`
- Web E2E: `Playwright`
- Mocks, fixtures, test data, and quality gates

---

## AI Usage

When the LLM decides whether to add tests, the default goal is not "cover everything" but "cover the most risk with the least necessary test surface."

Default decision order:

1. Determine which flow or boundary the change affects
2. Choose the test layer closest to that risk
3. Add only tests with real protective value

If a simple unit test or API test is enough, do not default to heavyweight E2E coverage.

---

## Must Rules

- Prioritize high-risk flows, protocol boundaries, and failure paths
- Unit, API, and E2E tests each protect their own layer and should not substitute for one another
- Do not test only the happy path
- Do not replace behavioral assertions with large snapshot coverage
- Mocks should preserve both realistic success and failure branches
- When adding or changing `--json` output, verify structural stability

---

## Layer Selection

Prefer unit tests for:

- Pure functions
- Formatters
- Parsers
- Mappers
- Rule evaluation
- Calculation logic

Prefer API tests for:

 - HTTP routes
- request validation
- response schema
- Auth / permission boundaries
- Critical write operations

Prefer E2E tests for:

- Core post-login user flows
- Main list-to-detail navigation flows
- Key form submission flows
- Key state transitions
- Role- or permission-based flow differences

---

## Unit Tests

Default expectations:

- A test file should focus on one module or one tightly related behavior group
- Rules should be expressed through input and output
- Test names should express the constraint clearly, not vaguely

Recommended:

- Use table-driven tests for rule branches
- Keep mocks limited; avoid over-mocking implementation detail

---

## API Tests

With `Vitest` + `supertest`, prioritize coverage for at least:

- Success paths
- Validation failures
- Permission failures
- Not-found cases
- Important error responses

Required assertions:

- Status code
- Response shape
- Key fields

Do not stop at asserting that "it returned 200."

---

## E2E Tests

When using `Playwright`:

- Each spec file should focus on one main user flow
- Prefer stable selectors such as `data-testid`
- Avoid hard waits; use explicit observable assertions instead
- Do not turn style details into brittle assertions

---

## Frontend Testing Defaults

For `apps/web`, prioritize verifying:

- `loading / empty / error / success` states
- Key form interactions
- Important list filtering or pagination behavior
- Permission-related UI branches
- Display logic in key business components

Avoid:

- Large DOM snapshot suites
- Deep mocking of React internals
- Mocking routing, queries, and stores into an unrealistic environment

---

## Backend And CLI Testing Defaults

For `apps/api`, prioritize verifying:

- Schema validation
- The main `route -> service -> repository` path
- Key error responses
- Idempotency or transaction boundaries

For `apps/cli`, prioritize verifying:

- Argument parsing
- Default stdout output
- `--json` output structure
- Error exit codes
- Key command branches

Additional CLI requirements:

- `--json` mode should not only be checked for existence; its structure must be verified
- Error tests must distinguish `stdout` from `stderr`

---

## Mocks And Fixtures

Default rules:

- Preserve both success and failure branches when mocking external dependencies
- Fixture names should express business meaning
- Test data should be minimal but complete
- Avoid oversized JSON fixtures where possible
- Keep time, randomness, and network dependencies controllable

Do not flatten complex systems into fake implementations that always succeed.

---

## Quality Gate

Before finishing a change, answer at least these three questions:

1. Which main flow or boundary did this change affect?
2. What is the most likely regression point?
3. Which test layer is best suited to cover it?

These changes usually require targeted verification:

- API protocol changes
- Data model changes
- Permission logic changes
- Key form flow changes
- CLI output protocol changes

---

## Avoid

- Writing low-value tests just to increase coverage numbers
- Testing only the happy path
- Putting too many unrelated flows into one test file
- Over-relying on snapshots
- Using unstable selectors in E2E tests
- Oversimplifying mocks until real regressions can no longer surface

---

## Summary

**The default testing strategy is to use the lightest, most stable, most risk-aligned test layer to protect critical flows, protocol boundaries, and failure paths, rather than chasing superficial coverage.**
