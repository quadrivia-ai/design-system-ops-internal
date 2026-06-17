---
name: ai-component-description
description: "Produce a six-section, LLM- and Figma-MCP-optimised specification for ONE component — identity, API, states/variants, composition, tokens/theming, usage/a11y — so agents and tools can consume it reliably. Triggers on 'document this component for AI', 'write a component spec for X', 'describe the Dialog for the MCP', 'generate component metadata doc'. Do NOT trigger for a composed multi-component pattern — use pattern-documentation. Do NOT trigger for when-to-use guidance only — use usage-guidelines. Do NOT trigger for a machine-readable index of ALL components — use codebase-index."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/component-bestiary-reference.md
  - ../../knowledge-notes/ai-readiness.md
---

# AI Component Description

## Context

Generates a structured spec for a single component, optimised for machine consumption (agents,
Figma MCP). Depth scales with Challenge Rating. It documents what's actually in the code —
inferred fields are flagged, and if the metadata is too thin to document well it says so
rather than inventing.

## Configuration

Reads (all optional): `stack.component_roots`; `output.report_dir`.

## Auto-pull integrations

None required. If Figma is configured, MAY cross-reference the component's design spec for
states/variants.

## Step 0 — Locate, rate, gate

Find the component; assign its Challenge Rating (depth driver). **Diagnostic mode:** if props,
types, or purpose can't be determined from the source (no types, opaque implementation),
stop and report exactly what's missing and what to add — never fabricate an API.

## Step 1 — Fill the six sections

1. **Identity** — name, CR, one-line purpose (mark inferred), tier.
2. **API** — props with types, defaults, required; events with payloads.
3. **States & variants** — the full matrix (variant × size × state).
4. **Composition** — sub-components/slots, `asChild`, expected nesting.
5. **Tokens & theming** — which tokens it consumes; per-theme behaviour.
6. **Usage & accessibility** — when to reach for it, keyboard model, ARIA contract.

CR1–2 may legitimately have a thin §3/§4 — note it; don't pad.

## Produce the spec

```
## <Component> — spec (CR<n>)

**Headline.** <One line: what it is + the one thing an integrator must know.>

### Identity / API / States & variants / Composition / Tokens & theming / Usage & a11y
<each section; inferred fields marked `(inferred)`>

### Scope
- Source: <files read>
- Inferred: <which fields were inferred vs read from types>
- Diagnostic: <if thin — what's missing to document this well>
```

## Closing note

This spec is only as trustworthy as the code it's read from — keep the `(inferred)` flags so
consumers know what to double-check. Regenerate when the API changes.

## Quality checks

- [ ] All six sections present (or diagnostic mode fired with the gap list).
- [ ] Inferred fields flagged; nothing fabricated.
- [ ] Depth matches CR — no padding on CR1–2, full detail on CR7+.
- [ ] Output-discipline self-check passed.
