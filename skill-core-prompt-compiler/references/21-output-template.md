# Prompt Template

The normalized runtime prompt should follow this structure.
Section names are canonical, but internal wording may be compacted.

## Template

```text
<skill>
<identity>
Name: {skill_name}
Description: {skill_description}

Triggers:
- ...

Primary purpose:
- ...

Non-goals:
- ...
</identity>

<runtime_contract>
Inputs:
- ...

Outputs:
- ...

Preconditions:
- ...
</runtime_contract>

<workflow>
1. ...
2. ...
</workflow>

<runtime_materials>
## LLM call contracts
- Call contract name:
  - placeholders:
  - instructions:
  - output expectations:

## Embedded resources
- ...

## Shared algorithms
- ...
</runtime_materials>

<constraints>
Hard rules:
- ...

Precedence:
- ...

Failure conditions:
- ...
</constraints>

<schemas>
## {schema_name}
~~~json
...
~~~
</schemas>

<examples>
### {example_name}
Input signals:
- ...

Expected behavior:
- ...

Why it matters:
- ...
</examples>
</skill>
```

## Section omission rule

If a section has no runtime-critical material, omit the section.
Do not emit empty wrapper sections.
If runtime closure depends on a template, resource, or shared algorithm, emit a non-empty `runtime_materials` section.

## Provenance rule

Do not place source file paths, source annotations, or discovery diagnostics in the prompt body.
Those belong in the output json.

## Canonical wording rule

Prefer short atomic statements.
One behavioral rule should normally become one bullet.
Preserve modality words exactly.
Do not leave behind workflow instructions that still require opening a file such as `references/prompt-template.md` at runtime.
