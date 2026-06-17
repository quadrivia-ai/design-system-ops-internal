---
name: naming-audit
description: "Audit naming consistency across components, tokens, files, props, and patterns — and flag cross-tier collisions (a name that reads semantic but holds a raw value, or one name meaning two things). Triggers on 'audit our naming', 'are our component/token names consistent', 'naming conventions check', 'find naming collisions'. Do NOT trigger for token tier STRUCTURE or theme parity — use token-audit. Do NOT trigger for prop-API shape validation — use component-api-validator. Do NOT trigger for a holistic sweep — use system-health."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/token-architecture.md
---

# Naming Audit

## Context

Checks that names across the system follow one coherent scheme and carry no collisions.
Consistency and clarity are the bar; the project's documented conventions are the local
expression of that bar, not a substitute for it. Output is a label-based report grouped by
naming domain.

## Configuration

Reads (all optional): `stack.component_roots`; `stack.token_files`; `severity_overrides`;
`accepted_findings`.

## Auto-pull integrations

None.

## Step 0 — Gather names

Collect component names, exported hook/util names, file names, prop names, and token names
across the configured roots and token files.

## Step 1 — Component & file naming

Best practice: one casing scheme per kind, predictable and greppable. Check PascalCase
components, verb+noun handlers (`handleClick`), boolean `is/has/should/can`, and file/barrel
conventions. Flag outliers with their counter-examples.

## Step 2 — Token naming

Check the semantic scheme is internally consistent (the `fg` / `fg-secondary` / `fg-muted`
hierarchy; `border-subtle` / `border-strong`). Flag tokens whose name doesn't predict their
tier or intent.

## Step 3 — Pattern naming

Do similar constructs share a suffix/prefix pattern (dialogs `*Dialog`, providers
`*Provider`, stores `use*Store`)? Flag families where the pattern is followed inconsistently.

## Step 4 — Cross-tier & cross-surface collisions

Flag a name that reads semantic but holds a raw value (`--accent-blue`), one identifier
meaning two different things in two places, and a shared name shadowed by a local one.

## Produce the report

```
## Naming Audit — <scope>

**<🟢/🟡/🟠/🔴> Headline.** <How consistent + the highest-leverage rename.>

| Domain | Health | Note |
|--------|--------|------|
| Components & files | … | … |
| Tokens | … | … |
| Patterns | … | … |
| Collisions | … | … |

### Findings
**<🔴/🟠/🟡/⚪> <title>**
- Evidence: `src/.../useFooData.ts` vs `src/.../fooDataHook.ts` — two schemes for one kind
- Why it matters: <one line>
- Remediation: <the specific rename → target name>
- Caveat: <one, or omit>

### Scope
- Inspected: <roots / token files scanned>
- Not inspected: prop-API shape (→ component-api-validator); token structure (→ token-audit)
- Assumed: <defaults used>
- Mark accepted names in `.ds-ops-config.yml › accepted_findings`.
```

## Closing note

Some "inconsistencies" are deliberate domain terms. Confirm and add them to
`accepted_findings` so future runs don't re-flag them.

## Quality checks

- [ ] Every naming finding shows the inconsistent name beside the expected scheme.
- [ ] Collisions distinguish "reads wrong tier" from "two meanings, one name".
- [ ] Renames are specific (from → to), not "consider a clearer name".
- [ ] Output-discipline self-check passed.
