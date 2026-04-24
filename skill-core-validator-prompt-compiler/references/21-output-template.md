# Prompt Template

The normalized runtime prompt should follow this structure.
Section names are canonical, but internal wording may be compacted.
This template defines the canonical prompt body used by `single_prompt`, and the source body that may be split into `chat_roles`.

## Template

```text
<skill>
<identity>
Validator skill: {validator_skill_name}
Target skill: {target_skill_name}
Description: {compiled_pair_description}

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

Runtime-critical invariants:
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

## Embedded target contract
- ...

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
If runtime closure depends on a template, resource, shared algorithm, or target-side contract surface, emit a non-empty `runtime_materials` section.

## Prompt-body coverage rule

If the source pair defines any of the following, show them explicitly in the prompt body:
- cross-surface equality or consistency rules
- selector identity, uniqueness, or once-only cardinality rules
- branch-sensitive nested schema requirements
- primary-authority versus support-only boundaries

Do not rely on the output json to carry those runtime rules.
If more than one such rule exists, preserve them as separate explicit bullets rather than one umbrella sentence.

## Provenance rule

Do not place source file paths, source annotations, or discovery diagnostics in the prompt body.
Those belong in the output json.

## Canonical wording rule

Prefer short atomic statements.
One behavioral rule should normally become one bullet.
Preserve modality words exactly.
Do not leave behind workflow instructions that still require opening a file such as `references/prompt-template.md`, `contracts/validator-compiler.contract.json`, or `contracts/target-validator.contract.json` at runtime.
Do not compress `exactly`, `only`, `same`, `non-null`, `at most`, `unique`, or `once and only once` into weaker wording.
In the `Runtime-critical invariants` subsection, each bullet should name the concrete surfaces, selectors, containers, nested members, and quantifiers that make the rule behaviorally binding.
