# Contributing

This pack is itself a design system of skills — so it's governed like one. New skills follow
one anatomy, obey one doctrine, and keep airtight triggering boundaries.

## Add or change a skill

1. **Start from the template.** Copy the frontmatter + section skeleton from
   [skills/_TEMPLATE.md](skills/_TEMPLATE.md) into `skills/<kebab-name>/SKILL.md`. The `name`
   must equal the folder name.
2. **Obey the doctrine.** Every audit/validate/assessment skill `references:`
   [knowledge-notes/output-discipline.md](knowledge-notes/output-discipline.md) and follows it —
   label sets only (no numeric scores), headline first, scope note, senior-peer tone, length
   ceiling. Best practice is the bar; stay stack-specific for evidence; never pre-excuse the
   current architecture (§4).
3. **Make the boundary airtight.** For every neighbouring skill this could be confused with,
   add an explicit `Do NOT trigger for X — use <skill>` line in the `description`. A
   plain-language ask must activate exactly one skill.
4. **Reference real notes.** Use relative paths `../../knowledge-notes/<note>.md` and verify
   they resolve.
5. **Stay instructions-only.** No skill executes code, makes network calls, sends data
   anywhere, or adds telemetry. A skill that emits runnable code (codemod, CI, migrate.js)
   ships it as a **dry-run-first proposal** with a review banner (see
   [human-oversight-framework](knowledge-notes/human-oversight-framework.md)).
6. **No secrets.** Reference integration credentials by environment-variable name only.
7. **Calibrate to maturity / Challenge Rating** where relevant — don't prescribe enterprise
   process to a small system, or light a11y to a CR8 component.

## Acceptance bar (self-check before opening a PR)

- [ ] Follows the template anatomy; `name` == folder.
- [ ] Description has trigger phrases **and** explicit boundary lines; exactly one skill fires
      for the asks it should own.
- [ ] Audit/validate skills obey output-discipline (run its self-check).
- [ ] Output template is concrete enough that a stranger to the codebase could act on a finding.
- [ ] No secret, telemetry, outbound send, or auto-executing code.
- [ ] All `references:` resolve (run the reference check below).

```bash
# reference integrity — should print nothing
grep -rhoE '\.\./\.\./knowledge-notes/[a-z-]+\.md' skills | sort -u \
  | while read r; do [ -f "knowledge-notes/$(basename "$r")" ] || echo "MISSING: $r"; done
```

## Originality & licensing

This is **original work** mirroring the *capabilities* of the MIT "Design System Ops" pack,
not its prose. Do not paste upstream text. If any upstream text is ever quoted verbatim, add a
`NOTICE` file with the MIT attribution. Contributions are MIT-licensed under the project
copyright.

## Commits

Conventional commits, present tense, no emoji. Keep one logical change per commit; update
`CHANGELOG.md` under *Unreleased* for anything user-facing.
