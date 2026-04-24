---
name: skill-core-validator-prompt-compiler
description: Compile one validator skill bundle and one target skill bundle into one semantic-faithful target-specialized validator prompt artifact plus one diagnostic output json. Use when a validator must be transported or executed as a closed prompt bound to one concrete target skill, and a generic single-skill compiler would leave target-specific validation contracts open.
---

# Purpose

Use this skill to compile one validator-skill pair into:
1. one semantic-faithful target-specialized validator prompt artifact
2. one formal output object that contains the emitted prompt artifact envelope plus diagnostics

This compiler preserves behavior and boundary meaning, not original file shape.
It may reorder, deduplicate, flatten, or compact source material when that reduces token cost without weakening the pair contract.
The emitted prompt artifact is the primary runtime deliverable.
Diagnostics may explain prompt coverage, but diagnostics never substitute for missing runtime rules in the prompt body.

# Invocation Policy

Explicit-only.

Use this skill only when the user or workflow explicitly asks to compile, export, normalize, or transport:
- one validator skill
- plus one concrete target skill

# Non-goals

Do not execute the validator skill.
Do not execute the target skill.
Do not compile a generic validator prompt that still depends on an unspecified future target.
Do not merge multiple validator skills into one output artifact.
Do not merge multiple target skills into one output artifact.
Do not invent missing validator contracts, target contracts, schemas, or examples.
Do not silently strengthen or weaken modality such as `must`, `may`, `should`, `block`, or `warn`.
Do not optimize for compactness when semantic fidelity would drop.

# Process Overview

1. Resolve the authoritative validator entry surface.
2. Resolve the authoritative target entry surface.
3. Resolve the validator compiler-facing contract.
4. Resolve the authoritative target validator-facing contract.
5. Select strict whitelist discovery or fallback reference-chain discovery for each bundle.
6. Collect allowed validator and target files and classify their source roles.
7. Extract semantic units from the collected pair bundle.
8. Elevate branch-critical and branch-shaping units into explicit runtime-critical invariants.
9. Verify that every validator-required target role is provided by the target contract.
10. Detect runtime-closure dependencies across the validator-target pair:
   - runtime-called prompt templates
   - runtime-loaded assets
   - shared algorithms or gate rules referenced by workflow
   - target-specific schemas, boundaries, and validation materials declared by the compiler-facing contracts
11. Normalize the units into a target-specialized canonical structure.
12. Materialize each runtime-critical invariant into one explicit prompt-body landing point instead of leaving it as class-level or topic-level prose.
13. Close the artifact by embedding or faithfully rewriting every validator-local and target-specific dependency that would otherwise remain external.
14. Compact safely:
   - remove exact duplicates
   - move provenance and authoring-only support material to the output json
   - flatten or slice schemas only when validation semantics are preserved
   - keep all examples when compact enough, otherwise keep a minimal discriminative example set
15. Assemble one normalized prompt artifact.
16. Emit one formal output object with emitted artifacts, pair coverage, normalization decisions, closure status, and fidelity notes.

# Normalization Posture

Prefer semantic fidelity over source-shape fidelity.
Prefer a shorter target-specialized validator prompt over a verbose one only when the shorter form preserves:
- validator trigger conditions
- validator inputs and outputs
- validator workflow obligations
- multi-surface consistency relations across runtime inputs, packets, candidates, snapshots, or other authoritative surfaces
- target role bindings and target-specific closure material
- runtime call contracts
- runtime assets
- shared algorithms and gate logic
- deterministic mappings and field-projection rules
- fallback and derivation cascades
- selection heuristics that affect emitted fields
- selector, identity, uniqueness, ordering, and cardinality rules that affect target choice or audit scope
- score bands, threshold rubrics, and pass/fail interpretation scales
- branch-sensitive gate and output-branch rules
- hard boundaries
- authority boundaries between primary inputs, support-only context, and non-authority signals
- localization granularity and target-selection rules
- modality and quantifier semantics such as `exactly`, `only`, `same`, `non-null`, `at most`, and `unique`
- precedence rules
- failure conditions
- schema constraints
- discriminative example coverage
- runtime closure

Behavioral equivalence matters more than mention coverage.
Do not treat a rule as preserved just because related words appear in the prompt.
The emitted artifact must keep the same operational branch surface and obligation surface for the same class of inputs, or block.
If the prompt body and the output diagnostics disagree, trust the prompt body and treat the pair as not preserved.

When uncertain whether a compaction step is lossless, keep the richer form or block.

# Boundary Rules

