---
name: system-health
description: "Holistic, calibrated sweep of a design system across seven dimensions — tokens, components, documentation, adoption, governance, AI-readiness, and platform maturity — placing the system on a five-level maturity model and pointing to the deeper skill per dimension. Triggers on 'how healthy is my design system', 'design system health check', 'overall state of our design system', 'where are the biggest gaps'. Do NOT trigger for a single dimension — use token-audit (tokens) or component-audit (components). Do NOT trigger for comparison against external/named systems — use system-benchmark. Do NOT trigger for a machine-readable inventory — use codebase-index."
references:
  - ../../knowledge-notes/output-discipline.md
  - ../../knowledge-notes/component-governance.md
  - ../../knowledge-notes/ai-readiness.md
  - ../../knowledge-notes/adoption-measurement.md
---

# System Health

## Context

A breadth-first read of the whole design system: one health label per dimension, the
evidence behind it, and which deeper skill to run next. It does not deep-dive any single
dimension — it tells you where to point one. Maturity calibrates which gaps are *urgent*,
not whether they're gaps — the bar stays best practice.

## Configuration

Reads (all optional): `target_repo`; `stack.*` (token files, component roots, test
convention, lint rules); `maturity_level` (else self-assessed); `severity_overrides`;
`accepted_findings`.

## Auto-pull integrations

None required. If GitHub is configured, MAY use commit/PR signals for the governance and
adoption dimensions; if Figma is configured, MAY note design-source coverage. Works fully
offline from source files.

## Step 0 — Place the system on the maturity ladder

Before judging anything, assess the level (canonical ladder in
[component-governance](../../knowledge-notes/component-governance.md)):

1. **Ad-hoc** — no shared tokens/components; values hardcoded.
2. **Emerging** — some shared components + a token file exist, used inconsistently; no enforcement.
3. **Structured** — semantic token layer + shared library in use; some automated enforcement; conventions documented.
4. **Managed** — enforcement blocks drift in CI; contribution + deprecation processes exist; adoption measured; theming systematic.
5. **Optimised** — system run as a product: versioning, decision records, adoption targets, AI-readable metadata, multi-surface/brand.

State the level and one trending arrow. It scales the *urgency* of each gap below — never
whether something counts as a gap.

## Steps 1–7 — One pass per dimension

For each: a health label, 1–2 concrete evidence points, and the deeper skill to run.

1. **Tokens** — semantic layer present and enforced? Signals: `stack.token_files` exist;
   `no-hardcoded-tailwind-colors` / `no-raw-tailwind-typography` active. → `token-audit`.
2. **Components** — count under `stack.component_roots`; shadcn/ui present; test coverage
   (count co-located `*.test.tsx` vs component files); duplication smell. → `component-audit`.
3. **Documentation** — where is component/token intent written? Storybook? MDX? a central
   prose guide? Note machine- vs human-readable docs. → `usage-guidelines`, `ai-component-description`.
4. **Adoption** — shared-component usage vs one-off local UI; areas lagging (see
   [adoption-measurement](../../knowledge-notes/adoption-measurement.md)). → `adoption-report`.
5. **Governance** — contribution path, deprecation process, decision records, CODEOWNERS,
   enforced conventions. → `contribution-workflow`, `decision-record`, `governance-encoder`.
6. **AI-readiness** — metadata, naming, and structure legible to agents (see
   [ai-readiness](../../knowledge-notes/ai-readiness.md)); a rich repo conventions guide is a
   strong asset here. → `context-engine-builder`, `codebase-index`.
7. **Platform maturity** — theming breadth, responsive/mobile rules, a11y posture, build/test
   tooling. → `theme-audit`, `accessibility-per-component`.

## Step 8 — Synthesise

Cross-reference dimensions for systemic patterns (e.g. strong enforcement + thin docs =
"enforced but unexplained"). Pick the **top three priorities** ordered by leverage, each
naming the one skill to run next.

## Produce the report

```
## System Health — <repo>

**<🟢/🟡/🟠/🔴> Headline.** Maturity: **Level N (name), trending → N+1**. <The single
biggest thing + the first move.>

| Dimension | Health | Evidence | Go deeper |
|-----------|--------|----------|-----------|
| Tokens | 🟡 | semantic layer enforced by 2 lint rules; no primitive tier | token-audit |
| Components | 🟡 | 84 components, 31 with co-located tests | component-audit |
| Documentation | 🟠 | no Storybook; intent lives in one prose guide | usage-guidelines |
| Adoption | 🟡 | … | adoption-report |
| Governance | 🟡 | conventions documented, no decision records found | decision-record |
| AI-readiness | 🟢 | rich machine-readable conventions guide | context-engine-builder |
| Platform maturity | 🟢 | 3 themes, mobile tap-target rule, Vitest + Playwright | theme-audit |

(table values above are illustrative — fill from what you actually scan)

### Top 3 priorities
1. <🔴/🟠> <thing> — why it's #1 — run `<skill>`.
2. …
3. …

### Scope
- Inspected: <roots, token files, test glob, configs read>
- Not inspected: per-dimension depth (each defers to its skill); runtime behaviour
- Assumed: maturity self-assessed (no `maturity_level` set)
- Mark accepted findings in `.ds-ops-config.yml › accepted_findings`.
```

## Closing note

This is a breadth read; each 🟠/🔴 is a pointer, not a verdict — confirm with the named
deeper skill before acting. Flag any dimension whose state is a deliberate choice.

## Quality checks

- [ ] Maturity level stated first and used to calibrate every label.
- [ ] Each dimension has evidence + a named deeper skill; no dimension deep-dived here.
- [ ] Top-three priorities are leverage-ordered and actionable.
- [ ] Output-discipline self-check passed (labels only, headline-first, scope note, peer tone).
