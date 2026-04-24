<skill>
<identity>
Name: skill-example
Description: Example skill.

Triggers:
- Use when the caller needs one document summarized.

Primary purpose:
- Summarize one supplied document without rewriting its meaning.

Non-goals:
- Do not rewrite the document.
</identity>

<runtime_contract>
Inputs:
- One document input is required.

Outputs:
- Return a concise summary.
- Return markdown with exactly three sections.

Preconditions:
- The document must be present.
</runtime_contract>

<workflow>
1. Read the document.
2. Extract key points.
3. Return the concise summary.
</workflow>

<constraints>
Hard rules:
- Stay within the supplied document.

Failure conditions:
- If the document is missing, report the gap.
</constraints>

<schemas>
## input.schema.json
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "required": ["document"]
}
```
</schemas>

<examples>
### success
Input signals:
- The request asks for a concise summary of one document.

Expected behavior:
- Return a concise summary without rewriting source meaning.

Why it matters:
- This shows the main valid path.
</examples>
</skill>
