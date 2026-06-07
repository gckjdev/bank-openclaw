# Database Guideline

## Scope

Applies to:

- `PostgreSQL`
- `Drizzle ORM`
- schema
- migration
- seed
- repository / query layers


## Priority

1. Keep the data model clear
2. Put constraints explicitly in the database
3. Design around real query paths
4. Preserve history and auditability

Do not put core business structure into JSON unless the task clearly requires it.

## Hard Rules

- `PostgreSQL` is the single source of truth
- Primary keys, foreign keys, unique constraints, and status fields must be explicit
- Core query fields must be explicit columns
- Keep each database-related code file under a hard maximum of `500` lines; start splitting around `400` lines
- All schema changes must go through migrations
- Critical write flows need explicit transaction boundaries
- Important state changes must be traceable
- Repositories handle data access only

## Modeling

Default naming:

- Primary key: `id`
- Foreign key: `<entity>_id`
- Timestamps: `created_at`, `updated_at`

Rules:

- One table represents one core entity or relationship
- High-frequency filter, sort, and join fields should be explicit columns
- Enumerated states should use bounded values, not vague free text
- Similar entities should use consistent naming
- Avoid vague columns such as `data`, `info`, or `misc`

## JSON Columns

Good uses:

- Request snapshots
- Raw third-party response fragments
- Non-core extension fields
- Flexible snapshot-style content

Bad uses:

- High-frequency filter fields
- Core join relationships
- Critical state fields
- Stable analytical dimensions

Prefer explicit columns and relations for core structures.

## Drizzle And Repositories

Use `Drizzle ORM` for:

- Schema definition
- Clear queries
- Transaction-safe writes

Rules:

- Do not scatter raw table-operation detail across route and service layers
- Repositories encapsulate query semantics
- Services own transactions and workflow coordination
- Prefer readable queries over clever ORM constructions

## Migrations

- All schema changes go through migrations
- Migrations are version-controlled
- Schema code and migrations must stay consistent
- Each migration should contain one clear kind of change
- Breaking changes should follow: add new structure -> switch code -> remove old structure

## Indexing

Consider indexes first for:

- Foreign keys
- Status fields
- Timestamp fields
- High-frequency filter fields
- High-frequency sort fields

Before adding an index, answer:

- Which query uses it?
- How often?
- Is the write cost acceptable?
- Is there already a similar index?

Do not add indexes mechanically.

## Transactions, History, Performance

- Multi-step write flows that require consistency must use explicit transactions
- Use idempotency keys for duplicate submissions, callbacks, and high-risk actions
- Use unique constraints as the final deduplication backstop
- Important actions should be traceable by actor, action type, related entity, request context, timestamp, and result
- Paginate list queries by default
- Keep detail queries narrow
- Use dedicated paths for heavy exports and aggregations
- Avoid N+1 and unbounded full-table scans

## Seeds

- Seed data should be meaningful
- Test data should be minimal but complete
- Distinguish development seed, demo seed, and test fixtures

## Avoid

- Core dimensions in JSON
- Schema changes without migrations
- Missing foreign keys
- Overwriting important state without history
- Full-table scans without pagination
- Workflow logic inside repositories
