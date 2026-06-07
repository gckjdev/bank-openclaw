# Database Guideline

## Scope

Applies to:

- `PostgreSQL`
- `Drizzle ORM`
- schema
- migration
- seed
- repository / query layers

---

## AI Usage

When the LLM generates database-related code, the default goals are:

1. Keep the data model clear
2. Put constraints explicitly in the database
3. Design around real query paths
4. Preserve history and auditability

Unless there is a strong reason, do not put core business structure into JSON, and do not build complex models for hypothetical future cases.

---

## Must Rules

- `PostgreSQL` is the single source of database truth
- Primary keys, foreign keys, unique constraints, and status fields must be explicit
- Core query fields must be modeled as explicit columns
- All schema changes must go through migrations
- Transaction boundaries for critical write flows must be explicit
- Important state changes must be traceable
- Repositories handle data access only, not business orchestration

---

## Modeling

Default naming:

- Primary key: `id`
- Foreign key: `<entity>_id`
- Timestamp fields: `created_at`, `updated_at`

Default modeling rules:

- One table should represent one core entity or relationship
- High-frequency filter, sort, and join fields should be explicit columns
- Enumerated states should use a bounded value set, not vague free text
- Similar entities should follow consistent naming

Do not use semantically vague column names such as `data`, `info`, or `misc`.

---

## JSON Columns

Good candidates for JSON columns:

- Request snapshots
- Raw fragments of third-party responses
- Non-core extension fields
- Low-frequency, flexible, snapshot-style content

Poor candidates for JSON columns:

- High-frequency filter fields
- Core join relationships
- Critical state fields
- Business dimensions that need stable analytics and aggregation

Default rule: if a core structure can be modeled clearly, prefer explicit columns and relations.

---

## Drizzle Rules

Default responsibilities of `Drizzle ORM`:

- Define database schema
- Express clear queries
- Support controlled writes inside transactions

Layering requirements:

- Route and service layers should not scatter raw table operation details
- Repositories should encapsulate business query semantics
- Services should own transaction and workflow coordination

For complex queries, prioritize readability before cleverness. Do not create hard-to-maintain ORM constructions just to look advanced.

---

## Migrations

Required:

- All schema changes go through migrations
- Migrations are version-controlled
- Schema code and migrations stay consistent

Recommended:

- A migration should contain one clear kind of change
- Migration names should reflect business intent
- Breaking changes should follow a compatibility-first migration sequence

Default compatibility-first sequence:

1. Add the new structure first
2. Switch application code second
3. Remove the old structure last

---

## Indexing

Fields that should be considered for indexes first:

- Foreign keys
- Status fields
- Timestamp fields
- High-frequency filter fields
- High-frequency sort fields

Before adding an index, answer:

- Which query will use it?
- How often does that query run?
- Is the write cost acceptable?
- Is there already a similar index?

Do not add indexes mechanically to every field, and do not prebuild complex indexes for hypothetical future queries.

---

## Transactions And Idempotency

Required:

- Multi-step write flows that require consistency must be wrapped in explicit transactions
- Transactions should be managed by the service layer or a clear helper
- Repositories should not implicitly start complex transactions

Recommended:

- Design idempotency keys for duplicate submissions, external callbacks, and high-risk actions
- Use unique constraints as the final backstop for critical deduplication logic

---

## History And Audit

Important actions should be traceable at least by:

- Actor
- Action type
- Related entity
- Request context
- Timestamp
- Result status

History retention may follow one of these clear patterns:

- Current-state table + audit log
- Current-state table + version history table
- Event records + aggregate projection

The important part is not which pattern you choose, but that the rule stays stable and the traceability path stays clear.

---

## Query And Performance

- Paginate list queries by default
- Detail queries should fetch only necessary fields
- Heavy exports and aggregations should use dedicated paths
- Avoid N+1 patterns and unbounded full-table scans
- Do not mix high-frequency transactional queries and analytical queries into one generic endpoint

---

## Seed And Fixtures

- Seed data should have clear meaning
- Test data should be minimal but complete
- Distinguish clearly between development seed, demo seed, and test fixtures

Do not use mysterious sample data that nobody can understand.

---

## Avoid

- Putting core dimensions into JSON
- Making schema changes without migrations
- Skipping foreign keys and relying only on application conventions
- Overwriting important state without keeping history
- Scanning full tables without pagination
- Mixing workflow logic into repositories

---

## Summary

**The default database model is: `PostgreSQL` is the single source of truth, core structures are modeled explicitly, changes are managed through migrations, queries are designed around real access paths, and important actions preserve history and auditability.**
