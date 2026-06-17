---
name: component-api-validator
description: "Validate component prop APIs for cross-library consistency — boolean naming, event-handler naming, variant/size enums, controlled/uncontrolled patterns, ref forwarding, and className/asChild passthrough — reporting PASS/WARN/FAIL per convention with the offending components. Triggers on 'validate component APIs', 'are our prop APIs consistent', 'check prop naming across components', 'component API audit'. Do NOT trigger for naming beyond props (files, tokens, patterns) — use naming-audit. Do NOT trigger for component health (coverage, complexity, dupes) — use component-audit. Do NOT trigger for design-spec fidelity — use design-to-code-check."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/component-bestiary-reference.md
---

# Component API Validator

## Context

Checks that component prop APIs follow one consistent contract across the library, so
consumers can predict an API without reading each implementation. Reports ✅/⚠️/❌ per
convention with the components that break it. Predictability is the bar; the shadcn/ui
primitives are the de-facto reference for the conventions below.

## Configuration

Reads (all optional): `stack.component_roots`; `stack.ui_library`; `severity_overrides`;
`accepted_findings`.

## Auto-pull integrations

None.

## Step 0 — Collect prop APIs

Extract each component's props (names, types, defaults, controlled/uncontrolled signals,
ref/forwardRef, `className`/`asChild`) across the roots.

## Step 1 — Check the conventions

- **Booleans** — `is/has/should/can` or a clear state name; not bare nouns.
- **Event handlers** — `on<Event>` with matching `<event>` payloads.
- **Variant/size** — string-union enums named `variant` / `size`, shared vocabulary.
- **Controlled/uncontrolled** — `value`+`onValueChange` (+ `defaultValue`) pattern applied
  consistently for stateful primitives.
- **Composition** — `className` passthrough merged via `cn()`; `asChild` where Radix-style
  slotting is expected.
- **Ref forwarding** — interactive primitives forward refs.

## Step 2 — Classify per convention

✅ followed library-wide · ⚠️ followed with isolated exceptions · ❌ inconsistent enough that
consumers can't predict it.

## Produce the report

```
## Component API Validation — <roots>

**<🟢/🟡/🟠/🔴> Headline.** <Least predictable convention + first fix.>

| Convention | Result | Offenders |
|------------|--------|-----------|
| Boolean naming | ⚠️ | `Card(active)` → `isActive` |
| Event handlers | ✅ | — |
| variant/size enums | ❌ | 3 components use `type` for variant |
| Controlled pattern | ⚠️ | … |
| className passthrough | ✅ | — |
| Ref forwarding | ⚠️ | `IconButton` doesn't forward |

### Findings
**<🔴/🟠/🟡/⚪> <convention> — <component(s)>**
- Evidence: `src/components/ui/foo.tsx:NN` — prop `type` used for visual variant
- Why it matters: <one line>
- Remediation: <the specific rename / signature change>
- Caveat: <one, or omit>

### Scope
- Inspected: <roots; components parsed>
- Not inspected: non-prop naming (→ naming-audit); component health (→ component-audit)
- Assumed: shadcn/ui conventions as the reference vocabulary
- Mark accepted exceptions in `.ds-ops-config.yml › accepted_findings`.
```

## Closing note

Some API divergences are deliberate (a deliberately different primitive). Confirm and accept
those; the rest are consumer-confusion tax worth paying down.

## Quality checks

- [ ] Per-convention result uses ✅/⚠️/❌; offenders named with `file:line`.
- [ ] Remediations are concrete signature changes, not "make it consistent".
- [ ] Output-discipline self-check passed.
