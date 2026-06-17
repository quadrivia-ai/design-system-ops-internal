---
name: change-communication
description: "Produce release communication — release notes, a migration guide, and an announcement — scaled to the impact level of the change (a quiet patch note vs a breaking-change campaign). Triggers on 'write release notes for X', 'announce this change', 'migration guide for the token rename', 'comms for the deprecation'. Do NOT trigger for choosing the version number — use version-bump-advisor. Do NOT trigger for the deprecation plan itself — use deprecation-process (this writes its comms)."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/component-governance.md
---

# Change Communication

## Context

Writes the human-facing comms for a change, scaled to its impact: a patch gets a one-line
note; a breaking change gets release notes + a migration guide + an announcement. The goal is
that no consumer is surprised.

## Configuration

Reads (all optional): the change set (described or diffed); `integrations.github.repo` (for
links); `output.report_dir`.

## Auto-pull integrations

None required. If GitHub is configured, MAY link PRs/commits in the notes.

## Step 0 — Set the impact level

Patch (fixes, no API change) · Minor (additive) · Major (breaking). The level decides how much
comms to produce — don't write a campaign for a patch.

## Step 1 — Produce the right artefacts

- **Release notes** — grouped Added / Changed / Fixed / Deprecated; user-facing language.
- **Migration guide** (minor-with-deprecations and major) — before/after per breaking change,
  ordered by how many consumers it hits.
- **Announcement** (major) — what, why, what you must do, by when, where to get help.

## Produce the comms

```
## <Release vX.Y.Z> — <impact level>

**Headline.** <The one thing consumers must know/do.>

### Release notes
**Added** — …  **Changed** — …  **Fixed** — …  **Deprecated** — …

### Migration (if breaking)
**<breaking change>** — <N consumers affected>
```tsx
// before → after
```

### Announcement (if major)
<what · why · required action · deadline · help channel>

### Scope
- Impact level: <patch/minor/major> from the change set described
- Assumed: <which changes are breaking>
```

## Closing note

Lead every artefact with the required action, not the changelog. Consumers skim — put "what
you must do" first.

## Quality checks

- [ ] Comms scaled to impact (no campaign for a patch).
- [ ] Every breaking change has a before/after and an affected-consumer count.
- [ ] Required action leads; no numeric scores.
- [ ] Output-discipline self-check passed.
