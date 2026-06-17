---
name: component-decision-tree
description: "Emit a 'which component for which need' decision tree from the actual library, so a developer or agent lands on the right component by answering a few questions. Triggers on 'which component should I use for X', 'build a component decision tree', 'help me pick the right component', 'component chooser'. Do NOT trigger for documenting one component's API — use ai-component-description. Do NOT trigger for usage rules of an already-chosen component — use usage-guidelines."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/component-bestiary-reference.md
---

# Component Decision Tree

## Context

Builds a decision tree that routes a need to the right component in the library — so people
stop reinventing an atom that already exists. Built from the components actually present, not
a generic catalogue.

## Configuration

Reads (all optional): `stack.component_roots`; `output.report_dir`.

## Auto-pull integrations

None.

## Step 0 — Inventory by need-category

Group the real components by the need they serve: input & choice, overlay & disclosure,
feedback & status, layout & container, navigation. Note where two components overlap (a
decision point) or where a need has no component (a gap to flag, not to invent around).

## Step 1 — Build the branches

For each category, write the discriminating questions that pick between options (how many
options? searchable? modal vs inline? transient vs persistent?), ending at a named component.

## Produce the tree

```
## Component Decision Tree

**Headline.** <The most common fork people get wrong.>

### Choice / input
- Collecting one option?
  - 2–5 options, all visible → `RadioGroup`
  - many options → `Select`
  - many + searchable → `Combobox` / `Command`
  - boolean → `Switch` (setting) / `Checkbox` (form)

### Overlay / disclosure
- Needs focus + blocks the page → `Dialog`
- Transient, anchored to a trigger → `Popover` / `Tooltip` (non-interactive)
- Inline expand/collapse → `Accordion` / `Collapsible`

### Feedback / Layout / Navigation
<same shape>

### Gaps (need with no component)
- <need> — no current component; candidate for `contribution-workflow`

### Scope
- Built from: <components found under the roots>
- Assumed: need-categories from observed usage
```

## Closing note

Keep the tree to real components and honest gaps — a tree that points at a component you don't
have just sends people to build a duplicate. Flag the gap instead.

## Quality checks

- [ ] Every leaf is a component that actually exists; gaps flagged separately.
- [ ] Discriminating questions are concrete (counts, modality), not vibes.
- [ ] Output-discipline self-check passed.
