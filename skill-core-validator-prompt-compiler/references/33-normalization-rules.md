# Normalization Rules

## Goal

Transform collected validator-target source material into a smaller target-specialized validator prompt without losing semantic fidelity.

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
- rewrite compiler-facing target contracts into shorter embedded target-role tables when required paths, role meaning, and authority are fully preserved
- rewrite an explicit cross-surface comparison into shorter wording only when every compared surface, the comparison relation, and the failure consequence remain explicit in the prompt body
- rewrite nested schemas into shorter prompt-facing rules only when the shortened form still preserves branch-sensitive nested requirements instead of reducing them to parent-field presence
- rewrite one runtime-critical invariant into one shorter atomic prompt rule only when the concrete surfaces, selectors, quantifiers, required members, and failure meaning remain explicit

## Prohibited transformations

The compiler must not:
- invent a trigger, capability, or failure rule
- convert optional behavior into required behavior
- convert guidance into authority
- weaken or strengthen precedence
- delete a unique runtime-critical constraint
- infer a missing target role from general prose
- flatten schemas in a way that changes validation semantics
- leave a runtime-called template, runtime asset, shared algorithm, validator contract, or target role binding as an unresolved external dependency
- replace a runtime-critical template, target contract, or asset with only a file-path mention
- demote a runtime-critical template, asset, contract, or algorithm to `runtime_support`
- reduce a local output or gate schema to a loose essentials-only summary when important constraints would be lost
- collapse a deterministic mapping, fallback cascade, selection heuristic, score band, or derived-field rule into a vague summary that hides branch order or default behavior
- drop verification side effects or algorithm-emitted fields that affect later matching, evidence promotion, gating, or downstream assembly
- replace an exact comparison across multiple runtime surfaces with a one-surface summary
- replace explicit selector uniqueness or once-only cardinality with weaker words such as `coherent`, `aligned`, or `consistent`
- replace branch-sensitive nested constraints with only a parent-field existence statement
- satisfy a runtime-critical rule only in output diagnostics while omitting it from the prompt body
- claim semantic fidelity from a coverage note when the emitted prompt does not actually contain the corresponding runtime rule
- merge multiple runtime-critical invariants into one class-level or topic-level statement that hides which exact rule is being enforced
- replace a concrete surface, selector, aggregate container, required nested member, or quantifier with an abstract label that no longer preserves the original branch behavior

## Execution-detail preservation rule

If the source defines any of the following, preserve them explicitly in the normalized prompt rather than alluding to them:
- source-to-target field mappings
- whitelist-to-field projection tables
- ordered fallback logic
- ranking or selection heuristics
- score bands or threshold interpretation text
- derived-field ownership, overwrite, or recomputation rules
- validator-required target roles
- target-contract role bindings
- verification outputs such as offsets, ambiguity flags, match methods, or verification levels

Statements such as `load target rules`, `apply target contract`, or `run matching` are not sufficient when the source also defines how those results are derived.
Statements such as `compare the brief`, `check the baseline`, or `validate the candidate schema` are not sufficient when the source defines which surfaces are compared, which nested members are required, or which cardinality rule decides validity.
Statements such as `enforce selector consistency`, `keep prompt fidelity`, or `check packet alignment` are not sufficient when the source defines separate branch-critical invariants under those umbrellas.

## Conservative compaction rule

If a compaction decision depends on uncertain semantic equivalence, keep the richer source meaning or block.
Do not guess that two different phrasings are equivalent when the distinction might matter.

## Prompt-primary rule

The prompt body is the runtime artifact.
Diagnostics may describe why the prompt is correct, but diagnostics do not count as preserving runtime behavior by themselves.
If a rule is runtime-critical, it must land in the prompt body.
If multiple runtime-critical invariants exist, each invariant must land explicitly in the prompt body. Class-level coverage is not enough.

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
