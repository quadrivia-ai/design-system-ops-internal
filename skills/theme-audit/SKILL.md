---
name: theme-audit
description: "Audit theme parity and dark-mode/brand correctness across the semantic and component token tiers — every token resolves in every theme, text/background pairs meet WCAG AA contrast per theme, and no hardcoded values break theming. Triggers on 'audit our themes', 'check dark mode', 'theme parity', 'does colorblind mode work', 'find values that don't invert'. Do NOT trigger for token DEFINITION structure/naming alone — use token-audit. Do NOT trigger for one component's a11y across keyboard/SR/ARIA — use accessibility-per-component."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/token-architecture.md
---

# Theme Audit

## Context

Verifies the system themes hold together: every semantic and component token resolves in
each theme, contrast survives the swap, and nothing is hardcoded in a way that breaks dark or
colorblind mode. Where `token-audit` checks token *definition completeness*, theme-audit
checks *resolution, contrast, and visual parity*.

## Configuration

Reads (all optional): `stack.token_files`; `stack.theme_classes` (defaults
`light`/`dark`/`colorblind`); `stack.component_roots`; `severity_overrides`;
`accepted_findings`.

## Auto-pull integrations

None.

## Step 0 — Enumerate themes and tokens

Identify the theme root classes and the semantic + component tokens defined under each.

## Step 1 — Semantic-tier parity

Every semantic token (`--fg`, `--surface`, `--accent`, `--destructive`, …) must resolve in
every theme. A token defined for `light` but absent in `dark`/`colorblind` inherits the light
value silently — flag each, with the theme it's missing from.

## Step 2 — Component-tier parity

The domain tokens (`--chart-*`, `--node-*`, `--waveform-*`, `--score-*`, `--heatmap-*`,
`--outcome-*`) must also resolve per theme, and the colorblind theme must map them onto the
intended distinguishable palette. Flag domain tokens that fall back to a light value or
collide visually in colorblind mode.

## Step 3 — Contrast (the value-add over token-audit)

For text-on-background semantic pairs (`--fg` on `--surface`, `--accent-foreground` on
`--accent`, …), check WCAG 2.1 AA contrast **in each theme**. Report pairs that pass in light
but fail in dark/colorblind — the common silent regression.

## Step 4 — Theming-breaking values

Scan code for values that don't invert: standalone `bg-white` / `text-black` /
`border-white`, hardcoded hex, and opacity-based tints that vanish on dark surfaces (e.g. a
near-transparent dark token over a dark background). Each is a concrete `file:line`.

## Produce the report

```
## Theme Audit — <themes>

**<🟢/🟡/🟠/🔴> Headline.** <Which theme is at risk + the first fix.>

| Dimension | Health | Note |
|-----------|--------|------|
| Semantic parity | … | e.g. 2 tokens missing in colorblind |
| Component-tier parity | … | … |
| Contrast (per theme) | … | … |
| Theming-breaking values | … | … |

### Findings
**<🔴/🟠/🟡/⚪> <title>**
- Evidence: `--accent-foreground` on `--accent` — 2.9:1 in dark (AA needs 4.5:1)
- Why it matters: <one line>
- Remediation: <the token value to adjust, or the class to replace>
- Caveat: <one, or omit>

### Scope
- Inspected: <token files, theme blocks, code scanned for non-theming values>
- Not inspected: per-component keyboard/SR a11y (→ accessibility-per-component)
- Assumed: contrast computed from token values; verify in-browser for the borderline pairs
- Mark accepted findings in `.ds-ops-config.yml › accepted_findings`.
```

## Closing note

A failing contrast pair may be on a surface that never carries text — confirm and accept
those. Borderline contrast should be verified in-browser before acting.

## Quality checks

- [ ] Parity checked for semantic AND component tiers, per theme.
- [ ] Contrast reported per theme, with the ratio and the AA threshold.
- [ ] Theming-breaking values cite `file:line`.
- [ ] Output-discipline self-check passed.
