# UI Design Guideline

## Scope

Applies to UI design, interaction design, and the design system in `apps/web`.

Default style:

- Enterprise-grade and professional
- Clear, restrained, and trustworthy
- Suited for workbenches, admin tools, data-heavy views, and process-driven interfaces


## Priority

1. Clear information hierarchy
2. Consistent state expression
3. Obvious action paths
4. Restrained visual style
5. Local polish only after the above

Unless the user asks otherwise, prefer enterprise application style over marketing-page or showcase style.

## Hard Rules

- The same semantic meaning must use the same visual language
- A page should make the title, primary action, current status, and main content obvious at a glance
- Keep each UI-related code file under a hard maximum of `500` lines; start splitting around `400` lines
- State colors are for state, not decoration
- Forms, tables, and detail views must be stable and predictable
- `loading`, `empty`, `error`, `disabled`, and success states must be explicit
- High-risk actions need confirmation or clear warning
- Interactive elements need hover, focus, and disabled states

## Visual Defaults

- Neutral light backgrounds or white surfaces
- Dark high-contrast text
- Low-saturation colors with limited accents
- Restrained shadows and motion
- No more than `1-2` primary accent color families per page
- Avoid large gradients, heavy shadows, and competing focal points

## Typography And Layout

- Use a stable sans-serif font
- Keep clear hierarchy across titles, body text, supporting copy, and labels
- Use a consistent spacing scale
- Section spacing should be larger than internal spacing
- Do not patch layout with arbitrary one-off margins

Each page should clearly answer:

- What is this page?
- What is most important now?
- What can the user do next?

## Lists, Details, Forms

List pages:

- Keep filters, sorting, and bulk actions in stable positions
- Show key fields first
- Keep secondary information out of the first scan

Detail pages:

- Show a summary at the top
- Put status, owner, last updated time, and key metrics near the top
- Separate history, audit, and supporting information

Forms:

- Organize fields by task flow, not database order
- Make required, optional, and read-only states obvious
- Keep submit, cancel, and dangerous actions in stable locations

## Design System

- Use `shadcn/ui` as the base when possible
- Add tokens, semantic components, and layout conventions on top
- Build custom components only for stable abstractions

Standardize first:

- Button
- Input
- Select
- Textarea
- Dialog / Drawer
- Tabs
- Table
- Badge
- Alert / Toast

## State And Feedback

Standardize:

- default
- hover
- active
- disabled
- loading
- success
- warning
- error

Additional rules:

- Empty states explain what is missing
- Error states explain what failed and what to do next
- No-permission, no-data, and load-failure states must stay distinct
- Long-running actions need progress or explicit in-progress feedback

## Data Presentation

- Left-align text; usually right-align numbers
- Keep column titles clear
- Do not let row actions hide primary content
- Use stable truncation for long text
- One metric card should express one core metric or conclusion
- Timelines and audit streams should be ordered and easy to scan

## Accessibility And Copy

- All interactive elements need focus states
- Maintain readable contrast
- Icon buttons need text alternatives or `aria-label`
- Copy should be concise, professional, and actionable
- Use one stable name for the same action across the app

## Avoid

- Marketing-page style UI
- Large gradients, heavy shadows, complex motion
- Different colors or copy for the same state
- Dense ungrouped forms
- Overwide tables that are hard to scan
- Mixed empty, error, and no-permission states
- Heavy design-system abstractions for one-off pages
