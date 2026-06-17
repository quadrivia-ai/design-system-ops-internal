# Component Governance (reference)

How a design system is *run* as it matures, and the canonical maturity ladder that calibrates
every recommendation in the pack. Loaded by `system-health`, `component-audit`,
`system-benchmark`, `triage`, `contribution-workflow`, `deprecation-process`,
`governance-encoder`, and the onboarding skills. Pairs with [output-discipline](output-discipline.md).

## The five-level maturity model (canonical)

Every skill that calibrates to maturity uses these levels and signals. The level scales the
*urgency* of a gap — never whether it is a gap (see output-discipline §4).

| Level | Name | Signals |
|-------|------|---------|
| 1 | **Ad-hoc** | No shared tokens/components; values hardcoded; copy-paste UI. |
| 2 | **Emerging** | A token file + some shared components exist, used inconsistently; no enforcement. |
| 3 | **Structured** | Semantic token layer + a shared library in real use; some automated enforcement (lint); conventions written down. |
| 4 | **Managed** | Enforcement blocks drift in CI; contribution + deprecation are defined processes; adoption is measured; theming is systematic. |
| 5 | **Optimised** | The system is run as a product: versioning, decision records, adoption targets, AI-readable metadata, multi-surface/brand, continuous measurement. |

Place a system by its **weakest load-bearing** signal, then note the trending direction. A
system can be level 4 on enforcement and level 2 on documentation — say so; don't average.

## Governance primitives

A maturing system needs these as **named, owned processes**, not folklore:

- **Ownership** — who decides what's canonical (a person, a group, or CODEOWNERS).
- **Contribution** — how a new component/token enters the system (→ `contribution-workflow`).
- **Deprecation** — how something leaves, with a migration path and sunset date (→ `deprecation-process`).
- **Decisions** — architectural choices recorded with context and consequences (→ `decision-record`).
- **Enforcement** — what is checked automatically vs by review (→ `governance-encoder`, `cicd-integration`).
- **Change communication** — how consumers learn what changed (→ `change-communication`).

## How skills use this note

- Assessment skills (`system-health`, `component-audit`, `system-benchmark`) read the ladder
  to set expectations to scale.
- Govern skills produce the primitives above, sized to the current level — a level-2 system
  gets a lightweight contribution checklist, not a level-5 RFC process.

## Calibration rule

Recommend the **next** level's practice, not the summit. Moving a Structured (3) system
toward Managed (4) means automating enforcement and writing down contribution/deprecation —
not standing up an enterprise governance board.
