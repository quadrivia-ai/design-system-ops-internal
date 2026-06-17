> **Illustrative sample.** Shows the shape and quality of a `token-audit` run against a
> Tailwind + HSL-custom-property codebase. Values are representative, not a live measurement.

## Token Audit — src/index.css, src/design-system/care-journeys/tokens.css

**🟡 Functional, with one structural gap worth planning around.** The semantic layer is
coherent and theme-aware; the missing primitive/reference tier is the thing to address before
a brand variant lands. Start there.

| Dimension | Health | Note |
|-----------|--------|------|
| Tier structure | 🟠 | Semantic + component tiers present; no primitive/reference tier |
| Theme-definition parity | 🟡 | 2 tokens defined for light/dark but absent in colorblind |
| Naming consistency | 🟢 | `fg`/`fg-secondary`/`fg-muted` + `border-subtle`/`-strong` schemes hold |
| Structural debt | 🟡 | 1 banned legacy alias still declared |
| DTCG readiness | 🟡 | No machine-readable type/description; CSS-var only |

### Findings

**🟠 No primitive/reference tier**
- Evidence: `src/index.css` — semantic tokens hold raw HSL triplets directly (e.g.
  `--accent: 245 100% 70%`), repeated per theme block, with no named scale behind them.
- Why it matters: a brand re-skin or a 4th theme means editing many semantic declarations
  across every `:root.*` block; values can't be audited as one scale.
- Remediation: introduce a primitive layer (`--brand-500: …`) and alias semantics to it
  (`--accent: var(--brand-500)`); themes then re-point primitives.
- Caveat: at three themes the duplication is tolerable today — this is a "before brand #2"
  item, not an emergency. Accept via `token-audit:no-primitive-tier` if deliberate.

**🟡 Colorblind theme missing 2 tokens**
- Evidence: `--warning`, `--score-good` defined in `:root.light`/`.dark`, absent in
  `:root.colorblind` → they inherit the light value and break the Wong-palette mapping.
- Why it matters: silent theme regression — colorblind users get an off-palette colour.
- Remediation: add both to the `.colorblind` block with their Wong-palette values.

**🟡 Banned legacy alias still declared**
- Evidence: `--accent-light` is still defined (project guidance replaced it with `bg-accent/15`).
- Why it matters: keeps a deprecated path alive for new code to pick up.
- Remediation: remove the declaration; migrate any consumers via `token-compliance` + `codemod-generator`.

### Scope
- Inspected: the two token files; theme blocks `:root.light`, `.dark`, `.colorblind`.
- Not inspected: code consumption (→ `token-compliance`); runtime contrast (→ `theme-audit`).
- Assumed: `token_files` defaults (no config present).
- Mark any finding accepted in `.ds-ops-config.yml › accepted_findings` to skip it next run.
