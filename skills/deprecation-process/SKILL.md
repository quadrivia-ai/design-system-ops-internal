---
name: deprecation-process
description: "Plan the safe removal of a component/token/pattern — blast-radius analysis (everything that uses it), a migration path with before/after code, a comms timeline, and a sunset date. Triggers on 'help me deprecate X', 'how do I remove this component', 'sunset the old Button', 'deprecation plan'. Do NOT trigger for adding something — use contribution-workflow. Do NOT trigger for generating the actual transform script — use codemod-generator (this plans; that emits)."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/human-oversight-framework.md
  - ../../knowledge-notes/component-governance.md
---

# Deprecation Process

> **Proposal only.** Plans a removal; removes nothing. The codemod is emitted separately by
> `codemod-generator`, and applying it is a human decision (see
> [human-oversight-framework](../../knowledge-notes/human-oversight-framework.md)).

## Context

Produces a safe sunset plan for something the system wants to retire: who's affected, how they
migrate, when it happens, and how to back out. The blast radius drives everything — a token
used in 4 files and one used in 400 are different conversations.

## Configuration

Reads (all optional): `target_repo`; `stack.component_roots`; `stack.token_files`;
`severity_overrides`.

## Auto-pull integrations

None required. If GitHub is configured, MAY use history to spot recent adopters who'd be most
surprised.

## Step 0 — Identify the target and its replacement

Name what's being deprecated and what replaces it. If there's no replacement, say so — that's
a blocker to surface, not to paper over.

## Step 1 — Blast-radius analysis

Find every usage across the repo. Report the count, the spread (how many files/areas), and the
highest-risk call sites. Severity scales with blast radius.

## Step 2 — Migration path (before/after)

Concrete `old → new` code examples covering the common cases and the awkward ones. Note where
the replacement isn't a drop-in (behaviour differences).

## Step 3 — Timeline and sunset

A phased timeline: **announce → soft-deprecate (warn) → migrate → sunset (remove)**, with a
concrete sunset date and rollback checkpoints at each phase. Hand the transform to
`codemod-generator` and the messaging to `change-communication`.

## Produce the plan

```
## Deprecation Plan — <target> → <replacement>

**<🟢/🟡/🟠/🔴> Headline.** Blast radius: <N usages across M files>. <First move + sunset date.>

### Migration (before → after)
```tsx
// before
<OldButton kind="primary" />
// after
<Button variant="default" />
```
### Timeline
- Announce: <date>  · Warn: <date>  · Migrate by: <date>  · Sunset: <date>
- Rollback checkpoint at each phase: <what to revert>

### Highest-risk call sites
- `src/.../X.tsx:NN` — <why risky>

### Scope
- Inspected: <usage scan>
- Not done: nothing removed; codemod is separate (→ codemod-generator)
- Assumed: <replacement is the canonical successor>
```

## Closing note

Don't set a sunset date you won't hold — a deprecation that never sunsets just adds a second
way to do things. If there's no clean replacement, pause the deprecation, not the consumers.

## Quality checks

- [ ] Blast radius quantified (count + spread + risk sites).
- [ ] Before/after migration covers the awkward cases, not just the happy path.
- [ ] Timeline has a real sunset date + rollback checkpoints.
- [ ] Proposal-only framing intact; hands off transform + comms by name.
- [ ] Output-discipline self-check passed.
