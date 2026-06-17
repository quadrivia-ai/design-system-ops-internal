---
name: token-audit
description: "Audit how design tokens are DEFINED — tier structure, naming consistency, theme-definition parity, DTCG readiness, and structural debt (semantics defined as raw literals, banned legacy aliases still declared, duplicates). Triggers on 'audit my tokens', 'review my design tokens', 'are my tokens well structured', 'token health', 'check my CSS variables'. Do NOT trigger for how CODE consumes tokens (hardcoded colours in components, wrong-tier refs in JSX) — use token-compliance. Do NOT trigger for a machine-readable token index — use codebase-index. Do NOT trigger for a holistic multi-dimension sweep — use system-health. Do NOT trigger for runtime theme/contrast parity across tiers — use theme-audit."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/token-architecture.md
---

# Token Audit

## Context

Inspects the files where tokens are *defined* (not where they're used) and reports how well
the token layer is structured: tiering, naming, theme-definition completeness, DTCG
readiness, and structural debt. Output is a short, label-based report a token owner can act
on line by line. For how product code *consumes* tokens, that is `token-compliance`.

## Configuration

Reads (all optional; works with zero config):

- `stack.token_files` — defaults to `src/index.css` and `src/design-system/care-journeys/tokens.css`.
- `stack.theme_classes` — defaults to `light`, `dark`, `colorblind`.
- `stack.lint.token_rules` — the ESLint rules this audit defers to (`no-hardcoded-tailwind-colors`, `no-raw-tailwind-typography`).
- `severity_overrides`, `accepted_findings`, `maturity_level`.

## Auto-pull integrations

None required. If `integrations.figma.file_key` is set, you MAY note where token names
diverge from Figma variable names as a pointer — but leave deep variable analysis to
`figma-variable-audit`.

## Step 0 — Locate and parse

Read each file in `stack.token_files`. Identify theme blocks (`:root`, `:root.light`,
`.dark`, `.colorblind`). Build an inventory: token name → value(s) per theme block. Record
counts (e.g. "61 tokens across 3 theme blocks").

## Step 1 — Classify each token into a tier

Using [token-architecture](../../knowledge-notes/token-architecture.md):

- **Primitive** — a raw scale value (e.g. a numbered hue) not referencing another var.
- **Semantic** — intent names (`--fg`, `--surface`, `--accent`, `--destructive`, `--border-subtle`…).
- **Component/domain** — prefixed families (`--chart-*`, `--node-*`, `--waveform-*`, `--score-*`, `--heatmap-*`, `--outcome-*`).

Record the shape. This repo implements the semantic and component tiers but **no
primitive/reference tier** — semantic tokens hold raw HSL triplets rather than aliasing a
named scale. Surface that against the three-tier model in Steps 3 and 5 at honest severity.

## Step 2 — Theme-definition parity

For every token defined in the base/light block, confirm it is also defined in each other
theme class. A token present in `:root.light` but missing from `.dark` or `.colorblind`
silently inherits the light value → broken theming. Report each gap with the token name and
the theme block missing it. (Deeper *visual* parity and contrast across tiers is
`theme-audit`'s job — note the handoff, don't do it here.)

## Step 3 — Structural debt

- **Semantic defined as a hard literal** where an alias would be expected — the alias layer
  isn't aliasing; flag as an opportunity.
- **Banned legacy aliases still declared** — e.g. `--accent-light` (→ `bg-accent/15`),
  anything mapping to `bg-muted` / `bg-background`, stuttering `--bg-*` / `--text-*`
  families. Cross-reference the project's banned-token list.
- **Duplicates / near-duplicates** — two tokens with identical values and overlapping intent.

## Step 4 — Naming consistency

Check semantic names follow one scheme (the `fg` / `fg-secondary` / `fg-muted` hierarchy;
`border-subtle` / `border-strong`). Flag cross-tier collisions (a name that reads semantic
but holds a raw value) and orphans (defined, referenced nowhere in the token files). Confirm
HSL triplet format is consistent (`H S% L%`, space-separated, no `hsl()` wrapper at the
definition site).

## Step 5 — DTCG readiness and severity calibration

Assess DTCG opportunity per [token-architecture](../../knowledge-notes/token-architecture.md)
(an interchange opportunity scaled by leverage). Then calibrate **severity, not existence**:
the absent primitive tier is a genuine gap against the three-tier model — report it and
recommend the reference layer. How *urgent* it is scales with the cost it already creates
(here: raw HSL values duplicated across three theme blocks, so a brand re-skin touches many
declarations). Don't downgrade the finding because it's the current design; let the team mark
it accepted if the duplication is tolerable.

## Produce the report

```
## Token Audit — <token files>

**<🟢/🟡/🟠/🔴> Headline.** <Posture + first action, one sentence.>

| Dimension | Health | Note |
|-----------|--------|------|
| Tier structure | 🟢/🟡/🟠/🔴 | … |
| Theme-definition parity | … | … |
| Naming consistency | … | … |
| Structural debt | … | … |
| DTCG readiness | … | … |

### Findings
**<🔴/🟠/🟡/⚪> <title>**
- Evidence: `src/index.css:128` — `--surface` defined in `:root.light`, absent in `.colorblind`
- Why it matters: <one line>
- Remediation: <exact token / value / rename>
- Caveat: <one, or omit this line>

### Scope
- Inspected: <token files actually read; theme blocks found>
- Not inspected: code consumption (→ token-compliance), runtime contrast (→ theme-audit)
- Assumed: <e.g. token_files defaults, since no config present>
- Mark any finding accepted in `.ds-ops-config.yml › accepted_findings` to skip it next run.
```

## Closing note

Several of these may be deliberate — a single semantic tier, an intentional duplicate for
one specific surface. Flag those and add their fingerprints to `accepted_findings`; future
runs will skip them.

## Quality checks

- [ ] Inspected only definition files; consumption findings deferred to `token-compliance`.
- [ ] Every gap names the token + the specific theme block / `file:line`.
- [ ] Structural gaps measured against best practice and reported; only their urgency is scaled to system size — none waved through as "the current design".
- [ ] Output-discipline self-check passed (labels only, headline-first, scope note, peer tone).
