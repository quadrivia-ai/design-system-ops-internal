---
name: pattern-documentation
description: "Document a composed UI pattern (not a single component) — the components it combines, its state coverage, accessibility of the composition, and the rules for composing it correctly. Triggers on 'document this pattern', 'write up our form layout pattern', 'document the master-detail pattern', 'how should this flow be composed'. Do NOT trigger for a single component spec — use ai-component-description. Do NOT trigger for when-to-use guidance on one component — use usage-guidelines."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/component-bestiary-reference.md
  - ../../knowledge-notes/design-to-code-contract.md
---

# Pattern Documentation

## Context

Documents a *composition* of components that recurs as a pattern — a form layout, a
master-detail view, a confirm-and-act flow. It captures how the pieces fit, the states the
composition must handle, and the rules that keep instances consistent. Documents real
patterns found in the code; if the pattern can't be identified, it says so.

## Configuration

Reads (all optional): `stack.component_roots`; `target_repo` (to find real instances);
`output.report_dir`.

## Auto-pull integrations

None.

## Step 0 — Identify the pattern and its instances

Name the pattern and find real instances in the codebase. **Diagnostic mode:** if no
consistent instances exist (it's a one-off, or instances diverge wildly), report that — a
pattern isn't documentable until it's actually a pattern; point at `drift-detection` if
instances disagree.

## Step 1 — Composition

The components involved, their arrangement, and the composition rules (order, required parts,
optional parts, what must not be nested).

## Step 2 — State coverage

The states the whole pattern must handle (loading, empty, error, partial, success) — per the
[design-to-code-contract](../../knowledge-notes/design-to-code-contract.md) state dimension —
not just the happy path.

## Step 3 — Accessibility of the composition

A11y that lives at the composition level: focus order across the pieces, landmark/region
roles, how errors are announced. (Single-component a11y stays in
`accessibility-per-component`.)

## Produce the doc

```
## Pattern — <name>

**Headline.** <When to use this pattern + its one composition rule that's easy to get wrong.>

### Composition
<components + arrangement + rules; a small code sketch>

### State coverage
loading / empty / error / partial / success — <how the pattern handles each>

### Accessibility (composition-level)
<focus order, regions, error announcement>

### Scope
- Instances reviewed: <real examples found>
- Diagnostic: <if not yet a consistent pattern — what diverges>
```

## Closing note

Document the pattern as it *should* compose, citing the cleanest real instance — and note the
divergent ones so they can be brought in line rather than enshrined.

## Quality checks

- [ ] Built from real instances; diagnostic mode if no consistent pattern exists.
- [ ] State coverage goes beyond the happy path.
- [ ] A11y is composition-level, not per-component.
- [ ] Output-discipline self-check passed.
