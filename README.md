# Design System Ops (internal)

A Claude Code skill pack for **staff-level design-system operations** on the Quadrivia
frontend — auditing, governance, documentation, validation, and communication, the way a
senior practitioner would: structured process, evidence-based findings, and output
calibrated to what's actually in the codebase.

Tuned to our stack: **React 18 + TypeScript + Tailwind v3** with **HSL-triplet CSS custom
properties**, shadcn/ui, Vitest, and the existing `no-hardcoded-tailwind-colors` /
`no-raw-tailwind-typography` ESLint rules. It reads source files locally and never uploads
your code anywhere.

## What you can ask for

Describe the task in plain language; exactly one skill activates:

| You say… | You get |
|----------|---------|
| "audit my tokens" | `token-audit` |
| "how healthy is my design system?" | `system-health` |
| "what's drifted from the design?" | `drift-detection` |
| "check this component for accessibility" | `accessibility-per-component` |
| "help me deprecate the old Button" | `deprecation-process` |
| "write a stakeholder brief" | `stakeholder-brief` |
| "generate a migration codemod" | `codemod-generator` (proposal only) |

39 skills across **Audit / Govern / Document / Validate / Communicate**, plus 4 chained
agents and 13 slash-command wrappers. Full list in
[2-WHATS-INCLUDED.md](2-WHATS-INCLUDED.md).

## House rules (every report obeys these)

No numeric scores, grades, or percentages — **labels only**:

- Dimension health: 🟢 Strong · 🟡 Functional · 🟠 Weak · 🔴 Absent
- Finding severity: 🔴 Critical · 🟠 High · 🟡 Medium · ⚪ Low
- Check result: ✅ PASS · ⚠️ WARN · ❌ FAIL

Headline first ("how worried should I be, and what first"). A **Scope** note on every
report. Evidence + specific remediation on every finding. Senior-peer tone, not auditor.
The full doctrine: [knowledge-notes/output-discipline.md](knowledge-notes/output-discipline.md).

## Guardrails

- **Instructions only.** No skill executes code, makes outbound network calls, sends your
  data anywhere, or includes telemetry.
- **Code-emitting skills are proposals.** `codemod-generator`, `cicd-integration`, and any
  generated `migrate.js` ship as code to review, dry-run-first — never auto-run.
- **No secrets in any file** — environment-variable *names* only.
- **`visual-report` is offline-openable** — no CDN or external calls; CSP-safe.

## Install & setup

- [1-INSTALL.md](1-INSTALL.md) — install in Claude Code and Cowork; entry points by use case
- [2-WHATS-INCLUDED.md](2-WHATS-INCLUDED.md) — every skill, agent, note, and command
- [3-SETUP-AND-CONFIG.md](3-SETUP-AND-CONFIG.md) — `.ds-ops-config.yml` reference, monorepo
  handling, integrations, troubleshooting

Zero-config by default: skills scan the current repo and work without any integrations.
Edit [.ds-ops-config.yml](.ds-ops-config.yml) to tune severity, point at token files, or
wire Figma/GitHub/Chromatic.

## License & provenance

MIT © Quadrivia AI. This is **original work**. It mirrors the *architecture and capability
set* of the open-source "Design System Ops" pack (MIT) but contains none of its prose; if
any upstream text is ever quoted verbatim, a `NOTICE` file will carry the attribution.
