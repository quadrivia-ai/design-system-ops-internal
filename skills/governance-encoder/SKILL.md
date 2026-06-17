---
name: governance-encoder
description: "Emit machine-readable governance rules — token/naming/API/a11y constraints — as a structured ruleset that agents and tooling can read and enforce, derived from the system's conventions and best practice. Triggers on 'encode our governance rules', 'machine-readable design system rules', 'generate enforceable rules for agents', 'governance ruleset'. Do NOT trigger for a human contribution process — use contribution-workflow. Do NOT trigger for a CI workflow file — use cicd-integration (this emits the rules; that wires the gates)."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/human-oversight-framework.md
  - ../../knowledge-notes/ai-readiness.md
---

# Governance Encoder

> **Proposal only.** Emits a ruleset to review; enforces nothing itself. How rules are
> enforced (lint, CI, agent) is a human decision (see
> [human-oversight-framework](../../knowledge-notes/human-oversight-framework.md)).

## Context

Turns the system's conventions into a structured, machine-readable ruleset that agents and
tooling can consume — so "how we build here" is enforceable, not folklore. Rules combine
observed conventions with best practice; anything inferred is flagged so a human can confirm
it before it's enforced.

## Configuration

Reads (all optional): `stack.lint.token_rules` (existing enforcement to encode); `stack.*`
conventions; `output.report_dir`.

## Auto-pull integrations

None.

## Step 0 — Gather the rules

Collect from existing enforcement (the ESLint token rules), documented conventions, and
best-practice defaults. Tag each rule's origin: `enforced` (already in lint), `documented`,
or `inferred` (per [ai-readiness](../../knowledge-notes/ai-readiness.md)).

## Step 1 — Encode

Express each as a structured rule: id, scope (token/naming/api/a11y), the constraint, severity,
and origin. Keep it tool-agnostic so lint, CI, or an agent can read it.

## Produce the ruleset

```yaml
# .ds-ops/governance.yml — GENERATED · review before enforcing · origins tagged
version: 1
rules:
  - id: tokens.no-raw-color
    scope: token
    constraint: "no hex/rgb or raw Tailwind colour classes in src/**; use semantic tokens"
    severity: high
    origin: enforced        # already in no-hardcoded-tailwind-colors
  - id: tokens.semantic-tier-only
    scope: token
    constraint: "code references semantic/component tokens, never primitives"
    severity: high
    origin: inferred        # confirm before enforcing
  - id: a11y.cr7-audit
    scope: a11y
    constraint: "CR7+ components require an accessibility-per-component pass + a decision-record"
    severity: critical
    origin: documented
  - id: api.variant-enum
    scope: api
    constraint: "visual variants use a `variant` string-union prop"
    severity: medium
    origin: inferred
```

Then a short note: which rules are `inferred` and need human confirmation before enforcement,
and how each could be enforced (lint / CI / agent).

```
### Scope
- Encoded from: <existing lint rules + conventions + best-practice defaults>
- Not done: nothing enforced; you decide where each rule runs
- Inferred rules flagged — confirm before turning them on
```

## Closing note

Turn on `enforced` and `documented` rules first; review the `inferred` ones with the team
before they start failing builds. A rule nobody agreed to is a surprise, not governance.

## Quality checks

- [ ] Every rule tagged `enforced` / `documented` / `inferred`.
- [ ] Inferred rules called out for confirmation; nothing auto-enforced.
- [ ] Ruleset is valid YAML, tool-agnostic.
- [ ] Output-discipline self-check passed.
