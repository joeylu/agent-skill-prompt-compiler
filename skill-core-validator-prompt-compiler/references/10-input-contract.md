# Input Contract

The invocation payload must match `schemas/compiler-input.schema.json`.

## Required top-level inputs

The required top-level objects are:
1. `requestContext`
2. `validatorSkill`
3. `targetSkill`

`targetRuntime` is optional.
`compilationPolicy` is optional.

## Token budget rule

`requestContext.tokenBudget` is advisory-only.
It informs token reporting for the caller, but it does not change:
- closure decisions
- fidelity decisions
- transport decisions
- example compaction decisions
- support-unit inclusion decisions

`maxPromptTokens` means the caller's preferred upper bound for the emitted prompt artifact estimate.
`reserveForUserPayload` means tokens the caller wants to keep available for later user payload.

## Transport mode rule

`requestContext.transportMode` is an execution-shape request, not a decorative hint.

Meanings:
- `single_prompt`: emit one single-prompt artifact
- `chat_roles`: emit one chat-roles artifact
- `both`: emit both artifacts

If `transportMode = chat_roles` or `both` and `targetRuntime` is supplied:
- use `supportsSystemRole` and `supportsDeveloperRole` to decide the emitted role envelope
- if neither role is supported, block

If `transportMode = chat_roles` or `both` and `targetRuntime` is omitted:
- default to the compiler's standard `system + developer` split

`targetRuntime` currently exposes only role-envelope capability fields.
Do not pass provider metadata, token-window hints, or assistant-prefill hints here unless the compiler grows explicit rules for them.

## Source bundle rule

The caller must pass both bundles as explicit text files.
This compiler does not fetch files from a filesystem on its own.

## Entry resolution rule

Resolve the authoritative entry surface for each bundle in this order:
1. `<bundle>.authoritativeEntryPath`
2. `<bundle>.entrySkillMdPath`
3. `SKILL.md` when present in that bundle's supplied file list

If either bundle entry cannot be resolved, block.

## Pair rule

The supplied payload must represent exactly:
- one validator skill bundle
- one target skill bundle

If either side mixes multiple skills and the active entry cannot be resolved cleanly, block.

## Compiler-facing contract rule

The compiler must also resolve:
- one validator compiler-facing contract
- one target validator-facing contract

Resolution order:
1. `validatorSkill.validatorContractPath`
2. `contracts/validator-compiler.contract.json` inside `validatorSkill.files`

and

1. `targetSkill.targetContractPath`
2. `contracts/target-validator.contract.json` inside `targetSkill.files`

If either contract path cannot be resolved, block.

Do not infer compiler-facing contracts from ordinary prose.
The only allowed authoring additions for pair compilation are:
- one authoritative validator compiler-facing contract file
- one authoritative target validator-facing contract file
- the corresponding `# Reference Map` lines when discoverability is needed

Do not introduce new non-contract documents or new runtime content to satisfy missing roles.
If existing authoritative files plus those allowed contract surfaces do not close the pair safely, block.

## Default compilation policy

If `compilationPolicy` is omitted, use these defaults:
- `missingFilePolicy = block`
- `schemaFlatteningMode = when_helpful`
- `exampleCompactionTokenThreshold = 2000`
- `supportUnitPolicy = keep_when_small`

Closed delivery is a fixed compiler rule, not a caller-tunable policy.
That means:
- runtime closure is always required
- runtime-called templates must be inlined or faithfully rewritten
- runtime assets that affect execution must be embedded or faithfully rewritten

## Output provenance rule

This compiler records provenance in the output json.
Do not spend runtime prompt tokens on source-path annotations unless a future policy explicitly changes that behavior.

## Runtime closure policy rule

Compilation policy may tune compaction of non-runtime/support material, but it does not relax closed pair delivery.
The compiled prompt should stand on its own for execution without requiring the caller to reopen source-side `references/`, `_shared/`, contract, or runtime asset files from either bundle.
