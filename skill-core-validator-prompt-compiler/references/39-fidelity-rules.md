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
- the emitted artifact envelope matches the requested `transportMode`
- validator-required target roles are resolved and embedded or faithfully rewritten
- runtime-called templates are embedded or faithfully rewritten into call contracts
- runtime assets are embedded or faithfully rewritten when their contents affect execution
- shared algorithms and gate rules needed at runtime are embedded
- deterministic mappings, fallback cascades, selection heuristics, and derived-field rules that affect execution are embedded or faithfully rewritten
- score bands, threshold rubrics, and interpretation text that affect branching or evaluation remain explicit
- local output, gate, validator-contract, and target-contract schemas keep their validation semantics
- multi-surface consistency rules keep every compared surface and comparison relation that affects runtime behavior
- selector, identity, uniqueness, ordering, and cardinality rules that affect target choice or audit scope remain explicit
- authority boundaries between primary inputs, support-only context, and non-authority signals remain explicit
- modality and quantifier semantics that affect runtime behavior remain explicit
- localization granularity rules remain explicit
- every runtime-critical invariant extracted from the source pair has an explicit prompt-body landing point

## Behavioral equivalence rule

Do not call fidelity `preserved` only because the emitted prompt mentions the same topics.
Use `preserved` only when the emitted prompt keeps the same operational behavior for the same input class, including:
- the same audit-versus-invalid gating surface
- the same output-branch surface
- the same authority boundary surface
- the same target-selection surface

## Critical rule classes

For diagnostics, treat these as critical rule classes whenever they appear in the source pair:
- `multi_surface_consistency`
- `branch_sensitive_gating`
- `nested_schema_constraints`
- `selector_identity_cardinality`
- `authority_boundary`
- `modality_and_quantifier`
- `localization_granularity`

## Landing-point rule

For every critical rule class that appears in the source pair, record at least one explicit landing point in `fidelityReport.criticalRuleCoverage`.

A landing point is the concrete runtime location that preserves the behavior, such as:
- an emitted prompt section
- an embedded schema fragment
- an explicit rewritten runtime rule
- an explicit failure rule

Do not mark fidelity `preserved` when a critical rule class exists in the source pair but has no landing point in the emitted artifact.
Do not count diagnostics-only notes as landing points.
The landing point must exist in the emitted prompt body itself.

## Invariant-granularity rule

Critical-rule-class coverage is necessary but not sufficient.
If one critical rule class contains multiple distinct runtime-critical invariants, fidelity is preserved only when each invariant has a recoverable explicit landing point in the prompt body.
Do not collapse multiple invariants into one generic landing point such as `brief alignment`, `baseline checks`, or `schema validation` when the source rules differ in surfaces, quantifiers, selector bindings, or branch effects.

## Prompt-primary fidelity rule

If the emitted prompt body lacks a runtime-critical rule, fidelity is not preserved even when the output json describes that rule correctly.
The output json may justify preservation, but it cannot create preservation on its own.

## Compaction levels

Use these labels for diagnostics:
- `none`
- `light`
- `moderate`
- `aggressive`

Aggressive compaction should be rare and must never be silent.
