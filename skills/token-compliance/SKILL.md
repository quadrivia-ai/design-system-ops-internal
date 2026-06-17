---
name: token-compliance
description: "Scan CODE for values that should be tokens — hardcoded hex/rgb, raw Tailwind colour and typography classes, banned legacy aliases, wrong-tier references, and inline styles that bypass tokens — emitting file + line + current value + recommended token for each, plus an assessment of where enforcement has gaps. Triggers on 'find hardcoded colours', 'token compliance scan', 'where are we bypassing tokens', 'check code for raw values'. Do NOT trigger for how tokens are DEFINED (tier structure, naming, theme parity) — use token-audit. Do NOT trigger for theme contrast — use theme-audit."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/token-architecture.md
---

# Token Compliance

## Context

Scans product code for values that should flow through tokens, and reports each as a concrete,
fixable line with the recommended token. It also assesses **whether enforcement has gaps** —
best practice is that raw values cannot reach production, so a violation existing despite the
project's ESLint rules is itself a finding.

## Configuration

Reads (all optional): `source_globs` / `ignore_globs`; `stack.token_files` (to resolve the
recommended token); `stack.lint.token_rules`; `severity_overrides`; `accepted_findings`.

## Auto-pull integrations

None.

## Step 0 — Scan code, not definitions

Scan `source_globs` (`.tsx`/`.ts`/`.css`), excluding the token-definition files themselves
and `ignore_globs`. Definition structure is `token-audit`'s job.

## Step 1 — Detect the violation classes

- **Hardcoded colour** — hex/rgb/hsl literals in JSX/CSS/inline styles, raw Tailwind colour
  classes (`text-red-500`, `bg-gray-100`).
- **Raw typography** — `text-sm`, `text-[13px]` instead of `text-body-2` etc.
- **Banned legacy alias** — `bg-accent-light`, `bg-muted`, `bg-background`, stuttering
  `bg-bg-*` / `text-text-*`.
- **Wrong-tier reference** — a primitive used where a semantic exists.
- **Token-bypassing inline style** — a `style={{ … }}` literal where a token/class exists.

## Step 2 — Map each to a recommended token

For every hit, resolve the correct replacement from the token files (e.g. `#ef4444` →
`text-destructive`; `text-sm` → `text-body-2`; `bg-accent-light` → `bg-accent/15`). A finding
without a concrete recommendation isn't done.

## Step 3 — Assess enforcement gaps

Cross-reference `stack.lint.token_rules`. If violations exist that those rules should catch,
report the **enforcement gap** (a rule hole, a disabled rule, or an `eslint-disable`
exemption) — don't just re-list the violations. Closing the gap prevents recurrence.

## Produce the report

```
## Token Compliance — <globs>

**<🟢/🟡/🟠/🔴> Headline.** <Volume + whether enforcement is the real fix.>

Counts: <hardcoded colour: 12 · raw typography: 5 · banned alias: 3 · wrong-tier: 1>

### Findings
**<🔴/🟠/🟡/⚪> <class> — `src/.../X.tsx:42`**
- Current: `className="text-red-500"`
- Recommended: `text-destructive`
- Caveat: <one, or omit>

(group by file when dense; keep the current→recommended pair on every line)

### Enforcement gap
<which rule should have caught these; where it's disabled/exempted; the fix>

### Scope
- Inspected: <globs scanned; files counted>
- Not inspected: token definitions (→ token-audit); contrast (→ theme-audit)
- Assumed: recommendations resolved from <token files>
- Mark accepted exceptions in `.ds-ops-config.yml › accepted_findings`.
```

## Closing note

Some raw values are deliberate (a one-off marketing surface, a third-party embed). Confirm and
accept those. Where the volume is high, the highest-leverage fix is usually closing the
enforcement gap, not hand-fixing each line.

## Quality checks

- [ ] Every finding has `file:line` + current value + a concrete recommended token.
- [ ] Enforcement gap assessed against the project's lint rules, not just violations listed.
- [ ] Scanned code only — token-definition files excluded.
- [ ] Output-discipline self-check passed.
