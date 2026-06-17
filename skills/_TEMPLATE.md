# SKILL.md authoring template

Copy the frontmatter + section skeleton below into `skills/<kebab-name>/SKILL.md`. This
file is documentation only — it lives loose in `skills/` so the plugin loader (which scans
`skills/<name>/SKILL.md`) never loads it as a skill. The 4 chained agents follow the same
convention: loose `skills/<name>-agent.md` files.

## Frontmatter

```yaml
---
name: <kebab-case-name>            # MUST equal the folder name
description: "<1–3 sentences. WHAT it does + WHEN to use it, in the user's words.
  Include the natural-language trigger phrases a user would actually type
  ('audit my tokens', 'how healthy is my system'). Then explicit boundaries:
  'Do NOT trigger for X — use <other-skill> for that.' Triggering precision is
  the top quality bar: a plain-language ask must activate EXACTLY ONE skill."
references:
  - ../../knowledge-notes/output-discipline.md   # MANDATORY for audit/validate/assessment skills
  - ../../knowledge-notes/<other-relevant-note>.md
---
```

## Body sections (in this order; omit a section only if genuinely N/A)

```markdown
# <Title>

## Context
2–4 sentences: what this skill is for and the shape of its output.

## Configuration
Which `.ds-ops-config.yml` keys it reads (all optional). State the zero-config behaviour.

## Auto-pull integrations
What it pulls if Figma / GitHub / npm / Chromatic are configured — and that it works
fully without them. Omit if the skill has no integrations.

## Step 0 … Step N
The actual process: discover → gather evidence → analyse → classify. Number the steps.
Step 0 is usually "detect maturity / calibrate depth / locate inputs".

## Produce the report
A concrete output template using ONLY the label sets from output-discipline. Headline
first. Concrete enough that a stranger to the codebase could act on a finding.

## Closing note
Invite the reader to flag intentional deviations and mark them accepted in config.

## Quality checks
A short self-verification list the skill runs before returning (include the
output-discipline self-check for any audit/validate skill).
```

## Authoring rules

- **Reference paths are relative from the skill file:** `../../knowledge-notes/<note>.md`.
  Verify every path resolves before committing.
- **Every audit / validate / assessment skill MUST reference and obey
  `output-discipline.md`** — label sets only, scope note, headline-first, peer tone, length
  ceiling.
- **Boundaries are airtight.** For each neighbouring skill this could be confused with, add
  an explicit `Do NOT trigger for X — use <skill>` line in the description.
- **Instructions only.** A skill never executes code, makes network calls, sends data
  anywhere, or adds telemetry. Skills that emit runnable code (`codemod-generator`,
  `cicd-integration`, `migrate.js`) ship it as a **proposal to review**, dry-run-first, with
  a review banner.
- **No secrets.** Reference integration credentials by environment-variable name only.
- **Calibrate to maturity.** Recommendations scale to the five-level maturity model
  (ad-hoc → optimised). Don't prescribe enterprise governance to a four-component system.
- **Tuned defaults, configurable mechanism.** Reference our real tokens, paths, and ESLint
  rules as the expected case, but read `.ds-ops-config.yml` for overrides.
