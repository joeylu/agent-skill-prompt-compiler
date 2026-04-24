# Fidelity Rules

## Fidelity outcomes

Use one of these outcomes:

| Value | Meaning |
|---|---|
| `preserved` | semantic fidelity is preserved |
| `reduced` | some non-critical material was omitted under policy but runtime-critical meaning remains intact |
| `blocked` | the compiler could not preserve semantic fidelity safely |

## Default stance

Prefer `preserved`.
Use `blocked` when runtime-critical meaning is at risk.
Do not call fidelity `preserved` when runtime closure remains open.

## Reduced rule

Use `reduced` only when:
- omitted material is not runtime-critical
- the omission is reported explicitly
- the remaining prompt still preserves behavior and boundary meaning
- the prompt remains runtime-closed

## Preserved rule

Use `preserved` only when all of the following hold:
- runtime closure is achieved
- runtime-called templates are embedded or faithfully rewritten into call contracts
- runtime assets are embedded or faithfully rewritten when their contents affect execution
- shared algorithms and gate rules needed at runtime are embedded
- deterministic mappings, fallback cascades, selection heuristics, and derived-field rules that affect execution are embedded or faithfully rewritten
- score bands, threshold rubrics, and interpretation text that affect branching or evaluation remain explicit
- local output and gate schemas keep their validation semantics

## Compaction levels

Use these labels for diagnostics:
- `none`
- `light`
- `moderate`
- `aggressive`

Aggressive compaction should be rare and must never be silent.
