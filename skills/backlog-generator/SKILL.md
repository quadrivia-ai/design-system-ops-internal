---
name: backlog-generator
description: "Convert audit/validate findings into structured, estimated work items ready for a tracker — title, context, acceptance criteria, effort size, and a priority derived from each finding's severity. Triggers on 'turn these findings into tickets', 'make a backlog from the audit', 'generate work items', 'backlog from drift-detection'. Do NOT trigger for producing the findings themselves — run the relevant audit/validate skill first. Do NOT trigger for release messaging — use change-communication."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/component-governance.md
---

# Backlog Generator

## Context

Takes the findings from any audit/validate report and turns them into work items a team can
pick up: each with enough context to act, acceptance criteria, an effort size, and a priority.
It transforms findings; it does not generate them.

## Configuration

Reads (all optional): the source findings (pasted or from `output.report_dir`);
`severity_overrides`.

## Auto-pull integrations

None.

## Step 0 — Ingest findings

Take findings with their evidence and severity. Group related ones into a single item where
fixing them together is cheaper (e.g. all hardcoded colours in one feature folder).

## Step 1 — Shape each work item

- **Title** — imperative, specific.
- **Context** — the finding's evidence (`file:line`, counts) so the picker needs no archaeology.
- **Acceptance criteria** — what "done" looks like, testably.
- **Effort** — S / M / L (a size, never a points-as-grade).
- **Priority** — from severity: 🔴 Critical → P1 · 🟠 High → P2 · 🟡 Medium → P3 · ⚪ Low → P4.

## Step 2 — Sequence

Order by priority, then by leverage (items that unblock others first — e.g. close an
enforcement gap before hand-fixing its violations).

## Produce the backlog

```
## Backlog — from <source report>

**Headline.** <N items; the P1 to start.>

### P1
**<title>**
- Context: `src/.../X.tsx:NN` — <evidence>
- Acceptance: [ ] <testable outcome>
- Effort: M
### P2 … P4
<same shape>

### Scope
- Source: <which report / findings>
- Assumed: priority mapped from severity; effort is a rough size
```

## Closing note

Group ruthlessly — ten "replace a hex" findings in one file is one ticket, not ten. And put
the enforcement fix above the cleanup it prevents recurring.

## Quality checks

- [ ] Every item carries evidence + testable acceptance criteria.
- [ ] Effort is S/M/L; priority maps from severity (no invented scores).
- [ ] Related findings grouped; enforcement-before-cleanup ordering applied.
- [ ] Output-discipline self-check passed.
