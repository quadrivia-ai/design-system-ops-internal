---
name: usage-guidelines
description: "Write when-to-use / when-not-to-use guidance for a component or token — including edge cases and anti-patterns derived from real misuse found in the codebase. Triggers on 'when should I use X', 'usage guidelines for the Dialog', 'when not to use this component', 'document the anti-patterns for X'. Do NOT trigger for the full component API spec — use ai-component-description. Do NOT trigger for a composed pattern — use pattern-documentation. Do NOT trigger for the which-component-for-which-need tree — use component-decision-tree."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/component-bestiary-reference.md
---

# Usage Guidelines

## Context

Answers "should I use this here, and how do I avoid the common mistakes" for a component or
token. Its anti-patterns aren't generic — they're drawn from real misuse found in the
codebase, which makes the guidance land.

## Configuration

Reads (all optional): `stack.component_roots`; `target_repo` (to find real usage/misuse);
`output.report_dir`.

## Auto-pull integrations

None.

## Step 0 — Gather real usage

Find where the subject is used (and misused) across the repo. **Diagnostic mode:** with no
usage evidence (a brand-new or unused component), say so and write provisional guidance
labelled as such, rather than presenting it as battle-tested.

## Step 1 — When to use / when not

The cases this is the right choice, and the cases where a *different* component/token is
better (name it). Boundaries are the most useful part.

## Step 2 — Edge cases + anti-patterns

Edge cases that trip people up, and anti-patterns observed in the code — each with the real
example and the correction.

## Produce the guidelines

```
## Usage — <component/token>

**Headline.** <The one mismatch people make + what to use instead.>

### Use when
- <case> …
### Don't use when
- <case> → use `<alternative>` instead

### Edge cases
- <case> — <how to handle>

### Anti-patterns (from the codebase)
**❌ <anti-pattern>** — seen at `src/.../X.tsx:NN`
- ✅ Instead: <correction>

### Scope
- Evidence: <usage/misuse found, or "no usage yet — provisional">
```

## Closing note

The "Don't use when" list is the highest-value section — it's what stops the next misuse.
Ground anti-patterns in real call sites so they're undeniable, and update as new misuse
appears.

## Quality checks

- [ ] Includes explicit "don't use when" with named alternatives.
- [ ] Anti-patterns cite real `file:line`, or guidance is labelled provisional.
- [ ] Output-discipline self-check passed.
