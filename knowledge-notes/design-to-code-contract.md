# Design-to-Code Contract (reference)

The agreed mapping between a design specification and its implementation — the dimensions a
single component or screen is checked on, and how a discrepancy is classified. Loaded by
`design-to-code-check` and `drift-detection`. Pairs with [output-discipline](output-discipline.md)
and [token-architecture](token-architecture.md).

## The five comparison dimensions

A faithful implementation matches the spec across:

1. **Visual** — colour, typography, radius, shadow, icon (via tokens, not raw values).
2. **Layout** — spacing, alignment, sizing against the spacing scale.
3. **Responsive** — behaviour across breakpoints, including the mobile tap-target floor.
4. **State** — every designed state exists (default, hover, focus, active, disabled, loading,
   error, empty).
5. **Token usage** — the right *tier* of token for each property (semantic in code, never raw).

## Discrepancy classification

Tag each mismatch with a cause so the reader knows who fixes it (shared with `drift-detection`):

- **Intentional** — deliberate, contextual deviation → invite acceptance.
- **Version-lag** — implementation predates the current spec → update.
- **Accidental** — a mistake → fix.
- **Misunderstanding** — the affordance was missed → docs/comms.
- **System-gap** — the design system can't express the spec → the system's to fix.

## Report confidence AND match — separately

Two questions, two labels, never multiplied (output-discipline §1):

- **Audit confidence** — how sure the check is, driven by spec availability (live Figma spec →
  high; a verbal description → low). Low confidence is honest, not a failure.
- **Match** — how well the implementation matches the spec *where it could be compared*.

A low-confidence / high-match result and a high-confidence / low-match result are very
different conversations; collapsing them into one number hides which one you're in.
