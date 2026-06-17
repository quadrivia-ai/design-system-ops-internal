---
name: version-bump-advisor
description: "Recommend a semver bump (patch / minor / major) from a described or diffed set of design-system changes, with the reasoning per change and the breaking ones called out. Triggers on 'what version bump is this', 'should this be major or minor', 'semver advice for these changes', 'version bump advisor'. Do NOT trigger for writing the release notes — use change-communication. Do NOT trigger for a full release retro — use release-retrospective."
references:
  - ../../knowledge-notes/output-discipline.md
---

# Version Bump Advisor

## Context

Recommends a semver bump for a set of design-system changes by classifying each change, then
taking the highest class. For a token/component library, "breaking" includes more than code
signatures — a changed token *value*, a removed variant, or a visual default shift can break
consumers too.

## Configuration

Reads (all optional): the change set (diff or described); `stack.token_files`,
`stack.component_roots` (to spot token/API changes).

## Auto-pull integrations

None required. If GitHub is configured, MAY read the diff directly.

## Step 0 — Gather changes

Collect the changes (from a diff, a PR list, or a description). For each, identify what
consumers see.

## Step 1 — Classify each change

- **Major (breaking)** — removed/renamed component/prop/token; changed token *value* that
  shifts appearance; removed variant; changed default behaviour.
- **Minor (additive)** — new component/variant/token; new optional prop; backward-compatible.
- **Patch** — bug fix, internal refactor, docs, no consumer-visible change.

## Step 2 — Recommend

The highest class present sets the bump. List the specific changes that forced it — especially
the breaking ones, since those are the argument.

## Produce the recommendation

```
## Version Bump — recommend <major | minor | patch>

**Headline.** <The bump + the single change that drove it.>

| Change | Class | Consumer impact |
|--------|-------|-----------------|
| `--accent` value 245→250 | major | visual shift everywhere accent is used |
| add `Button size="xs"` | minor | additive |
| fix focus ring leak | patch | none |

### Why <major>
<the breaking changes, each one line>

### Scope
- Inspected: <change set source>
- Assumed: token-value changes treated as breaking (appearance shift)
```

## Closing note

When unsure between two classes, round up — a surprise major is annoying; a breaking change
shipped as a minor erodes trust in the version number entirely.

## Quality checks

- [ ] Every change classified; the bump = the highest class.
- [ ] Token-value/visual-default changes evaluated as potentially breaking, not just signatures.
- [ ] Breaking changes listed explicitly as the rationale.
- [ ] Output-discipline self-check passed.
