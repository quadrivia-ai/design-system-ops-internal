# MCP / Integration Setup Guide (reference)

How to wire the optional integrations, and how skills degrade gracefully without them. Loaded
by `figma-variable-audit`, `cicd-integration`, `adoption-report`, and any skill with an
"Auto-pull integrations" section. Pairs with [output-discipline](output-discipline.md).

## Secrets doctrine

**No secret value lives in any file in this pack.** `.ds-ops-config.yml` stores only the
*names* of environment variables; the secret itself lives in your environment or a secret
manager. Any integration token should be **least-privilege and read-only** where the provider
supports it.

## Figma

Two ways to reach Figma variables/specs:

- **ClaudeTalkToFigma MCP** — if connected, skills use it directly; no token in config.
- **REST** — set `integrations.figma.file_key` (an identifier, not a secret) and
  `integrations.figma.access_token_env` (the env-var *name*, e.g. `FIGMA_ACCESS_TOKEN`).

Used by `figma-variable-audit` and optionally `design-to-code-check`.

## GitHub

Set `integrations.github.repo` and `integrations.github.token_env` (e.g. `GITHUB_TOKEN`,
read-only scope). Enriches history-based signals (change frequency, version-lag) for
`component-audit`, `drift-detection`, `adoption-report`.

## Chromatic / docs platform

`integrations.chromatic.project_token_env` and `integrations.docs_platform.api_key_env` —
names only. Used where visual-regression or docs-publishing signals are relevant.

## Graceful degradation (required)

Every integration is optional. A skill that can't reach an integration runs in **diagnostic
mode**: it states exactly what it could not reach, marks the affected dimensions N/A (never
🔴), and lists what it *would* check once configured. A skill never fabricates data it
couldn't fetch, and never blocks on a missing integration.
