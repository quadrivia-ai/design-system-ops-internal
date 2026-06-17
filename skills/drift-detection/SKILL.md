---
name: drift-detection
description: "Detect system-wide divergence — visual, behavioural, API, and token drift — across the whole codebase, and classify each instance by cause (intentional / version-lag / accidental / misunderstanding / system-gap). Triggers on 'what's drifted', 'find inconsistencies across the app', 'where are we diverging from the design system', 'detect drift'. Do NOT trigger for one component checked against its design spec — use design-to-code-check. Do NOT trigger for token DEFINITION structure — use token-audit. Do NOT trigger for a holistic health sweep — use system-health."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/design-to-code-contract.md
  - ../../knowledge-notes/token-architecture.md
---

# Drift Detection

## Context

Finds where the codebase has drifted away from its own sources of truth — tokens, shared
components, the shadcn/ui base — and, crucially, **classifies the cause** of each drift so
the reader knows whether to fix the consumer or fix the system. Output is a classified drift
inventory, not a per-component verdict.

## Configuration

Reads (all optional): `target_repo`; `stack.token_files`, `stack.component_roots`,
`stack.ui_library`; `severity_overrides`; `accepted_findings`.

## Auto-pull integrations

None required. If GitHub is configured, MAY use blame/history to distinguish version-lag
(old code) from accidental (recent divergence). Works fully offline.

## Step 0 — Establish sources of truth

Identify the canonical tokens, the shared components, and the shadcn base versions. Drift is
measured against these — not against an external ideal.

## Step 1 — Token drift

The same visual value expressed multiple ways: a raw hex where a token exists, a local
override of a token, two tokens used interchangeably for one intent.

## Step 2 — Component drift

Copies of a shared component that have diverged in markup, behaviour, or styling; instances
that re-style a shared component inline instead of using its variants.

## Step 3 — API drift

The same component invoked with inconsistent prop patterns across call sites (boolean vs
enum for the same concept, differing event-handler names).

## Step 4 — Visual & behavioural drift

Spacing, radius, typography, or interaction variations for the same intent (e.g. three
different card paddings; two different toast durations).

## Step 5 — Classify every drift

Tag each instance with a cause — this is the point of the skill:

- **Intentional** — a deliberate, contextual exception. Invite acceptance.
- **Version-lag** — consumer predates the current system version. Migration candidate.
- **Accidental** — a one-off mistake. Quick fix.
- **Misunderstanding** — the consumer didn't know the system affordance existed. Docs/comms gap.
- **System-gap** — the system lacks the thing, so people improvise. **This is a finding
  against the system, not the consumer** — feed it to `backlog-generator`.

## Produce the report

```
## Drift Detection — <repo>

**<🟢/🟡/🟠/🔴> Headline.** <How divergent + the dominant cause + first move.>

| Drift type | Instances | Dominant cause | Health |
|------------|-----------|----------------|--------|
| Token | 14 | accidental | 🟠 |
| Component | 6 | system-gap | 🟠 |
| API | 9 | misunderstanding | 🟡 |
| Visual | 11 | version-lag | 🟡 |

### Notable instances
**<🔴/🟠/🟡/⚪> <title> — [cause]**
- Evidence: `src/pages/.../X.tsx:88` vs canonical `src/components/ui/card.tsx`
- Why it matters: <one line>
- Remediation: <fix consumer | migrate | file system-gap to backlog-generator>
- Caveat: <one, or omit>

### System-gaps surfaced
<the drifts classified system-gap — the system's to-do, not the consumer's>

### Scope
- Inspected: <sources of truth; files/globs scanned>
- Not inspected: single-component spec fidelity (→ design-to-code-check)
- Assumed: <classification uses static signals; confirm version-lag against history if unsure>
- Mark intentional drift accepted in `.ds-ops-config.yml › accepted_findings`.
```

## Closing note

The **intentional** bucket is yours to confirm — add those fingerprints to
`accepted_findings`. The **system-gap** bucket is the design system's homework, not the
consumers'.

## Quality checks

- [ ] Every drift instance carries a cause classification.
- [ ] System-gaps are separated out and framed as the system's responsibility.
- [ ] Evidence pairs the drifted site with the canonical source.
- [ ] Output-discipline self-check passed.
