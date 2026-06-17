# Human-Oversight Framework (reference)

What requires a human decision before it happens. Loaded by every agent and by every skill
that emits runnable artefacts (`codemod-generator`, `cicd-integration`, the generated
`migrate.js`, `migration-agent`). Pairs with [output-discipline](output-discipline.md).

## Decision rights

| A skill/agent MAY do autonomously | A human MUST decide |
|-----------------------------------|---------------------|
| Read source, configs, history | Apply any change to source files |
| Analyse, classify, measure | Run a generated codemod / migration |
| **Propose** changes, code, plans | Merge, publish, release, tag |
| Write report/index/context artefacts locally | Deprecate or remove anything consumers use |
| Emit codemods/workflows as files to review | Send anything outward (PR, comms, external service) |

## Proposal-only doctrine

Agents and code-emitting skills produce **analysis and recommendations**. They make no
changes. Every action item is staged for a human to decide and execute. Each agent states
this at the top, verbatim:

> **Proposal only.** Produces analysis and recommendations; makes no changes. Every action
> item requires human decision and execution.

## Dry-run-first

Any generated executable (codemod, migration runner, CI workflow) ships with:

- a **review banner** at the top of the file stating it is generated and unreviewed,
- a **`--dry-run` default** (or report-only mode) so the first run changes nothing,
- explicit instructions to inspect the diff before applying.

## No silent scope

If a generated artefact or an automated step would change more than the reader expects
(touches N files, requires a secret, runs on every PR), say so plainly before they adopt it.
This is the output-discipline "cut scaffolding / no silent caps" rule applied to actions.
