# AI-Readiness (reference)

What makes a design system legible to AI agents that consume it — to write conformant code,
answer "which component for X", and resolve tokens without guessing. Loaded by
`system-health`, `codebase-index`, `context-engine-builder`, `metadata-schema-generator`,
`ai-component-description`. Pairs with [output-discipline](output-discipline.md).

## The assessment axes

Rate 🟢/🟡/🟠/🔴 per axis:

1. **Naming predictability** — can an agent guess the right symbol from intent? (PascalCase
   components, semantic token names, consistent patterns.)
2. **Metadata presence** — do components expose prop names/types and a stated purpose; do
   tokens carry intent? Thin metadata forces guessing.
3. **Structural consistency** — same folder shape, barrel exports, co-located tests, so an
   agent can navigate by convention.
4. **Machine-readable index** — is there a `.ai/index/` (or equivalent) an agent can query?
   (→ `codebase-index`.)
5. **Documented conventions** — a single source of truth for "how we build here". A rich
   conventions guide (e.g. a thorough `CLAUDE.md`) is a genuine asset and should be counted
   as one.

## The inferred-vs-ground-truth rule

Anything an agent or skill *infers* (a component's purpose, a token's intent) must be marked
inferred wherever it is emitted. Never let inference harden into a fact downstream consumers
trust. `codebase-index` and `metadata-schema-generator` enforce this in their output.

## How skills use this note

- `system-health` scores the AI-readiness dimension on these axes.
- `codebase-index` / `context-engine-builder` produce the index + context that raise axes 4
  and 5.
- `metadata-schema-generator` / `ai-component-description` raise axis 2.

## Calibration

A small system doesn't need a metadata schema; it needs predictable names and one conventions
doc. Recommend the cheapest axis that unblocks reliable agent consumption first.
