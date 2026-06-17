---
name: component-to-release-agent
description: "Chained release-readiness agent for ONE component: runs design-to-code-check, accessibility-per-component, and token-compliance in sequence with a HARD STOP on any critical failure, then (if clear) ai-component-description, usage-guidelines, and change-communication — producing one release-readiness verdict. Triggers on 'is this component ready to release', 'release-check the Dialog', 'release readiness for X', 'can we ship this component'. Do NOT trigger for a whole-system diagnostic — use full-system-diagnostic. Do NOT trigger for a single check in isolation — use that validate skill directly."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/agent-orchestration-guide.md
  - ../../knowledge-notes/human-oversight-framework.md
---

# Component → Release (agent)

> **Proposal only.** Produces a readiness verdict and recommendations; ships nothing. The
> release decision is the human's.

## Context

Gates one component toward release through the validate skills in sequence, stopping hard at
the first critical failure so later stages aren't wasted. If the gates pass, it produces the
release artefacts. See [agent-orchestration-guide](../../knowledge-notes/agent-orchestration-guide.md).

## Step 1 — Quality gates (hard stop on 🔴)

In order: `design-to-code-check` → `accessibility-per-component` → `token-compliance`.

**Hard stop:** the first 🔴 Critical halts the chain and reports **release-blocked** with that
blocker. Don't run the documentation/comms stages on a blocked component.

## Step 2 — Release artefacts (only if gates clear)

If no critical blockers: `ai-component-description` (the spec) → `usage-guidelines` (when/when-
not) → `change-communication` (the release note, scaled to impact).

## Produce the release-readiness report

```
## Release Readiness — <component> (CR<n>)

**<✅ READY | ⚠️ READY WITH NOTES | ❌ BLOCKED>** <One line: the verdict + the blocker or the
last thing to confirm.>

### Gates
| Gate | Result | Note |
|------|--------|------|
| Design-to-code | ✅/⚠️/❌ | confidence · match |
| Accessibility | ✅/⚠️/❌ | thoroughness · a11y (CR-gated) |
| Token compliance | ✅/⚠️/❌ | raw values / enforcement |

### If BLOCKED
**Blocker:** <the 🔴 + exactly what to fix>  (later stages not run)

### If READY — release artefacts
- Spec: <from ai-component-description>
- Usage: <from usage-guidelines>
- Release note: <from change-communication>

### Scope
- Ran: the gates above (+ artefacts if clear)
- Assumed: <spec source for design-to-code; CR for a11y depth>
- Nothing released; the decision is yours.
```

## Quality checks

- [ ] Gates run in order; first 🔴 hard-stops and reports the blocker.
- [ ] Documentation/comms produced only when gates clear.
- [ ] Verdict is one of READY / READY-WITH-NOTES / BLOCKED; proposal-only.
- [ ] Output-discipline self-check passed.
