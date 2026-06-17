---
name: metadata-schema-generator
description: "Generate a metadata schema for components and/or tokens — the typed shape (fields, types, allowed values) that metadata must follow, which schema-validator then checks against. Triggers on 'generate a metadata schema', 'define the schema for our token metadata', 'create a component metadata schema', 'what shape should our metadata be'. Do NOT trigger for VALIDATING files against a schema — use schema-validator (this generates it). Do NOT trigger for writing the index files themselves — use codebase-index."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/ai-readiness.md
  - ../../knowledge-notes/token-architecture.md
---

# Metadata Schema Generator

## Context

Generates the schema that defines well-formed component/token metadata — the contract
`schema-validator` enforces and `codebase-index` / `context-engine-builder` populate. It
infers a reasonable schema from what the system already expresses, aligned to DTCG for tokens
where sensible, and flags the fields the team should confirm.

## Configuration

Reads (all optional): `stack.component_roots`; `stack.token_files`; existing metadata if any;
`output.report_dir`.

## Auto-pull integrations

None.

## Step 0 — Infer the shape

Survey existing component props/metadata and token structure to infer the natural fields.
Tokens align to DTCG (`$value`, `$type`, `$description`) where it fits (see
[token-architecture](../../knowledge-notes/token-architecture.md)); components to the
[ai-readiness](../../knowledge-notes/ai-readiness.md) axes (name, purpose, tier, props, CR,
test, a11y).

## Step 1 — Emit the schema

A JSON Schema for component metadata and one for token metadata: required vs optional fields,
types, enums (e.g. tier ∈ {primitive, semantic, component}). Keep optional what the system
doesn't yet provide — a schema that fails every existing file is useless.

## Produce the schema

```
## Metadata Schema — components + tokens

**Headline.** <What the schema enforces + which fields are optional-for-now.>

### component.schema.json (excerpt)
```json
{
  "type": "object",
  "required": ["name", "path", "tier"],
  "properties": {
    "name": { "type": "string" },
    "tier": { "enum": ["ui", "atom", "molecule", "organism"] },
    "cr":   { "type": "integer", "minimum": 1 },
    "purpose": { "type": "string" },
    "hasTest": { "type": "boolean" }
  }
}
```
### token.schema.json (DTCG-aligned excerpt)
<$value / $type / $description / tier enum>

### Scope
- Inferred from: <props/tokens surveyed>
- Optional-for-now: <fields the system doesn't yet provide — confirm before requiring>
- Next: validate with `schema-validator`
```

## Closing note

Make required only what the system already provides everywhere; mark the rest optional and let
the team promote fields to required as coverage grows. Then wire `schema-validator` to it.

## Quality checks

- [ ] Required fields are ones the system actually provides; aspirational fields optional.
- [ ] Token schema aligned to DTCG where it fits; component schema to the ai-readiness axes.
- [ ] Valid JSON Schema; pairs with schema-validator by name.
- [ ] Output-discipline self-check passed.
