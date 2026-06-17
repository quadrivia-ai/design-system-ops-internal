---
name: component-audit
description: "Audit the health of the component library — test-coverage gaps, duplication and custom reimplementation of shadcn/ui primitives, complexity hot spots, usage signals, and orphaned components. Triggers on 'audit my components', 'component library health', 'which components are risky', 'find duplicate components', 'what's untested'. Do NOT trigger for a machine-readable component index/inventory — use codebase-index. Do NOT trigger for a holistic multi-dimension sweep — use system-health. Do NOT trigger for prop-API consistency in depth — use component-api-validator. Do NOT trigger for one component vs its design spec — use design-to-code-check."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/component-bestiary-reference.md
  - ../../knowledge-notes/component-governance.md
---

# Component Audit

## Context

Reads the shared component library and reports where it is fragile: untested high-risk
components, duplicated purpose, complexity hot spots, and components nobody imports. Findings
are calibrated by **Challenge Rating** — real implementation danger — not by how a component
looks (see [component-bestiary-reference](../../knowledge-notes/component-bestiary-reference.md)).
Output is a label-based report a library owner can triage.

## Configuration

Reads (all optional): `stack.component_roots` (defaults `src/components/{ui,atoms,molecules,organisms}`);
`stack.test` (`vitest`, `co-located`); `stack.ui_library` (`shadcn`); `severity_overrides`;
`accepted_findings`; `maturity_level`.

## Auto-pull integrations

None required. If GitHub is configured, MAY use import-frequency and change-frequency from
history to sharpen the usage and hot-spot signals. Works fully offline from source.

## Step 0 — Inventory and rate

List every component under the roots. Assign each a Challenge Rating from real implementation
danger (state, async, focus management, portals, a11y surface) — not visual complexity.

## Step 1 — Coverage against the bar

Best practice: every shipped component carries a co-located test; CR7+ components carry an
a11y test. Count `*.test.tsx` beside each component. Flag gaps **ordered by CR** — an
untested CR8 dialog is 🔴; an untested CR1 presentational atom is ⚪.

## Step 2 — Duplication and reimplementation

Find components with overlapping purpose (two button-likes, several dialog wrappers) and
custom reimplementations of a `src/components/ui` shadcn primitive. Each duplicate is a drift
risk and a maintenance multiplier; name the canonical one to keep.

## Step 3 — Complexity hot spots

Flag components over the project's ~300-line / single-responsibility guidance, with high prop
counts, or deep conditional branching. Pair complexity with coverage: **high complexity + low
coverage** is the top triage signal.

## Step 4 — Usage signals

Count imports of each shared component across the repo. Flag **orphans** (defined, imported
nowhere) and **one-offs** (a page-local component duplicating an existing shared atom — a
missed-adoption signal, usually a system gap rather than the author's fault).

## Produce the report

```
## Component Audit — <roots>

**<🟢/🟡/🟠/🔴> Headline.** <Posture + the first component to fix.>

| Dimension | Health | Note |
|-----------|--------|------|
| Test coverage | … | e.g. 31 of 84 components have a co-located test |
| Duplication | … | … |
| Complexity | … | … |
| Adoption of shared atoms | … | … |

### Findings (worst first)
**<🔴/🟠/🟡/⚪> <Component> (CR<n>)**
- Evidence: `src/components/organisms/FooDialog.tsx` — 412 lines, no test, 14 props
- Why it matters: <one line>
- Remediation: <specific: add focus-trap test / fold into ui/dialog / split out X>
- Caveat: <one, or omit>

### Scope
- Inspected: <roots scanned; test glob; import scan>
- Not inspected: prop-API depth (→ component-api-validator); design fidelity (→ design-to-code-check)
- Assumed: <defaults used>
- Mark any finding accepted in `.ds-ops-config.yml › accepted_findings` to skip it next run.
```

## Closing note

A duplicate or a fat component may be a deliberate, isolated exception. Flag those and add
their fingerprints to `accepted_findings`; future runs skip them.

## Quality checks

- [ ] Coverage gaps ordered by Challenge Rating, not alphabetically.
- [ ] Every finding cites a path + the measurable signal (lines / props / import count / test?).
- [ ] Duplicates name the canonical component to keep.
- [ ] Output-discipline self-check passed (labels only, headline-first, scope note, peer tone).
