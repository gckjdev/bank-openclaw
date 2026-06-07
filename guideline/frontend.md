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


## Priority

1. Reuse existing page, feature, component, and hook patterns
2. Put server state in `TanStack Query`
3. Put UI state in `Zustand` or local component state
4. Keep route logic in `TanStack Router`
5. Reuse the existing design system or `shadcn/ui`

Do not introduce a new state library, form framework, or data layer unless the task clearly requires it.

## Hard Rules

- The frontend is not the business source of truth; authoritative data comes from the server
- Do not scatter raw `fetch` calls inside components
- Do not mirror query data in `Zustand`
- Keep each code file under a hard maximum of `500` lines; start splitting around `250-300` lines
- Route files only handle params, composition, preloading, and permission checks
- Keep page components thin; push logic into hooks, helpers, and query wrappers
- Explicitly type props, route params, and query return values
- Avoid broad `any` and force-casts
- Key pages must handle `loading / empty / error / success`

## Default Structure

Prefer business-domain organization:

```text
apps/web/src
  /routes
  /features
  /components
  /api
  /stores
  /lib
```

- `routes/*`: route definitions and page composition
- `features/*`: domain sections, hooks, queries, actions
- `components/*`: reusable components
- `api/*`: clients and query/mutation wrappers
- `stores/*`: UI state only
- `lib/*`: pure helpers and constants

## Routing

- URLs should express business meaning, not implementation detail
- Keep clear list/detail/edit hierarchies
- Do not put business workflows in the routing layer

Example shapes:

- `/customers`
- `/customers/$customerId`
- `/customers/$customerId/edit`

## Data Fetching

Use `TanStack Query` for:

- Lists
- Detail data
- Metrics
- Remote configuration
- Read-after-write data

Required:

- Stable query keys
- Query functions outside large components
- Explicit invalidation, cache updates, or refetch after mutations

## State Management

Use `Zustand` only for UI state:

- Drawer / modal visibility
- Current tab
- Local filters
- Temporary sort preferences
- Local drafts

Do not store:

- Remote objects
- List data
- Query-owned cache
- Data derivable from URL, props, or query results

Prefer local component state when possible.

## Components

Preferred layers:

- Base components
- Business components
- Single-page sections

Split files when they:

- Handle unrelated requests
- Manage too many local states
- Combine tables, forms, dialogs, and data transformation
- Approach `400` lines, and never exceed `500` lines per code file

## Style

- Use `ESLint` and `Prettier`
- Call hooks only at the top level
- Use `useEffect` only for side effects
- Prefer derived values over extra state
- Prefer business-semantic names

## Avoid

- Requests directly in components
- Server-data mirroring in `Zustand`
- Heavy route files
- One giant page component
- Happy-path-only UI
- One-off libraries or patterns
