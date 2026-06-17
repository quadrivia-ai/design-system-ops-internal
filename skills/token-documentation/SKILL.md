---
name: token-documentation
description: "Document token INTENT by tier — what each token (or group) is for, its theming contract across light/dark/colorblind, and do/don't usage. Triggers on 'document our tokens', 'write the token reference', 'what is each token for', 'token usage guide'. Do NOT trigger for auditing token structure/debt — use token-audit. Do NOT trigger for scanning code for raw values — use token-compliance. Do NOT trigger for a metadata schema — use metadata-schema-generator."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/token-architecture.md
---

# Token Documentation

## Context

Documents what tokens are *for*, organised by tier, with the theming contract and concrete
do/don't guidance — the reference a developer consults to pick the right token. It documents
intent the tokens actually express; where intent is unclear it flags it rather than inventing
a rationale.

## Configuration

Reads (all optional): `stack.token_files`; `stack.theme_classes`; `output.report_dir`.

## Auto-pull integrations

None.

## Step 0 — Load and group by tier

Read the token files; group into semantic and component tiers (see
[token-architecture](../../knowledge-notes/token-architecture.md)). **Diagnostic mode:** if a
token's intent can't be inferred from its name/value/usage, list it as "intent unclear" rather
than guessing a purpose.

## Step 1 — Document intent + theming contract

For each token/group: its purpose, and how it behaves across `light`/`dark`/`colorblind`
(the theming contract — what stays constant, what re-points).

## Step 2 — Do / Don't

Concrete, token-specific guidance: the right use, and the common wrong use (e.g. "use
`--fg-muted` for timestamps and hints; **don't** use it for body text — that's `--fg`").
Where evidence exists, base the "don't" on real misuse.

## Produce the doc

```
## Token Reference

**Headline.** <The one rule: consume semantic tokens; never raw values.>

### Foreground (`--fg`, `--fg-secondary`, `--fg-muted`)
- Intent: <…>  · Theming: <constant/re-points across themes>
- ✅ Do: <…>   · ❌ Don't: <…>

### Surfaces / Accent / Status / Domain (`--chart-*`, `--outcome-*`, …)
<same shape per group>

### Scope
- Documented: <token files read; groups covered>
- Intent unclear: <tokens that need an owner to clarify>
```

## Closing note

A token reference earns its keep through the Don'ts — the misuse it prevents. Keep those
specific and grounded in real examples where you have them.

## Quality checks

- [ ] Organised by tier; each group has intent + theming contract.
- [ ] Do/Don't are token-specific, not generic; grounded in misuse where evidence exists.
- [ ] Unclear intent flagged, not invented.
- [ ] Output-discipline self-check passed.
