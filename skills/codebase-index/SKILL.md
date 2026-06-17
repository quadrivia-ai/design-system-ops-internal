---
name: codebase-index
description: "Emit a machine-readable index of the design system under `.ai/index/` — a component inventory, a token inventory, a relationship graph, and query protocols — for agents and tooling to consume. Triggers on 'build a codebase index', 'generate an AI index of the design system', 'emit the .ai/index files', 'make the system machine-readable'. This EMITS data artefacts; it is NOT a health report. Do NOT trigger for component health/quality findings — use component-audit. Do NOT trigger for a holistic assessment — use system-health. Do NOT trigger for human-facing context files for an AI agent — use context-engine-builder."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/ai-readiness.md
  - ../../knowledge-notes/context-engine-blueprints.md
---

# Codebase Index

## Context

Scans the design system and writes a structured, machine-readable index to `.ai/index/`:
inventory + relationship graph + query protocols an agent can follow. It is an **emitter**,
not an auditor — it makes no quality judgements and produces no scores. It only reads source
and writes local index files; no code execution, no network calls, no data egress.

## Configuration

Reads (all optional): `target_repo`; `stack.component_roots`; `stack.token_files`;
`output.report_dir` (the index goes under `.ai/index/` in the target repo by default).

## Auto-pull integrations

None. Source files only.

## Step 0 — Scan

Enumerate components (under the roots), tokens (in the token files), and supporting modules
(services, hooks) reachable from them. Record paths.

## Step 1 — Build the inventories

For each component: path, tier (ui/atom/molecule/organism), a one-line purpose inferred from
code, prop names + types, whether a co-located test exists, and an imported-by count. For
each token: name, tier, value(s) per theme. **Mark every inferred field as inferred** — never
present a guess as ground truth (see [ai-readiness](../../knowledge-notes/ai-readiness.md)).

## Step 2 — Build the relationship graph

Record import/composition edges (component → component, component → token, component →
service/hook) so a consumer can answer "what uses X" and "what does X depend on".

## Step 3 — Write query protocols

Emit a short protocol doc telling an agent how to use the index: how to find the component
for a need, how to resolve a token, how to traverse the graph. Blueprint in
[context-engine-blueprints](../../knowledge-notes/context-engine-blueprints.md).

## Produce the index

Write (local only):

```
.ai/index/
├── components.yml        # inventory: path, tier, purpose(inferred), props, test?, used_by
├── tokens.yml            # inventory: name, tier, values per theme
├── graph.yml             # edges: uses / used-by / depends-on
└── query-protocols.md    # how an agent should query the above
```

Then print a short summary (no scores):

```
## Codebase Index — written to .ai/index/

Indexed: <N components, M tokens, E edges>. Inferred fields flagged in-file.

Sample (components.yml):
  - name: Button
    path: src/components/ui/button.tsx
    tier: ui
    purpose: "primary action trigger"   # inferred
    props: [variant, size, asChild, disabled]
    test: true
    used_by: 142

### Scope
- Inspected: <roots, token files, modules scanned>
- Not inspected: runtime behaviour; anything outside the scanned globs
- Assumed: purpose/intent fields are inferred from code, flagged as such
- Regenerate any time; delete `.ai/index/` to reset.
```

## Closing note

The index is generated data — review it, then commit it (or `.gitignore` it) as your team
prefers. Re-run after structural changes to keep it fresh.

## Quality checks

- [ ] Output is valid YAML/Markdown under `.ai/index/`; no quality scores anywhere.
- [ ] Every inferred field is explicitly marked inferred.
- [ ] Only source was read; only `.ai/index/` files were written; nothing sent anywhere.
- [ ] Scope note lists exactly what was scanned.
