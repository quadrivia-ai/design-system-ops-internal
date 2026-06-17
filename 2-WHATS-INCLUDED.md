# 2 · What's Included

**39 skills · 4 agents · 11 knowledge notes · 13 commands · 1 config · 1 manifest.**

All skills follow one anatomy ([skills/_TEMPLATE.md](skills/_TEMPLATE.md)) and obey the house
doctrine ([knowledge-notes/output-discipline.md](knowledge-notes/output-discipline.md)): no
numeric scores, headline first, scope note always, best practice as the bar.

## Skills

### Audit (9) — understand what you have
| Skill | Does |
|-------|------|
| `token-audit` | How tokens are *defined*: tiering, theme-definition parity, structural debt, DTCG readiness |
| `component-audit` | Library health: coverage by Challenge Rating, duplication, complexity, orphans |
| `system-health` | Holistic 7-dimension read, maturity-placed, routing to the deeper skill per dimension |
| `drift-detection` | System-wide divergence + cause classification (incl. system-gap) |
| `naming-audit` | Naming consistency across components/tokens/files/props + cross-tier collisions |
| `figma-variable-audit` | Figma variable collections vs the three-tier model + code parity (diagnostic if no Figma) |
| `codebase-index` | Emits machine-readable `.ai/index/` (inventory + graph + protocols) |
| `system-benchmark` | Vs named external systems across ~12 dimensions, peer-scale calibrated |
| `theme-audit` | Resolution + contrast + parity across light/dark/colorblind |

### Validate (6) — verify before it ships
| Skill | Does |
|-------|------|
| `design-to-code-check` | One component vs spec across 5 dimensions; reports confidence AND match separately |
| `accessibility-per-component` | One component vs WCAG 2.1 AA; reports thoroughness AND accessibility separately |
| `token-compliance` | Scans code for raw values → file+line+current+recommended token; flags enforcement gaps |
| `schema-validator` | Validates token/metadata/.ai-index files against a schema (✅/⚠️/❌ per file) |
| `component-api-validator` | Prop-API consistency across the library (booleans, events, variants, refs) |
| `cicd-integration` | Generates a report-only-first CI workflow proposal (secrets via env only) |

### Govern (11) — run it as infrastructure
| Skill | Does |
|-------|------|
| `triage` | Entry point: classify stage, output a prioritised run plan of 3–5 skills |
| `contribution-workflow` | Six-stage contribution process + templates, sized to maturity |
| `deprecation-process` | Blast radius, migration path, comms timeline, sunset date (proposal) |
| `decision-record` | Capture an architectural decision (context/options/decision/consequences) |
| `change-communication` | Release notes + migration guide + announcement, scaled to impact |
| `backlog-generator` | Findings → structured, estimated, prioritised work items |
| `version-bump-advisor` | Recommend a semver bump from a change set (token-value changes count as breaking) |
| `release-retrospective` | Structured, blame-free retro on a release cycle |
| `governance-encoder` | Emit machine-readable governance rules (origin-tagged; inferred flagged) |
| `session-memory` | Save/Recall/Compare/Correlate findings across runs (2/3/4+ corroboration) |
| `codemod-generator` | Generate a dry-run-first codemod + `migrate.js` (proposal; never runs) |

### Document (7) — make it legible to humans and machines
| Skill | Does |
|-------|------|
| `ai-component-description` | Six-section AI/Figma-MCP component spec (diagnostic if metadata thin) |
| `pattern-documentation` | Document a composed pattern: composition, state coverage, a11y (diagnostic mode) |
| `token-documentation` | Document token intent by tier + theming contract + do/don't (diagnostic mode) |
| `usage-guidelines` | When-to-use / when-not + anti-patterns from real misuse (diagnostic mode) |
| `component-decision-tree` | A which-component-for-which-need tree from the real library |
| `context-engine-builder` | Build the agent context layer (conventions/do-don't/protocols) on the index |
| `metadata-schema-generator` | Generate the component/token metadata schema (pairs with schema-validator) |

### Communicate (6) — move people and decisions
| Skill | Does |
|-------|------|
| `adoption-report` | Coverage vs adoption (separately) + at-risk areas + first-run baseline |
| `stakeholder-brief` | One-page business brief: risk/cost/velocity/investment, no jargon |
| `system-pitch` | The investment case: ROI framing + rebuttals to common objections |
| `designer-onboarding` | Two-week designer ramp, calibrated to complexity |
| `engineering-onboarding` | Two-week engineer ramp (tokens/components/conventions/enforcement) |
| `visual-report` | Self-contained offline HTML dashboard (labels + counts; no CDN; CSP-safe) |

## Agents (4) — chained, proposal-only

| Agent | Chains | Notes |
|-------|--------|-------|
| `full-system-diagnostic-agent` | token + component + naming + drift + health → synthesis | small-system gate (<5 components) |
| `component-to-release-agent` | design-to-code + a11y + token-compliance → docs + comms | hard stop on first 🔴 |
| `governance-review-agent` | adoption + drift → stakeholder-brief | engineering view + leadership view |
| `migration-agent` | token + naming → transformation table → codemod → 4-phase rollout | dry-run-first; rollback checkpoints |

## Knowledge notes (11)

`output-discipline` (doctrine) · `token-architecture` · `component-governance` (5-level maturity) ·
`component-bestiary-reference` (Challenge Rating) · `ai-readiness` · `design-to-code-contract` ·
`human-oversight-framework` · `agent-orchestration-guide` · `mcp-setup-guide` ·
`adoption-measurement` · `context-engine-blueprints`.

## Commands (13)

`/token-audit` `/component-audit` `/system-health` `/system-benchmark` `/drift-check`
`/describe-component` `/visual-report` `/codemod-generator` `/cicd-integration`
`/full-diagnostic` `/release-check` `/governance-review` `/migration`

## Frameworks encoded across the pack

Three-tier token architecture · five-level maturity model (calibrates every recommendation) ·
Component Challenge Rating (calibrates doc/validation depth) · AI-readiness assessment.
