# Input Contract

The invocation payload must match `schemas/compiler-input.schema.json`.

## Required top-level inputs

The required top-level objects are:
1. `requestContext`
2. `sourceSkill`

`targetRuntime` is optional.
`compilationPolicy` is optional.

## Source bundle rule

The caller must pass the source bundle as explicit text files.
This compiler does not fetch files from a filesystem on its own.

## Entry resolution rule

Resolve the authoritative entry surface in this order:
1. `sourceSkill.authoritativeEntryPath`
2. `sourceSkill.entrySkillMdPath`
3. `SKILL.md` when present in the supplied file list

If none can be resolved, block.

## One-skill rule

The supplied bundle must represent exactly one source skill.
If the payload mixes multiple skills and the active entry cannot be resolved cleanly, block.

## Default compilation policy

If `compilationPolicy` is omitted, use these defaults:
- `missingFilePolicy = block`
- `schemaFlatteningMode = when_helpful`
- `exampleCompactionTokenThreshold = 2000`
- `supportUnitPolicy = keep_when_small`
- `runtimeClosureMode = required`
- `templateInliningMode = required_for_runtime_calls`
- `resourceEmbeddingMode = required_for_runtime_assets`

## Output provenance rule

This compiler records provenance in the output json.
Do not spend runtime prompt tokens on source-path annotations unless a future policy explicitly changes that behavior.

## Runtime closure policy rule

Compilation policy may tighten or relax compaction, but the default posture is closed runtime delivery.
The compiled prompt should stand on its own for execution without requiring the caller to reopen source-side `references/`, `_shared/`, or runtime asset files.
