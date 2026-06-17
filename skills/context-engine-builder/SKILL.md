---
name: context-engine-builder
description: "Build the broader structured context layer that AI agents consume to work within the design system — a conventions digest, do/don't rules, token/component pointers, and query protocols — layered on top of the codebase index. Triggers on 'build the AI context for our design system', 'create context files for agents', 'set up the context engine', 'make agent-readable conventions'. Do NOT trigger for the raw inventory + relationship graph — use codebase-index (this builds the context layer ON TOP of it). Do NOT trigger for a metadata schema — use metadata-schema-generator."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/ai-readiness.md
  - ../../knowledge-notes/context-engine-blueprints.md
---

# Context Engine Builder

## Context

Builds the *context layer* an agent reads to build conformant UI: the conventions, the
do/don'ts, and the query protocols — sitting on top of the facts that `codebase-index`
emits. Reads source and the index; writes local context files only. No execution, no network,
no egress.

## Configuration

Reads (all optional): `target_repo`; an existing `.ai/index/` (from `codebase-index`); a
conventions guide if present (e.g. `CLAUDE.md`); `output.report_dir`.

## Auto-pull integrations

None.

## Step 0 — Consume the index + conventions

Load `.ai/index/` if present (else recommend running `codebase-index` first for the factual
base). Read any conventions guide. Per [context-engine-blueprints](../../knowledge-notes/context-engine-blueprints.md).

## Step 1 — Distil conventions and rules

A concise conventions digest (naming, tokens-not-raw-values, component-first, theming) and
do/don't rules an agent can apply. Mark anything inferred (see
[ai-readiness](../../knowledge-notes/ai-readiness.md)).

## Step 2 — Write query protocols

The "how to act" protocols: choosing a component, resolving a token, assessing blast radius —
pointing into the index for the facts.

## Produce the context engine

Write (local only) under `.ai/context/`:

```
.ai/context/
├── conventions.md     # naming, tokens, component-first, theming — the rules
├── do-dont.md         # concrete do/don't with examples
└── protocols.md       # how an agent should query index + apply conventions
```

Then a short summary:

```
## Context Engine — written to .ai/context/

Built on: <.ai/index present? conventions guide?>. Inferred rules flagged.

### Scope
- Read: <index, conventions, source>
- Not done: facts inventory (→ codebase-index); schema (→ metadata-schema-generator)
- Assumed: <inferred conventions flagged in-file>
```

## Closing note

The context layer is opinion (how to build); the index is fact (what exists). Keep them
separate files so an agent can trust the facts and weigh the opinions — and regenerate both
after structural change.

## Quality checks

- [ ] Layered on the index facts, not duplicating them; recommends `codebase-index` if absent.
- [ ] Conventions vs facts kept separate; inferred rules flagged.
- [ ] Local files only; nothing executed or sent.
- [ ] Output-discipline self-check passed.
