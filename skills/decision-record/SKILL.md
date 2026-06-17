---
name: decision-record
description: "Capture an architectural decision as a structured record — context/problem, options considered with trade-offs, the decision, and its consequences. Triggers on 'write an ADR', 'record this decision', 'document why we chose X', 'decision record for the token rename'. Do NOT trigger for a full contribution or deprecation process — use those skills (a decision-record may accompany them). Do NOT trigger for release messaging — use change-communication."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/component-governance.md
---

# Decision Record

## Context

Produces one architectural decision record (ADR): the durable artefact that explains *why* a
choice was made so future readers don't relitigate it. Factual and concise — it records, it
doesn't sell.

## Configuration

Reads (all optional): `output.report_dir` (where ADRs are written, if persisted).

## Auto-pull integrations

None.

## Step 0 — Frame the decision

State the problem and why a decision is needed now. Capture the constraints in play.

## Step 1 — Lay out the options

2–4 real options, each with honest trade-offs (not a strawman + the chosen one). Include the
"do nothing" option where relevant.

## Step 2 — Record the decision and consequences

The choice, the reasoning, and the consequences — both the benefits and the costs/risks
accepted. Note what becomes easier and what becomes harder.

## Produce the record

```
## ADR-<NNN>: <title>

**Status:** proposed | accepted | superseded  ·  **Date:** <YYYY-MM-DD>

### Context
<the problem + constraints>

### Options considered
1. **<Option>** — trade-offs: <+ / −>
2. **<Option>** — trade-offs: <+ / −>

### Decision
<what was chosen, and why over the alternatives>

### Consequences
- Easier: <…>
- Harder / accepted cost: <…>
- Follow-ups: <…>
```

## Closing note

Keep it short enough that people read it. An ADR's value is being findable later — give it a
number and store it with the system.

## Quality checks

- [ ] Options are real alternatives with honest trade-offs, not strawmen.
- [ ] Consequences record the accepted costs, not only the upside.
- [ ] No scores or grades; status + date present.
- [ ] Output-discipline self-check passed (headline-first, peer tone).
