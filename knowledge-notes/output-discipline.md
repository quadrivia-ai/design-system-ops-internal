# Output Discipline (doctrine)

The non-negotiable house style for every audit, validation, and assessment skill in this
pack. If a skill produces a report a human reads, it obeys this note — cite it in
`references:` and follow it exactly. The point: output that reads like a senior teammate
thinking alongside the reader, calibrated to what's actually there, never a graded
compliance filing.

**The baseline is established design-system best practice.** Stay specific to the codebase
for evidence and remediation — real `file:line`, real token names — but never treat the
current architecture as correct by default. Surface gaps honestly at the severity best
practice warrants; let the *reader* decide what's intentional (§4).

## 0. The one-line test

Before returning anything, ask: *"Would a senior colleague sitting next to the reader say
it this way?"* If it reads like a grade, an audit filed *against* someone, or a generic
best-practices listicle, rewrite it.

## 1. No numeric scores. Ever.

No `X/10`, no percentages-as-grades, no letter grades, no star totals, no composite
"health score". They invent precision the evidence doesn't support and turn a conversation
into a verdict. Use **only** these label sets:

**Dimension health** — how mature/healthy an area is:
> 🟢 Strong · 🟡 Functional · 🟠 Weak · 🔴 Absent

**Finding severity** — how much a single issue should worry the reader:
> 🔴 Critical · 🟠 High · 🟡 Medium · ⚪ Low

**Check result** — pass/fail of a discrete check:
> ✅ PASS · ⚠️ WARN · ❌ FAIL

**Factual counts are allowed** — they are measurements, not ratings: "78 of 84 components
have a co-located test", "12 hardcoded hex values across 5 files". Never convert a count
into a grade ("92% → A−"). State the count; let the label carry the judgement.

When a skill must report two things about the same object — e.g. *how confident/thorough
the audit was* and *how good the thing is* — report them as **two separate labels**, never
multiplied into one number. (`design-to-code-check` and `accessibility-per-component`
depend on this.)

## 2. Scope humility

- Never write "X does not exist." Write **"X was not found in the files scanned."**
- Absence of evidence ≠ evidence of absence. A skill knows only the files it read.
- **Every report ends with a Scope note** (skeleton below): inspected / not inspected /
  assumed.

## 3. Senior-peer tone

Write as a colleague reviewing *alongside* the reader, not an auditor filing *against*
them. "We're referencing the raw red token in three buttons — worth routing through
`--destructive`" beats "VIOLATION: non-compliant colour usage detected." Lead with
"here's what I'd do next," not severity for its own sake.

## 4. Respect intentional decisions

**Respecting a decision is not pre-excusing it.** Measure against best practice, surface the
gap at honest severity, and name the trade-off — then let the *reader* mark it accepted.
Never soften a severity, or rate a gap 🟢/🟡 yourself, just because it looks deliberate or is
the current architecture. That call belongs to the reader, recorded in `accepted_findings`.

Many "issues" are deliberate trade-offs. Frame findings so the reader can say "that's on
purpose":

- Phrase as observation + suggested direction, not mandate.
- Invite the reader to mark a finding **accepted**. Accepted findings live in
  `.ds-ops-config.yml › accepted_findings` (by fingerprint) and are skipped on future runs.
  Note this at the end of any report carrying ≥1 finding.

## 5. Headline first

Open every report with **how worried should I be, and what to do first** — never
methodology. The first three lines answer: overall posture (one health label), the single
most important thing, and the immediate next action. Methodology, if any, goes last.

## 6. Cut scaffolding, keep substance

- Omit empty sections entirely. No "N/A", no "Nothing to report here".
- One caveat per finding — the most important one. Not three hedges.
- Every finding keeps three things: **evidence** (`file:line` or a count), **why it
  matters** (one line), and **specific remediation** (the actual token / rename / codemod —
  not "consider improving").
- Target lengths (ceilings, not goals): focused answer **< 500 words** · single-dimension
  audit **< 1,500** · full-system audit **< 3,000**.

## Report skeleton (reuse this)

```
## <Skill> — <subject>

**<🟢/🟡/🟠/🔴> Headline.** <One sentence: posture + the first thing to do.>

<If multi-dimension: a compact table — dimension → health label → one-line note.>

### Findings
**<🔴/🟠/🟡/⚪> <Short title>**
- Evidence: `path/to/file.tsx:42`  (or: "9 of 12 components …")
- Why it matters: <one line>
- Remediation: <the specific change — the token, the rename, the codemod>
- Caveat: <the single most important caveat — omit this line if none>

### Scope
- Inspected: <globs / files / integrations actually read>
- Not inspected: <what was out of scope>
- Assumed: <assumptions made>
- Mark any finding accepted in `.ds-ops-config.yml › accepted_findings` to skip it next run.
```

## Self-check (run before returning)

- [ ] Zero numeric scores/grades — only the three label sets and factual counts.
- [ ] Headline is first and answers "how worried + what first".
- [ ] Every finding has evidence + why + remediation; ≤ 1 caveat.
- [ ] No "doesn't exist" phrasing; Scope note present.
- [ ] Under the length ceiling for this report type.
- [ ] Reads like a peer; intentional choices can be flagged and accepted.
