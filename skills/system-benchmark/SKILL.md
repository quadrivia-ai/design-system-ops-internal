---
name: system-benchmark
description: "Compare this design system against named external reference systems (e.g. Material, Polaris, Primer, Carbon, the Radix/shadcn ecosystem) across ~12 dimensions, reporting where we lead, match, or lag. Triggers on 'benchmark our design system', 'how do we compare to Material/Polaris', 'where do we lag industry practice', 'compare us to other design systems'. Do NOT trigger for an internal-only health check — use system-health. Do NOT trigger for a single dimension (tokens/components) — use token-audit / component-audit."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/component-governance.md
  - ../../knowledge-notes/adoption-measurement.md
---

# System Benchmark

## Context

Positions this system against the published practices of named external reference systems
across roughly twelve dimensions. The references embody best practice, so this is the most
externally-anchored audit in the pack — but it is calibrated to **peer scale**: compare to a
system of similar size and purpose, and flag where an enterprise practice would be
cargo-culting at our scale.

## Configuration

Reads (all optional): `maturity_level`; `stack.*`; `severity_overrides`; `accepted_findings`.

## Auto-pull integrations

None required. Comparison uses the reference systems' publicly documented practices as the
yardstick; if GitHub is configured, MAY enrich our side with repo signals.

## Step 0 — Pick references and a peer band

Name 2–3 reference systems and state why they're the relevant comparators (similar
scale/purpose). Note explicitly that the comparison is against published practice, not a
controlled study.

## Steps 1–12 — Score each dimension (label, not number)

For each dimension: our state, the reference norm, and a health label.

1. Token architecture (tiering, DTCG/interchange)
2. Theming & modes
3. Component coverage & primitives
4. Accessibility baseline
5. Documentation (human + machine)
6. Contribution model
7. Versioning & release discipline
8. Adoption tooling & measurement
9. Design-source (Figma) integration
10. AI-readiness / metadata
11. Testing & visual regression
12. Governance & decision records

## Step 13 — Synthesise

Where we **lead**, **match**, and **lag**; the top 3 lagging dimensions worth closing, each
with the realistic next step at our scale (not "adopt Carbon's entire governance").

## Produce the report

```
## System Benchmark — vs <references>

**<🟢/🟡/🟠/🔴> Headline.** <Overall standing + the one gap most worth closing.>

| Dimension | Us | Reference norm | Standing |
|-----------|----|----------------|----------|
| Token architecture | semantic tier, no primitives | 3-tier + DTCG | 🟠 lag |
| Theming & modes | 3 themes incl. colorblind | light/dark | 🟢 lead |
| … | … | … | … |

### Lead / Match / Lag
- Lead: <dimensions>
- Match: <dimensions>
- Lag (top 3 to close): 1) … 2) … 3) … — each with a scale-appropriate next step.

### Scope
- Inspected: <our signals scanned; reference systems used as yardstick>
- Not inspected: <reference internals we can't see; this is published-practice comparison>
- Assumed: peer band = <named>; enterprise-only practices flagged as such
- Mark accepted gaps in `.ds-ops-config.yml › accepted_findings`.
```

## Closing note

Lagging a reference is only worth closing if it serves our users at our scale — call out any
gap that's fine to leave open, and let the team accept it.

## Quality checks

- [ ] References named, with a stated reason they're the right peers.
- [ ] Every dimension shows our state beside the reference norm.
- [ ] Lagging items have scale-appropriate next steps, not wholesale enterprise adoption.
- [ ] Output-discipline self-check passed.
