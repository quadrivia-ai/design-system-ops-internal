---
name: governance-review-agent
description: "Chained agent for a periodic governance review: runs adoption-report and drift-detection (the internal assessment), then stakeholder-brief (the leadership translation) — one cycle producing both the engineering view and the business view. Triggers on 'run a governance review', 'quarterly design-system review', 'review cycle with a leadership summary'. Do NOT trigger for a deep technical diagnostic — use full-system-diagnostic. Do NOT trigger for just the brief — use stakeholder-brief directly."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/agent-orchestration-guide.md
  - ../../knowledge-notes/human-oversight-framework.md
---

# Governance Review (agent)

> **Proposal only.** Produces assessment and recommendations; makes no changes. Decisions and
> investment are the human's.

## Context

One review cycle that produces both halves of a governance conversation: the internal
assessment (how adopted, how much drift) and the leadership translation (what it means for the
business). See [agent-orchestration-guide](../../knowledge-notes/agent-orchestration-guide.md).

## Step 1 — Internal assessment

Run `adoption-report` (coverage vs adoption, at-risk areas) and `drift-detection` (divergence +
cause classification, especially system-gaps). These are read-only; run both.

## Step 2 — Synthesise the engineering view

Correlate: are at-risk adoption areas the same places drift concentrates? System-gaps from
drift-detection become the system's backlog. State corroboration where a pattern shows up in
both.

## Step 3 — Leadership translation

Feed the synthesis to `stakeholder-brief` — risk/cost/velocity/investment, no jargon, no
component names. The brief must follow from the assessment, not restate it technically.

## Produce the review

```
## Governance Review — <repo> (<period>)

**<🟢/🟡/🟠/🔴> Headline.** <Overall posture + the one decision for leadership.>

### Engineering view
- Adoption: <coverage vs trend; at-risk areas>
- Drift: <volume + dominant cause; system-gaps to own>
- Correlated: <where at-risk adoption and drift coincide>

### Leadership brief (one page, business language)
<from stakeholder-brief — risk / cost / velocity / recommended investment>

### Human-owned next steps
1. … (engineering)   2. … (leadership decision)

### Scope
- Ran: adoption-report, drift-detection, stakeholder-brief
- Assumed: <baseline vs trend; period reviewed>
- Nothing changed; this informs decisions, it doesn't make them.
```

## Quality checks

- [ ] Both views present: engineering assessment AND leadership brief.
- [ ] The brief follows from the assessment; no jargon/component names in it.
- [ ] Correlations stated where adoption-risk and drift coincide.
- [ ] Proposal-only; output-discipline self-check passed.
