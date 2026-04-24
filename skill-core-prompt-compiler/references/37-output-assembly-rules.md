# Output Assembly Rules

## Stable ordering rule

Assemble the normalized prompt in this order:
1. `identity`
2. `runtime_contract`
3. `workflow`
4. `runtime_materials`
5. `constraints`
6. `schemas`
7. `examples`

## Single-placement rule

A semantic unit may appear only once in the runtime prompt.
If two source statements are exact duplicates, keep one canonical copy and record the deduplication in diagnostics.

## Atomic statement rule

Prefer one behavioral rule per bullet.
Prefer one workflow action per numbered step.

## Runtime materials rule

Use `runtime_materials` only when needed for closure.
Place embedded call contracts, embedded assets, and shared algorithms there instead of leaving file-path lookups in workflow steps.

## No source-tag rule

Do not emit file-origin tags such as `--- [Source: ...] ---` in the runtime prompt.

## Persisted prompt rule

When compiler results are written to disk:
- derive `<normalized-skill-name>` by removing a leading `skill-` prefix when present
- write the runtime prompt to `prompt-<normalized-skill-name>.md`
- write the diagnostic summary to `output-<normalized-skill-name>.json`
- write the normalized runtime prompt into the prompt file
- write only the compilation summary into the output file
