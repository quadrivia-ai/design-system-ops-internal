---
name: designer-onboarding
description: "Produce a two-week onboarding guide for a DESIGNER joining a team that consumes the design system — what to learn, in what order, calibrated to the system's complexity. Triggers on 'onboard a designer to our design system', 'designer onboarding guide', 'ramp plan for a new designer'. Do NOT trigger for an engineer's onboarding — use engineering-onboarding. Do NOT trigger for a leadership brief or funding pitch — use stakeholder-brief / system-pitch."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/component-governance.md
---

# Designer Onboarding

## Context

A two-week ramp plan for a designer joining a consuming team, calibrated to how complex the
system actually is — a small system needs days, not a fortnight of ceremony. Oriented to the
designer's world: tokens as design decisions, components as the shared vocabulary, and how to
propose changes.

## Configuration

Reads (all optional): `maturity_level` (else inferred); `integrations.figma` (to point at the
real variable collections); `stack.theme_classes`.

## Auto-pull integrations

None required. If Figma is configured, point the designer at the actual variable collections
and library file.

## Step 0 — Size the ramp

Scale the plan to system complexity (see [component-governance](../../knowledge-notes/component-governance.md)).
Compress for a small system; don't stretch two days into two weeks.

## Step 1 — Build the week-by-week

- **Week 1 — Learn the language:** the token tiers as design decisions (colour/space/type),
  the three themes incl. colorblind, the component library and what each is for, where the
  Figma variables/library live.
- **Week 2 — Work within it:** design a real small task using only system tokens/components;
  learn when something is missing and how to propose it (`contribution-workflow`); learn the
  deprecation/΄change rhythm.

## Produce the guide

```
## Designer Onboarding — <system> (~complexity: Level N)

**Day 1.** <the single most useful thing to read/open first>

### Week 1 — Learn the language
- [ ] Token tiers + the three themes (where defined, how they re-point)
- [ ] Component library tour — which component for which need (`component-decision-tree`)
- [ ] Figma variables / library file

### Week 2 — Work within it
- [ ] Design a real task using only system tokens/components
- [ ] Spot a gap → propose via the contribution process
- [ ] Understand how changes are announced

### Scope
- Sized for: Level N (<inferred/configured>)
- Assumed: <Figma availability>
```

## Closing note

The fastest ramp is one real task done inside the system in week 2 — front-load just enough
language in week 1 to make that possible, and cut the rest.

## Quality checks

- [ ] Sized to complexity; not padded to two weeks for a small system.
- [ ] Week 2 includes doing a real task within the system.
- [ ] Points at the right skills by name (decision-tree, contribution-workflow).
- [ ] Output-discipline self-check passed.
