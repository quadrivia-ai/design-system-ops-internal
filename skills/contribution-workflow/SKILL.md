---
name: contribution-workflow
description: "Produce a complete contribution process for adding a component/token/pattern to the system — six stages from proposal to release, with templates and acceptance criteria, sized to team and maturity. Triggers on 'how should people contribute to our design system', 'set up a contribution process', 'contribution guidelines', 'how do we add a component'. Do NOT trigger for removing/sunsetting something — use deprecation-process. Do NOT trigger for recording a single decision — use decision-record."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/component-governance.md
  - ../../knowledge-notes/component-bestiary-reference.md
---

# Contribution Workflow

## Context

Generates a contribution process — six stages, each with a template and acceptance criteria —
sized to the team's maturity. A Level-2 system gets a lightweight checklist; a Level-4 system
gets review gates and Challenge-Rating-driven depth. The output is a process document the team
can adopt, not a critique.

## Configuration

Reads (all optional): `maturity_level` (else inferred); `stack.component_roots`;
`stack.ui_library`.

## Auto-pull integrations

None.

## Step 0 — Size the process

Set rigor from maturity (see [component-governance](../../knowledge-notes/component-governance.md)).
Don't impose an RFC process on a small team; don't leave a large one with no gate.

## Step 1 — Build the six stages

1. **Propose** — problem, prior art (does a component already cover it? check the library),
   intended API.
2. **Design / spec** — tokens used, states, responsive, a11y intent. Figma spec if available.
3. **Build** — folder shape + barrel export, semantic tokens only, co-located test.
4. **Review** — design + code + a11y. **CR gates depth** (see
   [component-bestiary-reference](../../knowledge-notes/component-bestiary-reference.md)):
   CR7+ requires an `accessibility-per-component` pass and a `decision-record`.
5. **Document** — `ai-component-description` + `usage-guidelines`.
6. **Release** — version bump (`version-bump-advisor`) + comms (`change-communication`).

## Step 2 — Attach templates + acceptance criteria

Each stage gets a short template and a checklist of acceptance criteria that must be true to
advance. Keep them copy-pasteable.

## Produce the process

```
## Contribution Workflow — sized for Level N

**Headline.** <The one gate that matters most at this maturity.>

### Stage 1 — Propose
Template: <fields>  ·  Acceptance: [ ] no existing component covers this  [ ] API sketched
### Stage 2 … Stage 6
<same shape>

### Scope
- Sized for: maturity Level N (<inferred/configured>)
- Not included: deprecation (→ deprecation-process)
```

## Closing note

Adopt the stages you'll actually enforce — a process nobody follows is worse than none. Start
with Propose + Review + Document; add the rest as the team grows.

## Quality checks

- [ ] Six stages, each with a template and acceptance criteria.
- [ ] Rigor matches maturity; CR-gates named for the Review stage.
- [ ] Hands off to the right doc/release skills by name.
- [ ] Output-discipline self-check passed.
