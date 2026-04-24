# Semantic Unit Model

The compiler does not optimize whole files directly.
It first extracts semantic units from the collected bundle.

## Unit types

Use these unit types when applicable:
- `trigger`
- `purpose`
- `non_goal`
- `input_requirement`
- `output_requirement`
- `precondition`
- `workflow_step`
- `llm_call_contract`
- `template_placeholder`
- `decision_rubric`
- `score_band`
- `hard_rule`
- `precedence_rule`
- `failure_condition`
- `schema_constraint`
- `schema_narrative_constraint`
- `postprocess_contract`
- `shared_algorithm`
- `deterministic_mapping`
- `selection_heuristic`
- `fallback_rule`
- `derived_field_rule`
- `runtime_asset_item`
- `example_case`
- `support_note`
- `provenance_note`

## Criticality classes

Each unit must be assigned one criticality:

| Criticality | Meaning |
|---|---|
| `runtime_critical` | required to preserve runtime behavior or boundary meaning |
| `runtime_support` | helpful context that may be omitted from the prompt when compaction is needed |
| `provenance_only` | needed for traceability or diagnostics, not runtime execution |

## Unit extraction rules

- Preserve distinctions between required, optional, blocked, and warned behavior.
- Preserve distinctions between positive capability and negative boundary.
- Preserve step order when order affects execution.
- Keep precedence rules separate from ordinary hard rules.
- Keep schema constraints separate from prose guidance.
- Extract runtime template behavior separately from the workflow step that invokes it.
- Extract placeholder requirements, scoring rubrics, output-shape instructions, and post-processing obligations as distinct units when they affect execution.
- Extract deterministic mappings, field-projection tables, ordered fallback cascades, and selection heuristics as distinct units when they affect runtime outputs or gating.
- Extract labeled score bands and threshold-interpretation text as distinct units when they calibrate pass/fail, branching, or revision behavior.
- Extract derived-field generation and authoritative field-ownership rules as distinct units when orchestrator logic recomputes, overwrites, or backfills fields.
- If schema `description` text carries normative behavior, extract that behavior into a separate unit instead of treating it as decorative commentary.

## Criticality upgrade rules

Upgrade a unit to `runtime_critical` when any of the following holds:
- a workflow step directly tells the runtime to assemble or use a template file
- a workflow step directly loads or consumes a local asset file
- a shared algorithm governs matching, validation, evidence promotion, retry, gating, or failure behavior
- a shared algorithm records verification fields, ambiguity markers, offsets, or other outputs that later steps may consume or depend on
- a schema is used for runtime validation or final gate decisions
- a prompt template contains a scoring rubric, branch condition, output cardinality, or other contract that would change model behavior if omitted
- a template, pseudocode block, or schema narrative defines a deterministic mapping, fallback cascade, score band, selection heuristic, or derived-field rule that changes emitted output

Do not classify these units as `runtime_support` merely because they originated in `references/` or `_shared/`.

## Unit splitting rule

Split long prose into smaller units when that improves faithful normalization.
Do not split a statement if doing so would lose condition binding or modality.

## Unit identity rule

In diagnostics, each extracted unit should remain traceable to one or more source file paths even when the prompt does not expose provenance.
