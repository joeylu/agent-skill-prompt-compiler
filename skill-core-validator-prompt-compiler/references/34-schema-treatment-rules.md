# Schema Treatment Rules

## Default posture

Treat schemas as executable constraints, not illustrative prose.

## Schema tiering rule

Handle schemas by runtime role:
- compiler-facing validator contracts and target contracts: preserve verbatim by default
- validator output schemas and final gate schemas: preserve verbatim by default
- validator-local LLM-response schemas that directly constrain runtime validation: preserve verbatim or near-verbatim with no semantic loss
- target-side output or gate schemas required by the validator contract: preserve verbatim or near-verbatim with no semantic loss
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
- the flattened form does not weaken a validator-local output contract or target-local contract into a loose checklist

## Preserve semantics

Do not change or drop:
- `required`
- `additionalProperties`
- `oneOf` / `anyOf` / `allOf`
- `if` / `then` / `else`
- `enum`
- `const`
- numeric bounds
- string bounds
- array item rules
- array-item object shapes that affect nested validation
- nullability
- object nesting that affects meaning
- pattern constraints
- min/max item or length constraints
- branch-specific required fields
- selector equality or identity bindings that are expressed through schema structure

## Branch-sensitive schema rule

If a schema fragment decides any of the following, preserve it verbatim or near-verbatim with no semantic loss:
- whether runtime may enter a normal audit branch
- whether runtime must enter an invalid-input or blocked branch
- which output branch is legal
- whether a target selector or identity is valid
- whether item-level or field-level output shape is valid
- whether a nested array item or nested object member is valid enough to stay in the normal branch

Do not collapse a branch-sensitive schema into a presence-only checklist.
If the original schema says more than "the field exists", the emitted runtime material must also say more than "the field exists".
If the original schema requires specific nested members such as item-level explanation fields, constraint ids, or nested branch discriminators, the emitted runtime material must preserve those nested requirements explicitly.

## Prompt-body rule

When a branch-sensitive schema is rewritten instead of embedded verbatim, the rewritten constraints must appear in the prompt body itself.
Do not move branch-sensitive nested requirements only into diagnostics.

## Narrative constraint rule

If schema prose such as `description` text encodes runtime expectations, gate meaning, or authoritative field ownership, extract that meaning as a separate rule.
Do not silently drop normative schema prose just because the raw validator keywords remain.
This includes:
- validator-owned recomputation or overwrite rules
- target-owned identity or boundary rules
- fallback derivation rules
- null-versus-present semantics
- field-generation rules
- descriptions that say a field is diagnostic-only, human-review-only, or blocked from downstream programmatic consumption
- descriptions that define cross-surface equality, selector uniqueness, or authority boundaries

## Fallback rule

If flattening is risky, keep the original schema shape.
Compactness is secondary to correctness.
