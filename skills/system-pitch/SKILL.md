---
name: system-pitch
description: "Build the investment case for the design system — value narrative, cost/ROI framing, and rebuttals to the common objections ('we can't afford it', 'it slows teams down', 'designers will feel constrained'). Triggers on 'make the case for the design system', 'pitch the design system investment', 'ROI of the design system', 'why should we fund this'. Do NOT trigger for a neutral one-page status brief — use stakeholder-brief. Do NOT trigger for onboarding — use the onboarding skills."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/adoption-measurement.md
---

# System Pitch

## Context

The persuasive case for investing in the design system: why it pays, framed in cost/ROI terms,
with direct rebuttals to the objections that usually kill the funding. Honest — it argues from
real signals, and concedes where the case is weaker rather than overselling.

## Configuration

Reads (all optional): prior findings / adoption data (`output.report_dir`, `session-memory`).

## Auto-pull integrations

None.

## Step 0 — Anchor in evidence

Ground the pitch in real signals (coverage, rework, duplication, onboarding time). A pitch
without evidence is a wish; cite what you have and be honest about what you're estimating.

## Step 1 — Value narrative + ROI

The before/after story and the ROI framing: where time/cost is recovered (less rework, faster
onboarding, fewer one-off builds, safer rebrand/theming), against the cost of investing.

## Step 2 — Rebut the standard objections

- *"We can't afford it now"* → the cost of *not* investing (compounding inconsistency, rework).
- *"It'll slow teams down"* → the difference between upfront setup and steady-state speed-up.
- *"Designers/engineers will feel constrained"* → systems remove drudgery, not creativity;
  escape hatches exist.
- *"We tried before"* → what's different this time (evidence, scope, ownership).

## Produce the pitch

```
## The Case for the Design System

**The ask.** <What you want funded, in one line.>

### Why it pays
<value narrative + ROI framing, grounded in evidence>

### What it costs
<honest cost of the investment>

### Objections, answered
- "<objection>" → <rebuttal>

### Where the case is weaker
<honest concessions — builds credibility>

— Scope: evidence from <sources>; estimates marked as such.
```

## Closing note

The strongest pitch concedes its weak points — leaders trust a case that names its own risks
over one that pretends there are none. Mark every estimate as an estimate.

## Quality checks

- [ ] ROI grounded in real signals; estimates labelled as estimates.
- [ ] The common objections are named and answered.
- [ ] Includes an honest "where it's weaker" section; no fabricated numbers.
- [ ] Output-discipline self-check passed.
