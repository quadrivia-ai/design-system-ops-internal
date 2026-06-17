# Component Bestiary — Challenge Rating (reference)

Calibrate audit, documentation, and validation depth to a component's **implementation
danger**, not its visual complexity. A flat-looking combobox is far more dangerous than an
ornate static banner. Loaded by `component-audit`, `accessibility-per-component`,
`design-to-code-check`, `component-api-validator`, `ai-component-description`. Pairs with
[output-discipline](output-discipline.md).

## What raises Challenge Rating

Danger comes from behaviour and accessibility surface, not pixels:

- Internal state / controlled–uncontrolled duality
- Async data; loading / error / empty states
- Focus management (focus trap, focus restore, roving tabindex)
- Portals / overlays (dialog, popover, tooltip, dropdown)
- Keyboard interaction model (arrow keys, type-ahead, escape, enter)
- ARIA surface (roles, `aria-*`, live regions)
- Composition depth (compound components, slots, `asChild`)
- Theming surface (many token touch-points; per-theme behaviour)

## The bands

| CR | Band | Examples (our stack) | Required depth |
|----|------|----------------------|----------------|
| **CR1–2** | Presentational | Badge, Label, Separator, Skeleton | Light: render + token correctness. |
| **CR3–4** | Interactive primitive | Button, Input, Switch, Checkbox | Standard: states, keyboard basics, a co-located test. |
| **CR5–6** | Composite | Select, Tabs, Accordion, Tooltip, Card-with-actions | Thorough: full state matrix, focus, ARIA, test. |
| **CR7+** | Dangerous | Dialog, Popover, Combobox/Command, DataTable, DnD lists | **Mandatory** a11y audit (`accessibility-per-component`) + a decision record for API choices. |

## How skills use CR

- `component-audit` orders coverage gaps by CR — an untested CR8 ranks above ten untested CR1s.
- `accessibility-per-component` sets thoroughness by CR; CR7+ cannot be signed off on static
  analysis alone — it requires manual keyboard + screen-reader verification.
- `ai-component-description` documents CR7+ in full; CR1–2 get the short form.

## Calibration, not excuse

A high CR raises the **bar** for that component; it never lowers it. A dangerous component
that skips its a11y audit is a 🔴 finding regardless of how much effort the audit would take.
