# Context-Engine Blueprints (reference)

Blueprints for the structured context files that AI agents consume to work with the design
system reliably. Loaded by `codebase-index` and `context-engine-builder`. Pairs with
[ai-readiness](ai-readiness.md) and [output-discipline](output-discipline.md).

## What a context engine is

A set of machine-readable files that let an agent answer, without re-deriving from source:
*what exists*, *how it relates*, *how to use it*, and *what the rules are*. Two skills build
it: `codebase-index` (the inventory + graph) and `context-engine-builder` (the broader
context layer on top).

## The `.ai/index/` blueprint

```
.ai/index/
├── components.yml       # name, path, tier, purpose(inferred), props, test?, used_by
├── tokens.yml           # name, tier, values per theme
├── graph.yml            # edges: uses / used-by / depends-on
└── query-protocols.md   # how an agent should query the above
```

## Query-protocol blueprint

The protocol doc tells an agent how to act, e.g.:

- *"Need a component for X"* → search `components.yml` purpose + props; prefer the `ui` tier;
  confirm against `graph.yml` `used_by` before introducing a new one.
- *"Resolve a colour/spacing"* → look up `tokens.yml` by intent; never emit a raw value; use
  the semantic tier.
- *"Assess blast radius of changing X"* → traverse `graph.yml` `used-by`.

## Principles

- **Machine-readable first** — valid YAML/Markdown, stable keys.
- **Inferred fields flagged** — per [ai-readiness](ai-readiness.md); never present a guess as
  fact.
- **Regenerable, not hand-maintained** — re-run the emitter after a structural change.
- **Local only** — read source, write context files; no execution, no network, no egress.
