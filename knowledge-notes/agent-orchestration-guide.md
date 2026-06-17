# Agent Orchestration Guide (reference)

How the four chained agents sequence skills, gate between stages, and synthesise findings.
Loaded by `full-system-diagnostic-agent`, `component-to-release-agent`,
`governance-review-agent`, `migration-agent`. Pairs with
[human-oversight-framework](human-oversight-framework.md) and [output-discipline](output-discipline.md).

## Agents are proposal-only

Every agent opens with the [human-oversight-framework](human-oversight-framework.md) banner.
An agent chains *analysis* skills and synthesises; it never applies a change.

## Chaining patterns

- **Sequential with gates** — run skill A, then B, then C. Between stages a **gate** decides
  whether to continue (hard-stop on a 🔴 Critical; skip a stage that isn't applicable).
- **Hard stop** — `component-to-release-agent` stops at the first 🔴 and reports
  release-blocked; it does not spend the later stages.
- **Small-system gate** — `full-system-diagnostic-agent` checks size first: under ~5
  components it stops and recommends a single skill instead of a full diagnostic.

## Cross-skill synthesis

The value of an agent is the synthesis, not the concatenation. After the stages run,
correlate findings across skills and report confidence by corroboration:

| Independent skills flagging the same root | Confidence |
|-------------------------------------------|------------|
| 2 | possible |
| 3 | probable |
| 4+ | confirmed systemic |

State the corroboration count behind any "systemic" claim. (This mirrors `session-memory`'s
correlation rule, so single-run synthesis and cross-run trends speak the same language.)

## Report shape

An agent's output is one consolidated report: a single headline, the per-stage labels, the
synthesised systemic findings (with corroboration counts), and a prioritised, human-owned
action list. It obeys output-discipline like any other report — labels only, scope note, the
full-system-audit length ceiling.
