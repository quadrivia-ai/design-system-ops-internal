---
description: Run the release-readiness agent for one component (gates with hard stop on critical).
argument-hint: "[ComponentName or path]"
---
Run the **component-to-release-agent** on $ARGUMENTS. Gates in order — design-to-code-check → accessibility-per-component → token-compliance — hard-stop on the first 🔴; produce docs + release note only if gates clear. Verdict: READY / READY-WITH-NOTES / BLOCKED. Proposal only.
