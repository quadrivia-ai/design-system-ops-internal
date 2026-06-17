---
name: accessibility-per-component
description: "Audit ONE component for accessibility across keyboard, focus management, screen-reader/ARIA, colour contrast, and motion, mapped to WCAG 2.1 AA, and report audit thoroughness AND accessibility as two separate labels. Triggers on 'is this component accessible', 'a11y check on X', 'audit accessibility of the dialog', 'WCAG check for this component'. Do NOT trigger for theme-wide contrast across all tokens — use theme-audit. Do NOT trigger for a multi-check release gate — use the component-to-release agent. Do NOT trigger for the whole library's coverage — use component-audit."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/component-bestiary-reference.md
---

# Accessibility Per Component

## Context

Audits a single component's accessibility against WCAG 2.1 AA, depth calibrated to its
Challenge Rating, and reports **two separate labels** — how thorough the audit could be and
how accessible the component is. Static analysis has limits; this skill is explicit about
them rather than implying certainty.

## Configuration

Reads (all optional): `stack.component_roots`; `severity_overrides`; `accepted_findings`.

## Auto-pull integrations

None. Reads component source; recommends in-browser / assistive-tech verification for what
static analysis can't confirm.

## Step 0 — Rate and set thoroughness ceiling

Assign the component's Challenge Rating (see
[component-bestiary-reference](../../knowledge-notes/component-bestiary-reference.md)). CR7+
cannot be fully signed off statically — set the **thoroughness** ceiling accordingly and name
the manual checks still required.

## Step 1 — Audit the five surfaces

Map each to WCAG 2.1 AA success criteria:

- **Keyboard** — all interactions reachable/operable; logical tab order; no traps (2.1.1, 2.1.2).
- **Focus** — visible focus; trap+restore for overlays; roving tabindex where needed (2.4.7, 2.4.3).
- **Screen-reader / ARIA** — correct roles, names, states; live regions for async (4.1.2, 4.1.3).
- **Contrast** — text and non-text contrast meet AA (1.4.3, 1.4.11).
- **Motion** — respects reduced-motion; no motion-only cues (2.3.3, 1.4.13).

## Step 2 — Set the two labels

- **Audit thoroughness** (🟢 full → 🔴 surface-only) — how much could actually be verified
  given static limits + CR.
- **Accessibility** (🟢 strong → 🔴 blocking issues) — the component's state where checked.

## Produce the report

```
## Accessibility — <component> (CR<n>)

**Audit thoroughness: <🟢/🟡/🟠/🔴>** · **Accessibility: <🟢/🟡/🟠/🔴>**
**Headline.** <The blocking issue + first fix, one sentence.>

| Surface | Result | WCAG | Note |
|---------|--------|------|------|
| Keyboard | ✅/⚠️/❌ | 2.1.1 | … |
| Focus | … | 2.4.7 | … |
| SR / ARIA | … | 4.1.2 | … |
| Contrast | … | 1.4.3 | … |
| Motion | … | 2.3.3 | … |

### Findings
**<🔴/🟠/🟡/⚪> <title>** (WCAG <SC>)
- Evidence: `src/components/ui/dialog.tsx:NN` — no focus restore on close
- Why it matters: <one line>
- Remediation: <the specific fix>
- Caveat: <one, or omit>

### Manual verification still required
<for CR5+: the keyboard/SR walkthroughs static analysis can't replace>

### Scope
- Inspected: <component source; static checks performed>
- Not inspected: live AT behaviour (listed above); theme-wide contrast (→ theme-audit)
- Assumed: contrast from token values; verify borderline pairs in-browser
- Mark accepted findings in `.ds-ops-config.yml › accepted_findings`.
```

## Closing note

A 🟢 accessibility at 🟠 thoroughness means "looks good on static checks — still walk it with a
keyboard and a screen reader". Don't let a partial audit read as a clean bill.

## Quality checks

- [ ] Thoroughness and accessibility reported as two separate labels.
- [ ] Every finding maps to a specific WCAG 2.1 AA success criterion.
- [ ] CR5+ lists the manual checks static analysis couldn't cover.
- [ ] Output-discipline self-check passed.
