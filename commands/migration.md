---
description: Run the migration agent — understand → transformation table → codemod → four-phase rollout.
argument-hint: "[old scheme → new scheme]"
---
Run the **migration-agent** for the migration in $ARGUMENTS. Produce the transformation table (incl. manual cases), a dry-run-first codemod, and the four-phase rollout (add → migrate → deprecate → remove) with entry/exit criteria + rollback checkpoints + comms. PROPOSAL ONLY — nothing applied.
