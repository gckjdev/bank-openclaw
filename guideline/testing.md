# Testing Guideline
## Scope
Applies to:
- Unit tests: `Vitest`
- API tests: `Vitest` + `supertest`
- Web E2E: `Playwright`
- Mocks, fixtures, test data, and quality gates

## Priority
1. Identify the affected flow or boundary
2. Choose the lightest test layer that covers the risk
3. Add only tests with real protective value

Do not default to E2E if a unit test or API test is enough.

## Hard Rules
- Prioritize high-risk flows, protocol boundaries, and failure paths
- Unit, API, and E2E tests protect different layers
- Keep each test-related code file under a hard maximum of `500` lines; start splitting around `400` lines
- Do not test only the happy path
- Do not replace behavioral assertions with large snapshot coverage
- Mocks should preserve realistic success and failure branches
- When changing `--json` output, verify structural stability

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
- Request validation
- Response schema
- Auth / permission boundaries
- Critical write operations

Prefer E2E tests for:
- Core post-login flows
- Main list-to-detail navigation
- Key form submissions
- Key state transitions
- Role- or permission-based differences

## Unit Tests
- One test file should focus on one module or one tightly related behavior group
- Express rules through input and output
- Use clear test names
- Prefer table-driven tests for rule branches
- Avoid over-mocking implementation detail

## API Tests
With `Vitest` + `supertest`, cover at least:
- Success paths
- Validation failures
- Permission failures
- Not-found cases
- Important error responses

Assert:
- Status code
- Response shape
- Key fields

Do not stop at "returned 200."

## E2E Tests
With `Playwright`:
- Each spec should focus on one main user flow
- Prefer stable selectors such as `data-testid`
- Avoid hard waits
- Do not assert brittle style details

## Frontend Defaults
For `apps/web`, prioritize:
- `loading / empty / error / success` states
- Key form interactions
- Important filtering or pagination behavior
- Permission-related UI branches
- Display logic in key business components

Avoid:
- Large DOM snapshot suites
- Deep mocking of React internals
- Unrealistic routing, query, and store mocks

## Backend And CLI Defaults
For `apps/api`, prioritize:
- Schema validation
- The main `route -> service -> repository` path
- Key error responses
- Idempotency or transaction boundaries

For `apps/cli`, prioritize:
- Argument parsing
- Default stdout output
- `--json` structure
- Error exit codes
- Key command branches

CLI rule: distinguish `stdout` from `stderr`.

## Mocks And Fixtures
- Preserve success and failure branches
- Use business-meaningful fixture names
- Keep test data minimal but complete
- Avoid oversized JSON fixtures
- Keep time, randomness, and network dependencies controllable

Do not flatten complex systems into always-successful fakes.

## Quality Gate
Before finishing a change, answer:
1. Which flow or boundary changed?
2. What is the most likely regression?
3. Which test layer best covers it?

Targeted verification is usually required for:
- API protocol changes
- Data model changes
- Permission logic changes
- Key form flow changes
- CLI output changes

## Avoid
- Low-value tests written only for coverage
- Happy-path-only suites
- Too many unrelated flows in one file
- Snapshot-heavy testing
- Unstable E2E selectors
- Oversimplified mocks that hide regressions
