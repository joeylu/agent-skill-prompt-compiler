# Semantic Unit Model

The compiler does not optimize whole files directly.
It first extracts semantic units from the collected pair bundle.

## Unit types

Use these unit types when applicable:
- `trigger`
- `purpose`
- `non_goal`
- `input_requirement`
- `output_requirement`
- `precondition`
- `runtime_invariant`
- `workflow_step`
- `multi_surface_consistency_rule`
- `validator_target_role_requirement`
- `target_role_binding`
- `pair_closure_rule`
- `llm_call_contract`
- `template_placeholder`
- `decision_rubric`
- `score_band`
- `hard_rule`
- `precedence_rule`
- `failure_condition`
- `authority_boundary_rule`
- `selector_identity_rule`
- `cardinality_rule`
- `localization_rule`
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
- Extract compiler-facing validator requirements and target role bindings as distinct units.
- Extract runtime template behavior separately from the workflow step that invokes it.
- Extract placeholder requirements, scoring rubrics, output-shape instructions, and post-processing obligations as distinct units when they affect execution.
- Extract deterministic mappings, field-projection tables, ordered fallback cascades, and selection heuristics as distinct units when they affect runtime outputs or gating.
- Extract labeled score bands and threshold-interpretation text as distinct units when they calibrate pass/fail, branching, or revision behavior.
- Extract derived-field generation and authoritative field-ownership rules as distinct units when validator logic recomputes, overwrites, or backfills fields.
- Extract any rule that compares two or more runtime surfaces as one explicit `multi_surface_consistency_rule` that keeps every compared surface and the comparison relation.
- Extract selector equality, uniqueness, once-only cardinality, order alignment, and non-null target-choice rules as explicit selector or cardinality units instead of burying them inside broader prose.
- Extract authority-boundary statements such as "only authority", "support-only", "read-only support", or "not an authority channel" as explicit `authority_boundary_rule` units.
- Extract localization-granularity rules such as field-level versus item-level targeting as explicit `localization_rule` units when they affect outputs or evidence localization.
- When a nested schema decides auditability or output branching, extract the nested branch-sensitive member constraints as standalone schema units rather than only the parent object shell.
- If schema `description` text carries normative behavior, extract that behavior into a separate unit instead of treating it as decorative commentary.
- When a rule can change auditability, target selection, failure classification, or output branching, also extract it as one standalone `runtime_invariant` even if it already fits another unit type.
- Keep one invariant per concrete rule. Do not merge distinct branch-critical rules when they differ in compared surfaces, selectors, quantifiers, required members, or failure consequences.

## Criticality upgrade rules

Upgrade a unit to `runtime_critical` when any of the following holds:
- a workflow step directly tells the runtime to assemble or use a template file
- a workflow step directly loads or consumes a local asset file
- a shared algorithm governs matching, validation, evidence promotion, retry, gating, or failure behavior
- a validator contract defines a required target role
- a target contract binds one required target role to concrete paths
- a schema is used for runtime validation or final gate decisions
- a prompt template contains a scoring rubric, branch condition, output cardinality, or other contract that would change model behavior if omitted
- a template, pseudocode block, or schema narrative defines a deterministic mapping, fallback cascade, score band, selection heuristic, or derived-field rule that changes emitted output
- a rule compares values across multiple runtime surfaces
- a rule defines selector identity, uniqueness, order alignment, or once-only cardinality
- a rule defines a primary-authority versus support-only boundary
- a rule defines field-level versus item-level localization semantics
- a rule can flip auditable versus invalid input, one output branch versus another, or one target item versus another

Do not classify these units as `runtime_support` merely because they originated in `references/`, `_shared/`, or `contracts/`.

## Unit splitting rule

Split long prose into smaller units when that improves faithful normalization.
Do not split a statement if doing so would lose condition binding or modality.
Do not merge multiple runtime-critical invariants back into one abstract sentence during extraction.

## Unit identity rule

In diagnostics, each extracted unit should remain traceable to one or more source file paths even when the prompt does not expose provenance.
