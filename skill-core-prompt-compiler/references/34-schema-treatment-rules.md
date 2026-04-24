# Schema Treatment Rules

## Default posture

Treat schemas as executable constraints, not illustrative prose.

## Schema tiering rule

Handle schemas by runtime role:
- local output schemas and final gate schemas: preserve verbatim by default
- local LLM-response schemas that directly constrain runtime validation: preserve verbatim or near-verbatim with no semantic loss
- shared or upstream input schemas: slice only to the phase-local fields that are actually consumed
- purely diagnostic or provenance-oriented schema fragments: may move to the output json

## Verbatim rule

When a schema is already compact and direct, preserve it verbatim.

## Flattening rule

Flatten schema references only when all of the following hold:
- the flattening removes indirection that matters to token cost
- the resulting schema preserves the same validation semantics
- no required keyword, enum, branch, or cardinality rule is lost
- the compiler can still explain the flattening in the output json
- the flattened form does not weaken a local output contract into a loose checklist

## Preserve semantics

Do not change or drop:
- `required`
- `additionalProperties`
- `oneOf` / `anyOf` / `allOf`
- `enum`
- `const`
- numeric bounds
- string bounds
- array item rules
- nullability
- object nesting that affects meaning
- pattern constraints
- min/max item or length constraints
- branch-specific required fields

## Narrative constraint rule

If schema prose such as `description` text encodes runtime expectations, gate meaning, or authoritative field ownership, extract that meaning as a separate rule.
Do not silently drop normative schema prose just because the raw validator keywords remain.
This includes:
- orchestrator-owned recomputation or overwrite rules
- fallback derivation rules
- null-versus-present semantics
- field-generation rules such as how `usage_focus` or similar derived fields are produced
- descriptions that say a field is diagnostic-only, human-review-only, or blocked from downstream programmatic consumption

## Fallback rule

If flattening is risky, keep the original schema shape.
Compactness is secondary to correctness.
