> **Illustrative sample.** Shows the shape of a `system-health` run against a React + Tailwind
> SPA with semantic tokens, shadcn/ui, Vitest, and no Storybook. Values are representative,
> not a live measurement.

## System Health — frontend

**🟡 Functional and well-enforced, under-documented.** Maturity: **Level 3 (Structured),
trending → 4**. The system is enforced at the token level but its component intent lives only
in code and one prose guide — close the documentation gap before scaling adoption.

| Dimension | Health | Evidence | Go deeper |
|-----------|--------|----------|-----------|
| Tokens | 🟡 | Semantic layer enforced by 2 ESLint rules; no primitive tier | `token-audit` |
| Components | 🟡 | shadcn/ui base; co-located tests on a minority of components | `component-audit` |
| Documentation | 🟠 | No Storybook/MDX; intent lives in one prose guide | `usage-guidelines` |
| Adoption | 🟡 | Shared usage strong in mature areas, thin in newer ones | `adoption-report` |
| Governance | 🟡 | Conventions documented + lint-enforced; no decision records found | `decision-record` |
| AI-readiness | 🟢 | Rich machine-consumable conventions guide; predictable naming | `context-engine-builder` |
| Platform maturity | 🟢 | 3 themes incl. colorblind; mobile tap-target rule; Vitest + Playwright | `theme-audit` |

### Top 3 priorities
1. 🟠 **Component documentation** — intent is unwritten outside one guide; new contributors and
   agents infer it. Run `ai-component-description` on the CR7+ components first, then
   `usage-guidelines`.
2. 🟡 **Decision records** — strong conventions exist but the *why* isn't captured, so they get
   relitigated. Start an ADR log with `decision-record`.
3. 🟡 **Token primitive tier** — a real gap before any brand variant; plan it with `token-audit`
   → `migration`.

### Scope
- Inspected: component roots, token files, test glob, lint config, the conventions guide.
- Not inspected: per-dimension depth (each defers to its skill); runtime behaviour.
- Assumed: maturity self-assessed (no `maturity_level` set).
- Mark accepted findings in `.ds-ops-config.yml › accepted_findings`.
