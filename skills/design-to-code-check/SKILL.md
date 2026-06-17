---
name: design-to-code-check
description: "Check ONE component or screen against its design spec across visual, layout, responsive, state, and token dimensions; classify each discrepancy by cause; and report audit confidence AND match as two separate labels. Triggers on 'does this component match the design', 'design-to-code check on X', 'compare this screen to Figma', 'is the implementation faithful to the spec'. Do NOT trigger for system-wide divergence across many components — use drift-detection. Do NOT trigger for deep accessibility of the component — use accessibility-per-component. Do NOT trigger for a repo-wide hardcoded-value scan — use token-compliance."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/design-to-code-contract.md
  - ../../knowledge-notes/token-architecture.md
---

# Design-to-Code Check

## Context

Compares a single component or screen to its design spec across the five contract dimensions
(see [design-to-code-contract](../../knowledge-notes/design-to-code-contract.md)), classifies
each discrepancy by cause, and reports **two separate labels** — how confident the check is
and how well the code matches — never multiplied into one.

## Configuration

Reads (all optional): `integrations.figma` (the spec source); `stack.token_files`;
`stack.theme_classes`; `severity_overrides`; `accepted_findings`.

## Auto-pull integrations

If Figma is configured, pull the component/frame spec via the ClaudeTalkToFigma MCP or REST.
If only a screenshot or a verbal description is provided, use that and lower **audit
confidence** accordingly. With no spec at all → diagnostic mode: state that no spec was
available and stop, rather than inventing one.

## Step 0 — Locate both sides

Resolve the implementation file(s) and the spec. Note the spec's fidelity (live Figma vs
screenshot vs prose) — this sets the confidence ceiling.

## Step 1 — Compare per dimension

Walk the five dimensions — visual, layout, responsive, state coverage, token usage. For each,
note matches and discrepancies with concrete evidence (`file:line`, the property, both values).

## Step 2 — Classify each discrepancy

Tag each by cause per the contract: intentional / version-lag / accidental / misunderstanding
/ system-gap. System-gap discrepancies are the design system's to fix, not the component's.

## Step 3 — Set the two labels

- **Audit confidence** (🟢 high → 🔴 none) — driven by spec fidelity and how much could be
  compared. Low confidence is honest, not a failure.
- **Match** (🟢 faithful → 🔴 major divergence) — how well the code matched *where it could be
  compared*.

## Produce the report

```
## Design-to-Code Check — <component/screen>

**Audit confidence: <🟢/🟡/🟠/🔴>** · **Match: <🟢/🟡/🟠/🔴>**
**Headline.** <What to fix first, in one sentence.>

| Dimension | Match | Note |
|-----------|-------|------|
| Visual | 🟢/🟡/🟠/🔴 | … |
| Layout | … | … |
| Responsive | … | … |
| State coverage | … | … |
| Token usage | … | … |

### Discrepancies
**<🔴/🟠/🟡/⚪> <title> — [cause]**
- Evidence: `src/.../X.tsx:54` — radius `rounded-md`, spec `rounded-lg`
- Why it matters: <one line>
- Remediation: <the specific change, or "system-gap → backlog-generator">
- Caveat: <one, or omit>

### Scope
- Inspected: <impl files; spec source + fidelity>
- Not inspected: deep a11y (→ accessibility-per-component); other components (→ drift-detection)
- Assumed: <spec fidelity → confidence ceiling>
- Mark intentional deviations accepted in `.ds-ops-config.yml › accepted_findings`.
```

## Closing note

Confidence and match are independent — a 🟢 match at 🟠 confidence still means "verify the
spec before trusting this". Confirm intentional deviations and accept them.

## Quality checks

- [ ] Confidence and match reported as two separate labels — never one number.
- [ ] Every discrepancy carries a cause classification and `file:line` evidence.
- [ ] Diagnostic mode used (no invented spec) when no spec was available.
- [ ] Output-discipline self-check passed.
