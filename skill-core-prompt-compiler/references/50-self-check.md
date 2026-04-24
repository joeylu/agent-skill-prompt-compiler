# Self-Check

Before returning, verify:
- exactly one source skill bundle was targeted
- the authoritative entry surface was resolved correctly
- the selected discovery path was reported
- runtime-critical triggers, inputs, outputs, workflow rules, boundaries, failures, and schemas were preserved
- runtime closure was achieved or the result is blocked
- no workflow step still depends on opening a source-side `references/` or `_shared/` file at runtime
- runtime-called templates were embedded as call contracts
- runtime assets and shared algorithms that affect execution were embedded or faithfully rewritten
- deterministic mappings, projection tables, and derived-field rules needed at runtime remain explicit
- ordered fallback and selection logic remain explicit
- score bands, threshold interpretations, and rubric text inside runtime-called templates remain explicit
- no exact duplicate unit appears twice in the prompt
- omitted support material is recorded in the output json
- example handling follows the `2000` token threshold rule
- schema flattening, if used, is explained in diagnostics
- the output json does not duplicate the prompt body
