# Normalization Rules

## Goal

Transform collected source material into a smaller runtime prompt without losing semantic fidelity.

## Allowed transformations

The compiler may:
- reorder source material by runtime function
- rewrite verbose prose into shorter atomic instructions
- remove exact duplicates
- merge bridge prose into one shorter runtime note when no behavior changes
- move support or provenance material out of the prompt and into the output json
- normalize repeated headings into canonical prompt sections
- rewrite runtime-called templates into compact embedded call contracts when placeholders, task instructions, rubrics, and output expectations remain intact
- rewrite shared algorithms into concise embedded rules when branch behavior, outputs, and failure meaning remain intact
- rewrite deterministic mappings and field-projection rules into shorter explicit tables or bullets when source-to-target relationships remain fully recoverable
- rewrite fallback cascades into shorter ordered bullets when branch order, defaults, and null-versus-present behavior remain explicit
- compact runtime assets only when the emitted representation preserves the asset's selection and decision semantics

## Prohibited transformations

The compiler must not:
- invent a trigger, capability, or failure rule
- convert optional behavior into required behavior
- convert guidance into authority
- weaken or strengthen precedence
- delete a unique runtime-critical constraint
- flatten schemas in a way that changes validation semantics
- leave a runtime-called template, runtime asset, or shared algorithm as an unresolved external dependency
- replace a runtime-critical template or asset with only a file-path mention
- demote a runtime-critical template, asset, or algorithm to `runtime_support`
- reduce a local output or gate schema to a loose essentials-only summary when important constraints would be lost
- collapse a deterministic mapping, fallback cascade, selection heuristic, score band, or derived-field rule into a vague summary that hides branch order or default behavior
- drop verification side effects or algorithm-emitted fields that affect later matching, evidence promotion, gating, or downstream assembly

## Execution-detail preservation rule

If the source defines any of the following, preserve them explicitly in the normalized prompt rather than alluding to them:
- source-to-target field mappings
- whitelist-to-field projection tables
- ordered fallback logic
- ranking or selection heuristics
- score bands or threshold interpretation text
- derived-field ownership, overwrite, or recomputation rules
- verification outputs such as offsets, ambiguity flags, match methods, or verification levels

Statements such as `generate usage_focus`, `apply whitelist`, or `run matching` are not sufficient when the source also defines how those results are derived.

## Conservative compaction rule

If a compaction decision depends on uncertain semantic equivalence, keep the richer source meaning or block.
Do not guess that two different phrasings are equivalent when the distinction might matter.

## Runtime closure rule

The normalized prompt must be self-sufficient for execution.
If the prompt still instructs the runtime to consult another source-side file for behavior that matters at execution time, the artifact is not closed.

When closure would remain open, the compiler must:
1. embed the required semantics into the prompt, or
2. block the compilation

## Modality preservation rule

Preserve words such as:
- `must`
- `must not`
- `should`
- `may`
- `block`
- `warn`

When the source uses weaker or stronger language, the normalized prompt must keep that strength.
