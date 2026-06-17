# Adoption Measurement (reference)

How to measure whether the design system is actually *used*, and the crucial distinction
between coverage and adoption. Loaded by `adoption-report`, `system-health`,
`system-benchmark`. Pairs with [output-discipline](output-discipline.md).

## Coverage ≠ adoption

- **Coverage** — of the surfaces that *could* use the system, how many *do*. A static,
  structural ratio (shared-component usage vs one-off local UI).
- **Adoption** — the *trend* of active usage over time, and whether teams are moving toward
  or away from the system.

Reporting one as the other is the most common adoption-metric error. Report them separately,
each with its own health label.

## Measuring from static signals

Without analytics, derive coverage from the repo:

- Import counts of shared components vs the count of page-local components that duplicate a
  shared atom (a one-off is negative coverage).
- Token usage vs raw-value usage (cross-reference `token-compliance`).
- Per-area breakdown (by route / feature folder) to locate lagging areas.

## Baseline-establishment protocol (first run)

With no history, **do not invent a trend.** On first run, record current coverage as a
**baseline** snapshot (via `session-memory`), say explicitly that it is a baseline, and report
coverage only. Trends appear from the second run onward.

## At-risk areas

Flag areas where shared-component usage is low *and* local one-offs are high — most likely to
drift, and where enablement (docs, onboarding, comms) pays off. Frame as a system enablement
opportunity, not a team failing.
