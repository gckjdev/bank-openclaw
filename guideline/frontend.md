# Frontend Code Guideline

## Scope

Applies to `apps/web`.

Default stack:

- `TypeScript`
- `React`
- `Vite`
- `TanStack Router`
- `TanStack Query`
- `Zustand`
- `shadcn/ui` or a lightweight custom design system

---

## AI Usage

When the LLM generates frontend code, use this decision order by default:

1. Reuse existing page, feature, component, and hook patterns
2. Put server state in `TanStack Query`
3. Put UI state in `Zustand` or local component state
4. Keep routing logic in `TanStack Router`
5. Reuse the existing design system or `shadcn/ui` for styles and base components

Do not introduce a new state library, form framework, or data-layer abstraction unless there is a clear reason.

---

## Must Rules

- The frontend is not the source of business truth; authoritative data comes from the server
- Do not scatter raw `fetch` calls directly inside components
- Do not copy query data into `Zustand`
- Route files should only handle params, composition, page-level preloading, and permission checks
- Keep page components thin; push complex logic into hooks, helpers, and query wrappers
- Explicitly type props, query return values, and route params
- Do not use broad `any` types or force-casts to hide boundary problems
- All key pages must explicitly handle `loading / empty / error / success`

---

## Default Structure

Prefer organization by business domain:

```text
apps/web/src
  /routes
  /features
  /components
  /api
  /stores
  /lib
```

Default responsibilities:

- `routes/*`: route definitions, param reading, page composition
- `features/*`: domain-specific page sections, hooks, queries, actions
- `components/*`: reusable cross-domain components
- `api/*`: clients, request helpers, query/mutation wrappers
- `stores/*`: UI state only
- `lib/*`: pure functions, formatters, constants

---

## Routing

When using `TanStack Router`:

- URL names should express business meaning, not implementation detail
- List, detail, and edit routes should have a clear hierarchy
- The routing layer should not carry complex business logic

Examples:

- `/customers`
- `/customers/$customerId`
- `/customers/$customerId/edit`
- `/approvals`
- `/approvals/$approvalId`

---

## Data Fetching

Put server state in `TanStack Query` by default:

- Lists
- Detail data
- Metrics
- Remote configuration
- Read-after-write data from mutations

Required:

- Query keys must be stable and derivable
- Query functions should not live directly inside large components
- After mutations, explicitly choose invalidation, cache update, or refetch

Recommended key shapes:

```text
['customers']
['customers', filters]
['customer', customerId]
['approval', approvalId, 'timeline']
```

---

## State Management

Use `Zustand` for UI state only, such as:

- Drawer / modal visibility
- Current tab
- Local filters
- Temporary sort preferences
- Local interaction drafts

Do not put these into `Zustand`:

- Remote detail objects
- List data
- Cached data already owned by queries
- Data that can be derived from URL, props, or query results

If local component state is enough, do not lift it into a global store.

---

## Components

Components should usually fall into three layers:

- Base components: buttons, inputs, dialogs, badges
- Business components: for example customer cards or approval timelines
- Page sections: sections used by a single page only

Default rules:

- Presentation components should stay as pure as possible
- Container components should assemble data
- Form schemas, default values, and submit transforms should stay separate where possible
- Table column definitions should be separated from data fetching

Split a file when you see these signals:

- It handles several unrelated requests
- It manages too many local interaction states
- It combines tables, forms, dialogs, and data transformation
- It approaches `250-300` lines

---

## Style And Quality

- Use `ESLint` and `Prettier` as the formatting and style baseline
- Call hooks only at the top level
- Use `useEffect` only for side effects, not main data orchestration
- Prefer derived values over adding extra state
- Prefer business-semantic names over technical names

---

## Design System Defaults

- Reuse existing components and tokens first
- If no component exists, compose from `shadcn/ui` first
- Do not create heavy design-system abstractions for local needs
- Business-semantic components are fine, but their boundaries should stay stable

---

## Avoid

- Writing requests directly inside components and manually managing cache
- Mirroring server data in `Zustand`
- Turning route files into the page and business-logic center
- Putting all logic into one page component
- Handling only the happy path
- Introducing a new library or pattern for a one-off need

---

## Summary

**The default frontend model is: `TanStack Router` handles routing, `TanStack Query` handles server state, `Zustand` handles UI state, pages stay thin, components and hooks remain composable, and the existing design system should be reused whenever possible.**
