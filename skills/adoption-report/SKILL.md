---
name: adoption-report
description: "Report design-system adoption — coverage (of surfaces that could use the system, how many do) reported SEPARATELY from adoption (the usage trend) — with at-risk areas flagged and a baseline established on first run. Triggers on 'adoption report', 'how widely is the design system used', 'which areas are lagging on the design system', 'coverage report'. Do NOT trigger for component library health/quality — use component-audit. Do NOT trigger for a leadership business brief — use stakeholder-brief."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/adoption-measurement.md
---

# Adoption Report

## Context

Measures whether the system is actually used, keeping **coverage** and **adoption** strictly
separate (see [adoption-measurement](../../knowledge-notes/adoption-measurement.md)). On the
first run it establishes a baseline rather than inventing a trend. At-risk areas are framed as
enablement opportunities, not team failings.

## Configuration

Reads (all optional): `target_repo`; `stack.component_roots`; `output.trend_dir` (for
baseline/trend via `session-memory`); `severity_overrides`.

## Auto-pull integrations

None required. If GitHub is configured, MAY use history to compute the adoption *trend*; without
it, report coverage only and note the limitation.

## Step 0 — Baseline check

If there's no prior snapshot, this is a **baseline run**: compute coverage, save it (via
`session-memory`), and say explicitly that trend will appear next run. Don't fabricate a trend.

## Step 1 — Coverage (structural)

Of the surfaces that could use the system, how many do: shared-component usage vs page-local
one-offs that duplicate a shared atom; token usage vs raw values. Break down by area
(route/feature folder).

## Step 2 — Adoption (trend) — only with history

Direction over time per area: moving toward or away from the system. Requires prior snapshots
or git history; otherwise mark N/A.

## Step 3 — At-risk areas

Areas with low shared usage *and* high local one-offs — most likely to drift, best ROI for
enablement (docs, onboarding, comms).

## Produce the report

```
## Adoption Report — <repo>  [baseline | trend since <date>]

**<🟢/🟡/🟠/🔴> Headline.** <Coverage posture + the area to help first.>

### Coverage (today)
| Area | Shared usage | One-offs | Coverage |
|------|--------------|----------|----------|
| jobs | 41 imports | 3 | 🟢 |
| care-journeys | 12 | 9 | 🟠 |

### Adoption (trend)
<per-area direction — or "baseline run, no trend yet">

### At-risk areas
- <area> — low shared usage + high one-offs → enablement: <docs/onboarding/comms>

### Scope
- Inspected: <areas, import scan>
- Not inspected: runtime usage analytics
- Assumed: baseline on first run; trend needs history
```

## Closing note

Coverage and adoption answer different questions — a high-coverage area still drifting is a
different problem from a low-coverage area improving. Keep them apart, and treat at-risk areas
as places to help, not to name and shame.

## Quality checks

- [ ] Coverage and adoption reported separately, each with its own label.
- [ ] First run is a labelled baseline; no fabricated trend.
- [ ] At-risk areas framed as enablement, with the specific intervention.
- [ ] Output-discipline self-check passed.
