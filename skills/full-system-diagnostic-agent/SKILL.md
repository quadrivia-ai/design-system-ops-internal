---
name: full-system-diagnostic-agent
description: "Chained agent that runs the deep audit skills (token-audit, component-audit, naming-audit, drift-detection, system-health) and synthesises cross-skill patterns into one diagnostic. Triggers on 'full design-system diagnostic', 'run the whole audit suite', 'deep end-to-end design system review'. Do NOT trigger for a quick breadth read — use system-health alone. Do NOT trigger for a single dimension — use that skill directly."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/agent-orchestration-guide.md
  - ../../knowledge-notes/human-oversight-framework.md
---

# Full System Diagnostic (agent)

> **Proposal only.** Produces analysis and recommendations; makes no changes. Every action
> item requires human decision and execution.

## Context

Runs the deep audit skills in sequence and — crucially — **synthesises** their findings into
cross-skill patterns, rather than concatenating five reports. See
[agent-orchestration-guide](../../knowledge-notes/agent-orchestration-guide.md).

## Step 0 — Small-system gate

Count components under the roots. **If under ~5 components, stop**: a full diagnostic is
overkill — recommend a single skill (usually `token-audit` or `component-audit`) and exit.

## Step 1 — Run the chain

In order, collecting each report: `token-audit` → `component-audit` → `naming-audit` →
`drift-detection` → `system-health`. No gate between them (all are read-only analyses); run
the full set.

## Step 2 — Synthesise

Correlate findings across the five skills by shared root cause. Rate confidence by
corroboration: 2 skills = possible · 3 = probable · 4+ = confirmed systemic. State the
corroboration count behind each systemic claim.

## Produce the consolidated diagnostic

```
## Full System Diagnostic — <repo>

**<🟢/🟡/🟠/🔴> Headline.** Maturity ≈ Level N. <The systemic issue + the first move.>

### Per-skill posture
| Skill | Health | One-line takeaway |
|-------|--------|-------------------|
| token-audit | … | … |
| component-audit | … | … |
| naming-audit | … | … |
| drift-detection | … | … |
| system-health | … | … |

### Systemic findings (corroborated)
- **confirmed systemic** (4 skills): <root> — <why it matters> → <first move>
- **probable** (3 skills): …

### Prioritised, human-owned actions
1. … 2. … 3. …

### Scope
- Ran: the five audit skills (read-only)
- Not done: validation/release checks (→ component-to-release agent); nothing changed
- Assumed: <maturity inferred>
```

Obeys the full-system-audit length ceiling.

## Quality checks

- [ ] Small-system gate evaluated first.
- [ ] Synthesis present — systemic findings with corroboration counts, not five stitched reports.
- [ ] Proposal-only banner; actions are human-owned.
- [ ] Output-discipline self-check passed.
