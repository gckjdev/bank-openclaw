# UI Design Guideline

## Scope

Applies to UI design, interaction design, and the design system in `apps/web`.

Default style:

- Enterprise-grade and professional
- Clear, restrained, and trustworthy
- Suited for workbenches, admin tools, data-heavy views, and process-driven interfaces

---

## AI Usage

When the LLM generates UI or page structure, the default goal is not "flashy" but "clear, stable, and useful."

Priority order:

1. Clear information hierarchy
2. Consistent state expression
3. Obvious action paths
4. Restrained visual style
5. Local polish only after the above

Unless the user asks otherwise, prefer enterprise application style over marketing-page or showcase style.

---

## Must Rules

- The same semantic meaning must use the same visual language
- A page must make the title, primary action, current status, and main content obvious at a glance
- State colors are for state, not decoration
- Forms, tables, and detail views must have stable, predictable structure
- `loading`, `empty`, `error`, `disabled`, `success`, and similar states must be explicit
- High-risk actions must have confirmation or clear warning
- Interactive elements must include basic hover, focus, and disabled states

---

## Visual Defaults

Default visual language:

- Neutral light backgrounds or white surfaces
- Dark high-contrast text
- Low-saturation colors with limited accents
- Restrained use of shadows and motion

Default limits:

- No more than `1-2` primary accent color families per page
- Avoid large gradients and heavy shadows
- Avoid multiple competing focal points

---

## Typography And Spacing

- Use a stable sans-serif font
- Maintain clear hierarchy across titles, body text, supporting copy, and labels
- Use a consistent spacing scale
- Spacing between sections should be larger than spacing within a section
- Do not patch layouts with arbitrary one-off margins

Each page should clearly express at least these levels:

- Page title
- Section title
- Body content
- Supporting text

---

## Page Layout

Enterprise application pages typically include:

- A title area
- A filter or action area
- A main content area
- A detail area, side panel, or supporting panel

Each page should clearly answer three questions:

- What is this page?
- What is the most important information right now?
- What can the user do next?

---

## Lists, Detail, Forms

Default rules for list pages:

- Keep filters, sorting, and bulk actions in stable positions
- Show key fields first
- Do not let secondary information crowd the first screen

Default rules for detail pages:

- Show a summary at the top
- Put status, owner, last updated time, and key metrics near the top
- Separate history, audit, and supporting information into distinct sections

Default rules for forms:

- Organize fields by user task flow, not by database column order
- Make required, optional, and read-only fields obvious
- Keep submit, cancel, and dangerous actions in stable locations

---

## Design System

Default strategy:

- Use `shadcn/ui` as the base when possible
- Add tokens, semantic components, and layout conventions on top
- Build custom components only for stable abstractions that are actually needed

Base components to standardize first:

- Button
- Input
- Select
- Textarea
- Dialog / Drawer
- Tabs
- Table
- Badge
- Alert / Toast

Semantic components worth building over time:

- Status badges
- Approval timelines
- Summary cards
- KPI cards
- Empty-state panels
- Audit record blocks

---

## State And Feedback

Define and standardize these states clearly:

- default
- hover
- active
- disabled
- loading
- success
- warning
- error

Additional requirements:

- Empty states should explain what is missing
- Error states should explain what failed and what to do next
- No-permission, no-data, and load-failure states must stay distinct
- Long-running actions should show progress or explicit in-progress feedback

---

## Data Presentation

Tables:

- Left-align text and usually right-align numbers
- Keep column titles clear
- Row actions should not hide primary content
- Use a stable truncation strategy for long text

Metric cards:

- One card should communicate one core metric or conclusion
- Separate current value from trend information

Timelines / audit streams:

- Order must be obvious
- Event types should be easy to distinguish
- Fields should be complete without becoming overloaded

---

## Accessibility And Copy

- All interactive elements must have a focus state
- Contrast should meet basic readability requirements
- Icon buttons need text alternatives or `aria-label`
- Copy should be concise, professional, and actionable
- The same action should not use multiple names across different pages

---

## Avoid

- Generating marketing-page style UI
- Large gradients, heavy shadows, and complex motion
- Using different colors or copy for the same state
- Dense ungrouped forms
- Tables with too many columns to scan
- Mixing empty, error, and no-permission states
- Creating overly heavy design-system abstractions for one-off pages

---

## Summary

**The default UI style is enterprise-grade, restrained, structurally stable, and state-clear. Prioritize fast recognition and task completion over visual noise.**
