---
name: migration-agent
description: "Chained agent that plans a system-wide migration: runs token-audit + naming-audit, builds a transformation table (old → new), generates the codemod, and lays out a four-phase rollout (add → migrate → deprecate → remove) with entry/exit criteria, rollback checkpoints, and the change communication. Triggers on 'plan a migration', 'migrate our tokens/components to the new scheme', 'roll out the rename across the codebase'. PROPOSAL ONLY — generates a plan + reviewable codemod; runs nothing. Do NOT trigger for one component's deprecation — use deprecation-process. Do NOT trigger for just the codemod — use codemod-generator."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/agent-orchestration-guide.md
  - ../../knowledge-notes/human-oversight-framework.md
---

# Migration (agent)

> **Proposal only.** Produces a migration plan and a reviewable, dry-run-first codemod. It
> applies nothing — every phase is a human decision (see
> [human-oversight-framework](../../knowledge-notes/human-oversight-framework.md)).

## Context

Plans and stages a system-wide migration end to end: understand the current state, define the
transformation, generate the (dry-run) codemod, and sequence a safe four-phase rollout with
checkpoints and comms. See [agent-orchestration-guide](../../knowledge-notes/agent-orchestration-guide.md).

## Step 1 — Understand

Run `token-audit` + `naming-audit` to map what's changing and the debt around it (banned
aliases, inconsistent names) so the migration cleans up rather than entrenches.

## Step 2 — Transformation table

Build the explicit `old → new` table (token/name/component mappings), including the cases that
*can't* be auto-transformed (dynamic usage) — listed, not hidden.

## Step 3 — Codemod (dry-run-first)

Hand the table to `codemod-generator`: emits the transform + a `migrate.js` that defaults to
dry-run, with a review banner. Generated, not run.

## Step 4 — Four-phase rollout

Sequence with entry/exit criteria and a rollback checkpoint per phase:

1. **Add** — introduce the new (token/component) alongside the old. Exit: new available, nothing broken.
2. **Migrate** — run the codemod (human, dry-run → apply); update call sites. Exit: lint/tests green.
3. **Deprecate** — warn on the old; announce via `change-communication`. Exit: no new usage of old.
4. **Remove** — delete the old after the sunset date. Exit: zero references; rollback = revert the removal commit.

## Produce the migration plan

```
## Migration Plan — <old scheme> → <new scheme>

**Headline.** Blast radius: <N sites>. <First phase + the one risk to watch.>

### Transformation table
| Old | New | Auto? |
|-----|-----|-------|
| `--accent-light` | `bg-accent/15` | yes |
| `<OldButton kind>` | `<Button variant>` | partial |
| dynamic token lookups | — | manual |

### Codemod
- `migrate.js` (dry-run default) + transform — review before applying.

### Four-phase rollout
- **Add** — entry/exit: …  rollback: …
- **Migrate / Deprecate / Remove** — <same shape; sunset date on Remove>

### Scope
- Ran: token-audit, naming-audit; generated codemod (dry-run)
- Manual cases: <listed from the table>
- Nothing applied; each phase is your call.
```

## Quality checks

- [ ] Transformation table includes manual/unsafe cases explicitly.
- [ ] Codemod is dry-run-first with a review banner; nothing applied.
- [ ] Four phases each have entry/exit criteria + a rollback checkpoint; Remove has a sunset date.
- [ ] Proposal-only banner; output-discipline self-check passed.