Preserve source meaning, rule strength, precedence, and failure surface.
The runtime prompt may change section order, heading style, and grouping.
Source provenance belongs in the output json by default, not in the runtime prompt.
The runtime prompt must be operationally closed: it must not require the consumer to look up another source file in order to execute the validator correctly for the chosen target skill.
If a validator workflow step says to assemble, load, validate against, or otherwise consult a target-side source file, the compiler must embed the relevant target semantics into the prompt or block.
If the validator contract requires a target semantic role and the target contract does not provide it, block.
If the requested transport envelope cannot be emitted safely as a closed artifact, block.
Only the canonical compiler-facing contract files and the corresponding `# Reference Map` lines may be added for pair compilation.
Do not add other documents, examples, schemas, rules, or filler content just to make a missing role appear closed.
If existing authoritative runtime files plus those allowed contract surfaces do not close the pair safely, block.
If a template, pseudocode block, shared algorithm, or schema narrative defines a deterministic mapping, fallback cascade, selection heuristic, score band, or derived-field rule that affects runtime behavior, preserve that logic explicitly instead of replacing it with a loose summary.
If a source rule compares the same semantic value across multiple runtime surfaces, preserve every compared surface plus the comparison relation instead of collapsing the rule into one surface.
If selector semantics include uniqueness, positional alignment, same-run identity, or cardinality conditions, preserve those semantics explicitly instead of keeping only a subset.
Do not collapse a branch-sensitive nested schema into a looser checklist when the original schema decides executable validity, output shape, or output branch.
Do not satisfy a runtime-critical rule only in diagnostics or coverage notes when the emitted prompt body lacks that rule.
If a source rule can change auditability, output branching, target selection, or failure classification, the compiler must preserve it as one explicit prompt-body invariant or block.
Do not treat class-level summaries such as `check the brief`, `check the baseline`, or `validate the schema` as sufficient when the source defines multiple concrete surfaces, selectors, nested members, or quantifiers.
Schema flattening is allowed only when validation semantics are preserved.
Example compaction is allowed only when discriminative coverage is preserved.
If a source document is only authoring guidance, compatibility bridge text, or provenance metadata, it may move to the output json instead of the runtime prompt.
Compiler-facing validator contracts, target contracts, local output schemas, gate schemas, and runtime-called templates are not authoring-only material.

# Failure Rule

Block when the compiler cannot preserve semantic fidelity with confidence.
If the validator bundle or target bundle lacks enough material to reconstruct runtime-critical behavior, return blocking reasons instead of fabricating a compact prompt.
Block when runtime closure would otherwise remain open because a validator-local template, target-side schema, target-side validation rule, resource, shared algorithm, or schema contract cannot be embedded safely.
Block when the validator contract and target contract do not close to one safe pair specialization.
Block when pair closure would require adding any non-contract document or new runtime content beyond the corresponding `# Reference Map` lines.
Block when behavioral equivalence would be uncertain even if runtime closure looks closed on paper.

# Reference Map

- Scope and posture: `references/00-scope.md`
- Input contract: `references/10-input-contract.md`
- Output contract: `references/20-output-contract.md`
- Prompt template: `references/21-output-template.md`
- Discovery rules: `references/30-discovery-rules.md`
- Source role rules: `references/31-source-role-rules.md`
- Semantic unit model: `references/32-semantic-unit-model.md`
- Normalization rules: `references/33-normalization-rules.md`
- Schema treatment rules: `references/34-schema-treatment-rules.md`
- Example treatment rules: `references/35-example-treatment-rules.md`
- Provenance rules: `references/36-provenance-rules.md`
- Output assembly rules: `references/37-output-assembly-rules.md`
- Token estimation rules: `references/38-token-estimation-rules.md`
- Fidelity rules: `references/39-fidelity-rules.md`
- Failure rules: `references/40-failure-rules.md`
- Validator skill compiler contract spec: `references/60-validator-skill-contract.md`
- Target skill compiler contract spec: `references/61-target-skill-contract.md`
- Contract authoring guide: `references/62-contract-authoring-guide.md`
- Self-check: `references/50-self-check.md`
- Input schema: `schemas/compiler-input.schema.json`
- Output schema: `schemas/compiler-output.schema.json`
- Prompt artifact schema: `schemas/prompt-artifact.schema.json`
- Validator contract schema: `schemas/validator-compiler-contract.schema.json`
- Target contract schema: `schemas/target-validator-contract.schema.json`
- Example input: `examples/input-minimal.json`
- Example ready output: `examples/output-ready.json`
- Example blocked output: `examples/output-blocked.json`
- Normalized artifact example: `examples/normalized-artifact-example.md`
