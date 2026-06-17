# Token Architecture (reference)

How design tokens are *structured* and how to spot structural debt. Loaded by
`token-audit`, `token-compliance`, `token-documentation`, `theme-audit`,
`figma-variable-audit`, `schema-validator`, and the migration agent. Pairs with
[output-discipline](output-discipline.md) — report findings with labels, evidence, and a
Scope note.

## Our model (confirmed against the repo)

Tokens are **CSS custom properties in HSL-triplet format**, themed by a root class and
consumed through Tailwind:

- **Definition:** `src/index.css` (global) and `src/design-system/care-journeys/tokens.css`
  (capability-scoped).
- **Format:** bare HSL triplets — `--accent: 245 100% 70%;` — so Tailwind can apply the
  alpha modifier: `hsl(var(--accent) / <alpha-value>)`. Inline styles wrap explicitly:
  `style={{ color: "hsl(var(--fg))" }}`.
- **Theming:** `:root.light` / `:root.dark` / `:root.colorblind` swap the triplet *values*;
  consumers never change. The colorblind theme maps every domain token onto the Wong 2011
  palette.
- **Partial enforcement exists:** `eslint-rules/no-hardcoded-tailwind-colors.js` and
  `no-raw-tailwind-typography.js`. Best practice is that raw values *cannot* reach
  production. Treat these rules as partial coverage to assess *against* that bar — don't
  re-list every lint-caught violation as noise, but if a skill finds raw values the rules
  should have caught, the gap itself is a finding (a rule hole, a disabled rule, or an
  `eslint-disable` exemption).

## The three tiers

1. **Primitive (raw scale)** — a literal value with no meaning: a hue, spacing step, or
   radius. Answers "what", never "what for". *Shape:* `--blue-500: 217 91% 60%`.
2. **Semantic (alias)** — intent mapped onto a primitive; **this is the tier product code
   consumes.** *Ours:* `--page`, `--surface`, `--elevated`, `--field`, `--fg`,
   `--fg-secondary`, `--fg-muted`, `--accent`, `--accent-foreground`, `--success`,
   `--warning`, `--destructive`, `--border-subtle`, `--border-strong`, `--link`. Themes
   re-point these.
3. **Component (scoped)** — a token for one component/domain family when semantics aren't
   enough. *Ours:* `--chart-1..8`, `--node-start/-llm/-tool`, `--waveform-agent/-patient`,
   `--score-excellent/-good/-poor`, `--heatmap-*`, `--outcome-*`. These reference semantics
   or primitives, never raw hex.

**The rule that makes tiers worth having:** product code consumes **semantic** (and where
justified, **component**) tokens — never primitives, never raw values. Primitives feed
semantics; semantics feed components and code.

**Where this repo sits against the model:** it implements the semantic and component tiers
but **no primitive/reference tier** — semantic tokens hold raw HSL triplets directly
(`--accent: 245 100% 70%`) rather than aliasing a named scale. The audits surface that as a
real gap — raw values are copy-pasted across the three theme blocks, so a brand re-skin means
editing many semantic declarations — at a severity that scales with how many themes/brands
re-point values today.

## Tier leakage (what the audits hunt)

Leakage is a reference that skips or crosses tiers. In our codebase it appears as:

- **Raw value in code** — `bg-[#4f46e5]`, `text-red-500`, `style={{ color: '#fff' }}`.
  Caught by `no-hardcoded-tailwind-colors`; a live finding means the rule has a gap or the
  file is exempted. → `token-compliance`
- **Raw Tailwind typography** — `text-sm`, `text-[13px]` instead of `text-body-2` etc.
  Caught by `no-raw-tailwind-typography`. → `token-compliance`
- **Primitive consumed directly in code** — a component using `--blue-500` instead of
  `--accent`. Defeats theming: themes re-point semantics, not primitives. → `token-audit`
- **Banned legacy alias** — `bg-accent-light` (use `bg-accent/15`), `bg-muted` (use
  `bg-elevated`), `bg-background` (use `bg-page`), the stuttering `bg-bg-*` / `text-text-*`
  families. Migration debt, not raw leakage. → `token-audit`, migration agent
- **Semantic defined as a literal** — a "semantic" token whose value is a hard hex rather
  than referencing a primitive scale. Structural debt: the alias layer isn't aliasing
  anything. → `token-audit`

## DTCG alignment (interchange opportunity)

The [DTCG format module, 2025.10 draft](https://tr.designtokens.org/) standardises tokens
as typed JSON (`$value`, `$type`, `$description`, group `$extensions`). CSS custom properties
are a valid *runtime* form of tokens; DTCG is the emerging *interchange* format that lets
design tools and multi-platform pipelines read the same source. Lacking a DTCG source isn't a
correctness defect, but it is a real gap once design↔code interop is a goal:

- Report DTCG gaps as 🟡/🟠 **opportunities scaled by leverage** ("tokens carry no
  `$description`/`$type`; a DTCG export would unlock Figma variable sync and multi-platform
  tooling") — reserve 🔴 for when interop is an active, blocked requirement.
- Honest mapping: an HSL triplet ≈ a DTCG `color` token; the tier names already approximate
  DTCG groups; what's missing is machine-readable type + description + a single source file.
  `token-documentation` and `metadata-schema-generator` are the bridge.

## How skills use this note

- `token-audit` — assess tier structure, naming, leakage, and DTCG opportunity; calibrate
  depth to maturity.
- `token-compliance` — scan *code* for the leakage patterns above; every finding = file +
  line + current value + recommended token.
- `theme-audit` — verify every semantic token resolves in `light`/`dark`/`colorblind`; flag
  parity gaps.
- `token-documentation` — document token *intent* by tier, with theming contracts.
- `figma-variable-audit` / migration agent — map Figma variable collections to these tiers;
  drive transformation tables.

## Scope reminder

These tiers describe how tokens *should* layer. A skill reports only what it found in the
files it scanned, and says so (see [output-discipline](output-discipline.md) §2).
