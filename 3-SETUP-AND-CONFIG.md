# 3 · Setup & Configuration

Everything works with **zero config**. This file is for when you want to tune it.

## `.ds-ops-config.yml` reference

All keys are optional. Place the file at the root of the repo you analyse.

| Key | Purpose | Default |
|-----|---------|---------|
| `target_repo` | Root the skills scan | `"."` (CWD) |
| `source_globs` | Where component/token source lives | `src/**/*.{ts,tsx,css}` |
| `ignore_globs` | Excluded paths | `node_modules`, `dist`, `*.test.*` |
| `stack.styling` | Styling system | `tailwind` |
| `stack.token_format` | How tokens are expressed | `css-custom-properties-hsl` |
| `stack.token_files` | Token definition files | `src/index.css`, `src/design-system/care-journeys/tokens.css` |
| `stack.theme_classes` | Theme root classes | `light`, `dark`, `colorblind` |
| `stack.component_roots` | Where components live | `src/components/{ui,atoms,molecules,organisms}` |
| `stack.ui_library` | Informs duplication/drift heuristics | `shadcn` |
| `stack.test.runner` / `.convention` | Test signal | `vitest` / `co-located` |
| `stack.lint.token_rules` | Existing enforcement to assess against | the two ESLint rules |
| `maturity_level` | Override the self-assessed level (1–5) | `null` (auto) |
| `severity_overrides` | Re-grade a finding type | `{}` |
| `accepted_findings` | Fingerprints to skip (intentional decisions) | `[]` |
| `integrations.*` | Optional Figma/GitHub/Chromatic/docs (env-var NAMES only) | empty |
| `output.report_dir` / `trend_dir` | Where reports / run snapshots go | `.ds-ops/reports`, `.ds-ops/history` |

### Accepting intentional findings

When a skill flags something you've decided on purpose, add its fingerprint to
`accepted_findings` and future runs skip it:

```yaml
accepted_findings:
  - "token-audit:no-primitive-tier"            # we accept value duplication across themes for now
  - "token-compliance:hardcoded-color:src/pages/marketing/Hero.tsx"
```

The skill prints each finding's fingerprint so you can copy it. This is how the pack respects
deliberate decisions without ever pre-excusing them itself.

## Monorepo handling

For a multi-package repo, run per-package: set `target_repo` to the package root (or pass a
path to the command, e.g. `/token-audit packages/web/src/styles.css`), and point
`stack.token_files` / `stack.component_roots` at that package. Run the skill once per package;
`session-memory` keeps each package's snapshots separate by path. This repo (`frontend`) is a
single package, so the defaults apply as-is.

## Integrations (all optional)

Skills run fully offline; integrations only *enrich* signals. Store **env-var names**, never
secrets (see [mcp-setup-guide](knowledge-notes/mcp-setup-guide.md)):

- **Figma** — `integrations.figma.file_key` + `access_token_env: FIGMA_ACCESS_TOKEN`, or the
  ClaudeTalkToFigma MCP. Used by `figma-variable-audit` (and optionally `design-to-code-check`).
- **GitHub** — `integrations.github.repo` + `token_env: GITHUB_TOKEN` (read-only). Enriches
  history signals for `component-audit`, `drift-detection`, `adoption-report`.
- **Chromatic / docs** — `*_token_env` / `api_key_env`. 

Every token should be **least-privilege and read-only** where the provider supports it. A
skill that can't reach an integration runs in **diagnostic mode** (states what it couldn't
reach, marks it N/A, never fabricates).

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| A skill doesn't trigger from natural language | It's a description-match; use the slash-command (`/token-audit`) or name the skill. Check the install symlink/copy. |
| Two skills both seem to apply | They shouldn't — each description has explicit `Do NOT trigger for X` boundaries. Pick by the boundary lines; if genuinely ambiguous, file it as a triggering bug. |
| Figma skill says "diagnostic mode" | No `file_key`/token reachable — set `integrations.figma` or connect the MCP. |
| A report cites a token/path that's wrong | Your `stack.token_files` / `component_roots` differ from defaults — set them in config. |
| Findings I've accepted keep reappearing | Add their fingerprints to `accepted_findings`. |
| "Where did my report go?" | `output.report_dir` (default `.ds-ops/reports`); add `.ds-ops/` to `.gitignore` if you don't want it committed. |
