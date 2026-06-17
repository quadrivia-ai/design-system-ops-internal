# 1 · Install

The pack is a Claude Code plugin: a directory of `skills/`, `commands/`, `knowledge-notes/`,
a `.claude-plugin/plugin.json`, and a `.ds-ops-config.yml`. It reads source files locally and
makes no network calls — nothing leaves your machine.

## Option A — quick project-local use (recommended to start)

Claude Code auto-discovers skills in `<repo>/.claude/skills/` and slash-commands in
`<repo>/.claude/commands/`. From the repo you want to analyse (e.g. `frontend`):

```bash
# from the target repo root
mkdir -p .claude
ln -s ../design-system-ops-internal/skills   .claude/skills/ds-ops
ln -s ../design-system-ops-internal/commands .claude/commands/ds-ops
# copy the config to the repo root so skills find it
cp design-system-ops-internal/.ds-ops-config.yml .ds-ops-config.yml
```

(Use `cp -R` instead of `ln -s` if you prefer a copy.) Restart Claude Code; the skills now
auto-trigger from natural language and the slash-commands appear.

## Option B — install as a distributable plugin

Because the pack ships `.claude-plugin/plugin.json`, it can be added through Claude Code's
plugin system (`/plugin`) from a local path or a marketplace repo. Once added, its skills and
the 13 commands are available in every session — no per-repo symlink. This is the path for
sharing it across the team.

## Cowork

Install the same plugin in Cowork; the skills and commands surface identically. The
integration env-vars (Figma/GitHub) are read from the Cowork environment, never from files.

## Verify the install

Ask, in plain language: *"how healthy is my design system?"* → `system-health` should
activate. Or run `/token-audit`. If nothing triggers, see
[3-SETUP-AND-CONFIG.md](3-SETUP-AND-CONFIG.md) → Troubleshooting.

## Entry points by use case

| You want to… | Start with |
|--------------|-----------|
| Not sure where to start | `/system-health`, or the `triage` skill |
| Just inherited a system | `triage` → `codebase-index` |
| Check tokens / components / themes | `/token-audit` · `/component-audit` · `theme-audit` |
| Find inconsistency across the app | `/drift-check` |
| Ship one component safely | `/release-check` |
| Deep end-to-end review | `/full-diagnostic` |
| Brief leadership / make the case | `stakeholder-brief` · `system-pitch` |
| Plan a rename/migration | `/migration` |
| Make findings visual | `/visual-report` |

Everything works with **zero configuration**. Configure `.ds-ops-config.yml` only to tune
severity, point at token files, or wire optional integrations.
