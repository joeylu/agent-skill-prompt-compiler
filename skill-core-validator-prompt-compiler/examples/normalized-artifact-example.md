<skill>
<identity>
Validator skill: skill-example-validator
Target skill: skill-example-target
Description: Example target-specialized validator prompt.

Triggers:
- Use when the caller needs the example validator bound to the example target skill.

Primary purpose:
- Validate one example-target output against the example validator rules.

Non-goals:
- Do not validate a different target skill.
- Do not rewrite the target output.
</identity>

<runtime_contract>
Inputs:
- One validator-ready payload for the example target is required.

Outputs:
- Return one validator report packet.

Preconditions:
- The chosen target must be `skill-example-target`.
</runtime_contract>

<workflow>
1. Read the validator-ready payload.
2. Apply the embedded target boundary and output-contract rules.
3. Return the validator report.
</workflow>

<runtime_materials>
## Embedded target contract
- The target output must be an object with required field `result`.
- Stay within the target field boundary.
</runtime_materials>

<constraints>
Hard rules:
- Stay within the chosen target contract.

Failure conditions:
- If the input does not match the example target contract, block.
</constraints>

<schemas>
## output.schema.json
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "required": ["result"]
}
```
</schemas>

<examples>
### success
Input signals:
- The request asks for the example validator bound to the example target.

Expected behavior:
- Return a validator report that uses the embedded example-target contract.

Why it matters:
- This shows the main valid pair-specialization path.
</examples>
</skill>
