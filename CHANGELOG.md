# Changelog

All notable changes to this pack are documented here. Format follows
[Keep a Changelog](https://keepachangelog.com/); this pack itself versions with semver.

## [0.1.0] — 2026-06-16

Initial internal release.

### Added
- **39 skills** across Audit (9), Validate (6), Govern (11), Document (7), Communicate (6).
- **4 chained agents** (proposal-only, gated): full-system-diagnostic, component-to-release,
  governance-review, migration.
- **11 knowledge notes** encoding the doctrine and frameworks — output-discipline,
  token-architecture, the five-level maturity model, Component Challenge Rating, AI-readiness,
  the design-to-code contract, human-oversight, agent-orchestration, integration setup,
  adoption measurement, and context-engine blueprints.
- **13 slash-commands** wrapping the headline skills and agents.
- Annotated `.ds-ops-config.yml` (zero-config defaults tuned to the Quadrivia frontend;
  env-var names only, no secrets), `.claude-plugin/plugin.json`, and install/reference/setup
  docs.
- Sample outputs for `token-audit`, `system-health`, and `stakeholder-brief`.

### Doctrine
- Output discipline: no numeric scores — label sets only (🟢🟡🟠🔴 / 🔴🟠🟡⚪ / ✅⚠️❌);
  headline-first; scope note on every report; senior-peer tone.
- Best practice is the bar; skills stay stack-specific for evidence but never treat the
  current architecture as correct by default. Intentional decisions are the reader's to accept
  (via `accepted_findings`), not the skill's to pre-excuse.
- Instructions only: no skill executes code, makes network calls, sends data anywhere, or
  includes telemetry. Code-emitting skills are dry-run-first proposals.

### Provenance
- Original work. Mirrors the architecture and capability set of the open-source "Design System
  Ops" pack (MIT); contains none of its prose. No `NOTICE` required at this release.
