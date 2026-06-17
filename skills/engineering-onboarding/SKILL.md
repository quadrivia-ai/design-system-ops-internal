---
name: engineering-onboarding
description: "Produce a two-week onboarding guide for an ENGINEER joining a team that consumes the design system — tokens, components, conventions, enforcement, and the contribution flow, in ramp order, calibrated to complexity. Triggers on 'onboard an engineer to our design system', 'engineering onboarding guide', 'ramp plan for a new dev on the design system'. Do NOT trigger for a designer's onboarding — use designer-onboarding. Do NOT trigger for a leadership brief or funding pitch — use stakeholder-brief / system-pitch."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/component-governance.md
  - ../../knowledge-notes/ai-readiness.md
---

# Engineering Onboarding

## Context

A two-week ramp for an engineer joining a consuming team, calibrated to system complexity. It
gets them productive *and* conformant fast: consume semantic tokens (never raw values), reach
for shared components first, and understand what the tooling enforces.

## Configuration

Reads (all optional): `maturity_level` (else inferred); `stack.*` (token files, component
roots, lint rules, test convention).

## Auto-pull integrations

None.

## Step 0 — Size the ramp

Scale to complexity (see [component-governance](../../knowledge-notes/component-governance.md)).
A small system is a day-one read, not a fortnight.

## Step 1 — Build the week-by-week (stack-specific)

- **Week 1 — Learn the rules:** the token tiers + `hsl(var(--token))` consumption; the three
  themes; the component library + `component-decision-tree`; the conventions doc; what the
  ESLint rules (`no-hardcoded-tailwind-colors`, `no-raw-tailwind-typography`) block and why.
- **Week 2 — Ship within it:** build a real small feature using only semantic tokens + shared
  components; add a co-located test; run the checks locally; learn the contribution flow and
  how releases/deprecations are communicated.

Point at the machine-readable context (`codebase-index` / `context-engine-builder`) for the
AI-assisted workflow (see [ai-readiness](../../knowledge-notes/ai-readiness.md)).

## Produce the guide

```
## Engineering Onboarding — <system> (~complexity: Level N)

**Day 1.** Read the conventions doc; skim the token file; run the app + the checks.

### Week 1 — Learn the rules
- [ ] Token tiers + `hsl(var(--token) / <alpha>)`; the three themes
- [ ] Component library + which-for-which (`component-decision-tree`)
- [ ] What lint enforces (the two token rules) and why
- [ ] Co-located test convention

### Week 2 — Ship within it
- [ ] Build a real task: semantic tokens + shared components only
- [ ] Add a co-located test; run lint + tests locally
- [ ] Contribution flow + how changes are announced

### Scope
- Sized for: Level N (<inferred/configured>)
- Assumed: <stack defaults from config>
```

## Closing note

The lint rules are the fastest teacher — encourage the new engineer to let them fail once and
read why; that's the convention internalised. Front-load only enough to ship a real task in
week 2.

## Quality checks

- [ ] Sized to complexity; stack-specific (tokens, lint rules, tests named).
- [ ] Week 2 ships a real task within the system + runs the checks.
- [ ] Points at decision-tree / context skills by name.
- [ ] Output-discipline self-check passed.
