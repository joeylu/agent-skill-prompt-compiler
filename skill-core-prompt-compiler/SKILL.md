---
name: skill-core-prompt-compiler
description: Compile one explicit source skill bundle into one semantic-faithful normalized prompt artifact plus one diagnostic output json. Use when a website, external LLM surface, or constrained runtime cannot accept a whole skill folder, and the caller still needs near-original execution quality with lower token cost than file-level transport.
---

# Purpose

Use this skill to compile one source skill bundle into:
1. one semantic-faithful normalized prompt artifact for runtime injection
2. one diagnostic output json for provenance, coverage, and persistence

This compiler preserves behavior and boundary meaning, not original file shape.
It may reorder, deduplicate, flatten, or compact source material when that reduces token cost without weakening the source contract.

# Invocation Policy

Explicit-only.

Use this skill only when the user or workflow explicitly asks to compile, export, normalize, or transport one skill into a prompt artifact.

# Non-goals

Do not execute the source skill.
Do not judge whether the source skill is good or bad.
Do not merge multiple source skills into one output artifact.
Do not invent missing rules, schemas, or examples.
Do not silently strengthen or weaken modality such as `must`, `may`, `should`, `block`, or `warn`.
Do not optimize for compactness when semantic fidelity would drop.

# Process Overview

1. Resolve one authoritative entry surface.
2. Select strict whitelist discovery or fallback reference-chain discovery.
3. Collect allowed source files and classify their source roles.
4. Extract semantic units from the collected bundle.
5. Assign each unit a criticality:
   - `runtime_critical`
   - `runtime_support`
   - `provenance_only`
6. Detect runtime-closure dependencies:
   - runtime-called prompt templates
   - runtime-loaded assets
   - shared algorithms or gate rules referenced by workflow
7. Normalize the units into a runtime-oriented canonical structure.
8. Close the runtime artifact by embedding or faithfully rewriting every runtime dependency that would otherwise remain external.
9. Compact safely:
   - remove exact duplicates
   - move provenance and authoring-only support material to the output json
   - flatten or slice schemas only when validation semantics are preserved
   - keep all examples when compact enough, otherwise keep a minimal discriminative example set
10. Assemble one normalized prompt artifact.
11. Emit one diagnostic output json with coverage, normalization decisions, closure status, and fidelity notes.

# Normalization Posture

Prefer semantic fidelity over source-shape fidelity.
Prefer a shorter runtime prompt over a verbose one only when the shorter form preserves:
- trigger conditions
- inputs and outputs
- workflow obligations
- runtime call contracts
- runtime assets
- shared algorithms and gate logic
- deterministic mappings and field-projection rules
- fallback and derivation cascades
- selection heuristics that affect emitted fields
- score bands, threshold rubrics, and pass/fail interpretation scales
- hard boundaries
- precedence rules
- failure conditions
- schema constraints
- discriminative example coverage
- runtime closure

When uncertain whether a compaction step is lossless, keep the richer form or block.

# Boundary Rules

Preserve source meaning, rule strength, precedence, and failure surface.
The runtime prompt may change section order, heading style, and grouping.
Source provenance belongs in the output json by default, not in the runtime prompt.
The runtime prompt must be operationally closed: it must not require the consumer to look up another source file in order to execute the skill correctly.
If a workflow step says to assemble, load, validate against, or otherwise consult a source file at runtime, the compiler must embed the relevant semantics into the prompt or block.
If a template, pseudocode block, shared algorithm, or schema narrative defines a deterministic mapping, fallback cascade, selection heuristic, score band, or derived-field rule that affects runtime behavior, preserve that logic explicitly instead of replacing it with a loose summary.
Schema flattening is allowed only when validation semantics are preserved.
Example compaction is allowed only when discriminative coverage is preserved.
If a source document is only authoring guidance, compatibility bridge text, or provenance metadata, it may move to the output json instead of the runtime prompt.
Local output schemas, gate schemas, and runtime-called templates are not authoring-only material.

# Failure Rule

Block when the compiler cannot preserve semantic fidelity with confidence.
If the bundle lacks enough material to reconstruct runtime-critical behavior, return blocking reasons instead of fabricating a compact prompt.
Block when runtime closure would otherwise remain open because a template, resource, shared algorithm, or schema contract cannot be embedded safely.

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
- Self-check: `references/50-self-check.md`
- Input schema: `schemas/compiler-input.schema.json`
- Output schema: `schemas/compiler-output.schema.json`
- Example input: `examples/input-minimal.json`
- Example ready output: `examples/output-ready.json`
- Example blocked output: `examples/output-blocked.json`
- Normalized artifact example: `examples/normalized-artifact-example.md`
