---
name: figma-variable-audit
description: "Audit Figma variable collections against token best practice and against the code tokens — tier structure of collections, mode coverage for theming, and name/value parity between Figma and `src/index.css`. Triggers on 'audit my figma variables', 'do our figma variables match our tokens', 'check figma variable collections', 'figma token parity'. Requires a Figma connection (ClaudeTalkToFigma MCP or REST + FIGMA_ACCESS_TOKEN); runs in diagnostic mode if absent. Do NOT trigger for code-side token DEFINITION structure — use token-audit. Do NOT trigger for code consumption of tokens — use token-compliance."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/token-architecture.md
  - ../../knowledge-notes/mcp-setup-guide.md
---

# Figma Variable Audit

## Context

Compares the Figma variable collections to the three-tier token model and to the code tokens,
reporting where the design source and the code source disagree. The bar is a clean
primitive→semantic→component collection structure with theme **modes**, mirrored 1:1 by code.

## Configuration

Reads (all optional): `integrations.figma.file_key`; `integrations.figma.access_token_env`
(name only); `stack.token_files`; `stack.theme_classes`; `severity_overrides`;
`accepted_findings`.

## Auto-pull integrations

**This skill needs Figma.** It pulls variable collections + modes via the ClaudeTalkToFigma
MCP if connected, or the Figma REST API using the token named in
`integrations.figma.access_token_env`. See [mcp-setup-guide](../../knowledge-notes/mcp-setup-guide.md).

**If no Figma connection is available → diagnostic mode:** do not guess. Report 🟢/🟠 = N/A,
state exactly what could not be reached, and list what the skill *would* check once
`file_key` + the token env var are set. Nothing is fetched without configuration.

## Step 0 — Connect and pull

Resolve the connection. Pull collections, variables, and modes. Record what was retrieved
(e.g. "3 collections, 2 modes, 71 variables").

## Step 1 — Collection structure vs the three tiers

Map collections to primitive / semantic / component per
[token-architecture](../../knowledge-notes/token-architecture.md). Flag a flat collection
with no primitive layer, or semantic variables holding raw literals instead of aliasing
primitives — the same gap the code side has, surfaced honestly on both sides.

## Step 2 — Mode coverage (theming)

Best practice: each theme is a **mode**, and every variable resolves in every mode. Compare
modes to `stack.theme_classes` (`light`/`dark`/`colorblind`). Flag variables missing a mode
value and themes present in code but absent as a Figma mode.

## Step 3 — Code ↔ Figma parity

Diff Figma variable names/values against the code tokens. Report: variables with no code
token, code tokens with no variable, and name mismatches for the same intent (`accent` vs
`brand/primary`). Each is a divergence that will desync design and build.

## Produce the report

```
## Figma Variable Audit — <file_key or "diagnostic mode">

**<🟢/🟡/🟠/🔴> Headline.** <Parity posture + first reconciliation.>

| Dimension | Health | Note |
|-----------|--------|------|
| Collection structure | … | … |
| Mode coverage | … | … |
| Code ↔ Figma name parity | … | … |
| Code ↔ Figma value parity | … | … |

### Findings
**<🔴/🟠/🟡/⚪> <title>**
- Evidence: Figma `color/accent` (#4F46E5) vs code `--accent: 245 100% 70%` — value mismatch
- Why it matters: <one line>
- Remediation: <align which source to which; who owns the canonical value>
- Caveat: <one, or omit>

### Scope
- Inspected: <collections/modes pulled, or "no connection — diagnostic mode">
- Not inspected: code consumption (→ token-compliance)
- Assumed: <file_key/env from config; HSL↔hex compared after conversion>
- Mark accepted divergences in `.ds-ops-config.yml › accepted_findings`.
```

## Closing note

Name divergences are often historical and deliberate; confirm the canonical source per token
and accept the rest. Decide and record **which side owns the value** for each token.

## Quality checks

- [ ] Ran diagnostic mode (not guesses) when Figma was unreachable.
- [ ] HSL↔hex compared after conversion, not as raw strings.
- [ ] Each parity finding names which source should change.
- [ ] Output-discipline self-check passed.
