# Provenance Rules

## Prompt posture

Do not emit provenance in the runtime prompt by default.
Runtime prompt tokens should be spent on behavior, not tracing metadata.

## Output json posture

Record provenance in the output json, including:
- consumed validator and target file paths
- discovery path for each bundle
- resolved validator contract path
- resolved target contract path
- omitted support material
- schema flattening decisions
- example compaction decisions
- runtime closure status
- template embedding decisions
- runtime asset embedding decisions
- shared algorithm embedding decisions
- target contract strategy

## Traceability rule

Even when the prompt is normalized, the output json must let a reviewer understand:
- what was included
- what was omitted from the prompt
- why the omission was considered safe
- how the validator-required target roles were closed

## No hidden omission rule

If support or provenance material is removed from the prompt, say so explicitly in the output json.
If runtime closure required embedding or blocking, say so explicitly in the output json.
