---
name: release-retrospective
description: "Run a structured retrospective on a design-system release cycle — what shipped, what went well, what hurt (with evidence), and concrete owned improvements for next cycle. Triggers on 'retro on the last release', 'design system release retrospective', 'what should we change next cycle', 'post-release review'. Do NOT trigger for choosing the version number — use version-bump-advisor. Do NOT trigger for writing the release comms — use change-communication."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/component-governance.md
---

# Release Retrospective

## Context

Produces a structured, blame-free retrospective on a release cycle so the next one goes
better. It's a reflective artefact for the team — honest about what hurt, concrete about what
changes, and specific about who owns each change.

## Configuration

Reads (all optional): the cycle's scope (releases, PRs, dates); `integrations.github.repo`
(for shipped-items evidence); `output.report_dir`.

## Auto-pull integrations

None required. If GitHub is configured, MAY pull merged PRs / release tags as the "what
shipped" evidence base.

## Step 0 — Establish what shipped

Summarise the cycle's output factually: components/tokens added/changed, deprecations,
breaking changes. Counts, not grades.

## Step 1 — Went well / hurt

- **Went well** — practices to keep, with the evidence they worked.
- **Hurt** — friction, rework, surprises, escaped drift — each with a concrete example, not a
  vibe.

## Step 2 — Improvements (owned, concrete)

Turn each "hurt" into a specific change for next cycle, with an owner and how you'll know it
worked. Vague resolutions ("communicate better") don't count.

## Produce the retro

```
## Release Retrospective — <cycle>

**Headline.** <The one change that would help most next cycle.>

### Shipped
<factual summary: counts, notable items, breaking changes>

### Went well
- <practice> — evidence: <…>

### Hurt
- <friction> — example: `<PR / file / incident>`

### Next cycle (owned)
1. <change> — owner: <who> — success signal: <measure>

### Scope
- Cycle: <range/scope reviewed>
- Assumed: <data sources>
```

## Closing note

Keep "next cycle" to the 2–3 changes you'll actually make. A retro that produces twelve
resolutions produces none.

## Quality checks

- [ ] "Hurt" items each have a concrete example, not a generalisation.
- [ ] Improvements are owned and have a success signal.
- [ ] Blame-free, peer tone; counts not grades.
- [ ] Output-discipline self-check passed.
