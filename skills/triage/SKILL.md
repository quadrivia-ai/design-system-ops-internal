---
name: triage
description: "Entry point for a design-system engagement: classify the system's stage (new / growing / established / legacy) and output a prioritised plan of the 3–5 skills to run first, in order, with why. Triggers on 'where do I start', 'triage my design system', 'what should I run first', 'I just inherited this design system'. Do NOT trigger for the actual assessment — triage points to system-health and friends; it does not assess. Do NOT trigger for a single known task ('audit my tokens') — invoke that skill directly."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/component-governance.md
---

# Triage

## Context

A five-minute orientation that decides *what to run*, not *what's wrong*. It reads cheap
signals, classifies the system's stage, and returns an ordered run plan. It never deep-dives —
that's the job of the skills it points to.

## Configuration

Reads (all optional): `target_repo`; `stack.*`; `maturity_level` (else inferred).

## Auto-pull integrations

None.

## Step 0 — Read cheap signals

Component count under the roots; presence of a semantic token layer; automated enforcement
(lint rules); documentation surface; repo age/history if available. Map to a maturity level
(see [component-governance](../../knowledge-notes/component-governance.md)).

## Step 1 — Classify the stage

- **New** (≈ L1–2) — little shared; establish foundations.
- **Growing** (≈ L3) — library in use, enforcement partial; tighten and measure.
- **Established** (≈ L4) — measure adoption, hold the line, govern.
- **Legacy / inherited** — unknown shape; map before judging.

## Step 2 — Emit the prioritised run plan

3–5 skills, ordered, each with a one-line why. Suggested starting maps:

| Stage | Run first (in order) |
|-------|----------------------|
| New | `token-audit` → `component-audit` → set up `contribution-workflow` |
| Growing | `system-health` → `token-audit` + `component-audit` → `drift-detection` → `cicd-integration` |
| Established | `system-health` → `adoption-report` → `drift-detection` → `governance-review` agent |
| Legacy | `codebase-index` (map it) → `system-health` → `naming-audit` → `drift-detection` |

**Small-system gate:** under ~5 components, stop and recommend a single skill (usually
`token-audit` or `component-audit`) — a full plan is overkill.

## Produce the plan

```
## Triage — <repo>

**Stage: <new/growing/established/legacy>** (maturity ≈ Level N). <One line: the first move.>

### Run these, in order
1. `<skill>` — <why first>
2. `<skill>` — <why next>
3. …

### Scope
- Read: <signals used>
- Not done: any assessment — each skill above does its own
- Assumed: <maturity inferred, since no config>
```

## Closing note

This is a starting line, not a verdict. After the first skill runs, re-triage if its findings
change the picture.

## Quality checks

- [ ] A stage classification with the signals behind it.
- [ ] An ordered plan of 3–5 named skills, each with a why (or the small-system gate fired).
- [ ] No assessment performed here — only routing.
- [ ] Output-discipline self-check passed.
