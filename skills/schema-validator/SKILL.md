---
name: schema-validator
description: "Validate token files, metadata files, or the .ai/index/ artefacts against a schema, reporting PASS/WARN/FAIL per file with the failing path and the expected shape. Triggers on 'validate my tokens against the schema', 'schema-validate the metadata', 'check .ai/index conforms', 'validate token JSON'. Do NOT trigger for GENERATING a schema — use metadata-schema-generator. Do NOT trigger for prop-API consistency — use component-api-validator. Do NOT trigger for token tier-structure/naming quality — use token-audit."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/ai-readiness.md
  - ../../knowledge-notes/token-architecture.md
---

# Schema Validator

## Context

A discrete conformance check: validate target files against a schema and report ✅ PASS /
⚠️ WARN / ❌ FAIL per file, with the exact failing path and what was expected. It checks
*shape*, not quality — token-tier *quality* is `token-audit`'s job.

## Configuration

Reads (all optional): a schema path (config, provided inline, or a recognised standard such
as DTCG); the target files/globs (default: `stack.token_files`, metadata files, `.ai/index/`);
`severity_overrides`.

## Auto-pull integrations

None.

## Step 0 — Resolve schema and targets

Identify the schema (explicit path > inline > DTCG default for token files). Identify the
target files. If no schema can be resolved → diagnostic mode: say so and list candidate
schemas; validate nothing against a guessed shape.

## Step 1 — Validate

For each target, check required keys, types, and allowed values against the schema. Record
every violation with its JSON/path pointer, the expected type/value, and the actual.

## Step 2 — Classify per file

- **✅ PASS** — conforms.
- **⚠️ WARN** — conforms structurally but misses recommended-but-optional fields (e.g. DTCG
  `$description`).
- **❌ FAIL** — a required key/type/value is wrong or missing.

## Produce the report

```
## Schema Validation — <schema> vs <targets>

**Headline.** <How many FAIL + the first file to fix.>

| File | Result | Violations |
|------|--------|------------|
| src/index.css (parsed) | ✅ | 0 |
| .ai/index/tokens.yml | ❌ | 2 |

### Violations
**❌ `.ai/index/tokens.yml`**
- `tokens[3].tier` — expected one of [primitive, semantic, component], got "brand"
- `tokens[7].value` — required, missing

### Scope
- Inspected: <schema used; files validated>
- Not inspected: token quality/structure (→ token-audit)
- Assumed: <schema source; default schema if none provided>
```

## Closing note

A ⚠️ WARN is a recommended-field gap, not a break — fix when convenient. ❌ FAIL means a
consumer relying on the schema will mis-parse; fix before they do.

## Quality checks

- [ ] Per-file result uses ✅/⚠️/❌; no numeric pass-rate.
- [ ] Every violation cites a path pointer + expected vs actual.
- [ ] Diagnostic mode used when no schema resolved — nothing validated against a guess.
- [ ] Output-discipline self-check passed.
