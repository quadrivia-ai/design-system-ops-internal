---
name: session-memory
description: "Persist findings across runs so the pack can track change over time — Save a run snapshot, Recall prior runs, Compare two runs for deltas, and Correlate findings across skills/runs (2 = possible, 3 = probable, 4+ = confirmed systemic). Triggers on 'save these findings', 'compare to last run', 'what changed since the last audit', 'has this regressed', 'correlate findings'. Do NOT trigger for generating findings — run the audit/validate skill first. Do NOT trigger for converting findings to tickets — use backlog-generator."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/agent-orchestration-guide.md
  - ../../knowledge-notes/adoption-measurement.md
---

# Session Memory

## Context

The pack's memory across runs. It stores run snapshots locally and answers "what changed" and
"is this systemic". It only reads and writes local snapshot files under the configured trend
directory — no execution, no network, no egress.

## Configuration

Reads/writes: `output.trend_dir` (default `.ds-ops/history`). Reads `severity_overrides`.

## Auto-pull integrations

None. Local snapshots only.

## The four operations

- **Save** — write the current run's findings as a timestamped snapshot (skill, findings with
  fingerprints + severity, counts). Timestamp is supplied by the caller, never invented.
- **Recall** — list/load prior snapshots for a skill or date.
- **Compare** — diff two runs into **fixed** (gone), **new** (appeared), **persistent**
  (still present). This is the regression/progress view.
- **Correlate** — group findings that share a root across skills and runs, and rate confidence
  by corroboration (shared with [agent-orchestration-guide](../../knowledge-notes/agent-orchestration-guide.md)):

  | Independent skills/runs flagging the same root | Confidence |
  |-----------------------------------------------|------------|
  | 2 | possible |
  | 3 | probable |
  | 4+ | confirmed systemic |

## First-run / baseline

With no prior snapshot, there's nothing to compare — **Save a baseline and say so** (per
[adoption-measurement](../../knowledge-notes/adoption-measurement.md)). Never fabricate a
trend from a single run.

## Produce the output

```
## Session Memory — <operation>

**Headline.** <e.g. "3 fixed, 1 new, 5 persistent since 2026-06-01" or "baseline saved">

### Compare (since <prior>)
- ✅ Fixed (3): …
- 🆕 New (1): …
- ➡️ Persistent (5): …

### Correlate
- **confirmed systemic** — raw-value usage flagged by token-compliance, drift-detection,
  component-audit, and theme-audit (4 skills): <root cause>

### Scope
- Snapshots: <which runs loaded from trend_dir>
- Assumed: timestamps provided by caller; no prior run ⇒ baseline only
```

## Closing note

Persistent findings across several runs aren't nagging — they're the signal that a one-off fix
won't hold and the enforcement layer needs the change. Escalate those.

## Quality checks

- [ ] Compare separates fixed / new / persistent.
- [ ] Correlation states the corroboration count behind any "systemic" claim.
- [ ] First run saves a baseline; no fabricated trend.
- [ ] Only local snapshot files read/written; output-discipline self-check passed.
