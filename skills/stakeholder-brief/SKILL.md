---
name: stakeholder-brief
description: "Produce a one-page, business-language brief on the design system for non-technical stakeholders — framed as risk, cost, velocity, and recommended investment, with no jargon and no component names. Triggers on 'write a stakeholder brief', 'one-pager for leadership', 'business summary of the design system', 'brief for my VP'. Do NOT trigger for the full investment/ROI pitch with objection rebuttals — use system-pitch. Do NOT trigger for an onboarding guide — use designer-onboarding / engineering-onboarding."
references:
  - ../../knowledge-notes/output-discipline.md
---

# Stakeholder Brief

## Context

Translates the technical state of the design system into one page a non-technical leader can
act on: where the risk is, what it costs, how it affects delivery speed, and what to invest.
**No jargon, no component names, no token names** — business outcomes only.

## Configuration

Reads (all optional): prior findings (from `output.report_dir` / `session-memory`).

## Auto-pull integrations

None.

## Step 0 — Translate, don't list

Take the technical findings and restate each as a business consequence. "47 hardcoded colours"
→ "inconsistent UI that's slow and risky to rebrand". If you can't tie a finding to a business
outcome, leave it out.

## Step 1 — The four frames

- **Risk** — what could go wrong / is going wrong (inconsistency, accessibility/legal
  exposure, key-person dependence).
- **Cost** — where effort leaks today (rework, duplicated build, slow onboarding).
- **Velocity** — how the system speeds or slows shipping.
- **Recommended investment** — the one or two moves worth funding, and what they buy.

## Produce the brief

```
## Design System — Leadership Brief  (<date>)

**Bottom line.** <One sentence: posture + the single recommended decision.>

**Risk.** <2–3 lines, business terms.>
**Cost.** <2–3 lines: where effort/spend leaks today.>
**Velocity.** <2–3 lines: effect on delivery speed.>
**Recommended investment.** <the 1–2 funded moves + what they return.>

— Scope: based on <which assessments>, as of <date>. Detail available on request.
```

Keep it to one page. Health may be conveyed in words (strong / functional / weak / at-risk),
never as a score.

## Closing note

If a stakeholder has to ask "so what should I do?", the brief failed. Lead with the decision;
make the four frames support it.

## Quality checks

- [ ] One page; no jargon, no component/token names.
- [ ] Every point is a business consequence, not a technical finding.
- [ ] A single clear recommended decision; no numeric scores.
- [ ] Output-discipline self-check passed.
