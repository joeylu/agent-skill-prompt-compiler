# Output Contract

The output must match `schemas/compiler-output.schema.json`.

## Ready result

A ready result must contain:
- `status = ready`
- a `summary`
- `consumedFiles`
- a `coverageReport`
- a `normalizationReport`
- a `tokenReport`
- a `fidelityReport`
- `warnings` and `blockingReasons`

Its diagnostics must also make clear:
- whether runtime closure was achieved
- whether templates were embedded or still referenced
- whether runtime assets were embedded or still referenced
- whether shared algorithms were embedded or still referenced

## Blocked result

A blocked result must still contain:
- `status = blocked`
- the resolved or unresolved summary state
- `consumedFiles`
- a diagnostic `coverageReport`
- a diagnostic `normalizationReport`
- a diagnostic `tokenReport`
- a diagnostic `fidelityReport`
- concrete `blockingReasons`

## Prompt body rule

The prompt artifact is the runtime deliverable.
The output json must describe how that prompt was assembled, not duplicate the prompt body.
The output json must explicitly record any closure-sensitive material that was embedded, compacted, or blocked.

## Disk materialization rule

When the compiler result is written to disk for this workflow:
- derive `<normalized-skill-name>` by removing a leading `skill-` prefix when present
- emit exactly one prompt file named `prompt-<normalized-skill-name>.md`
- emit exactly one summary file named `output-<normalized-skill-name>.json`
- write the normalized runtime prompt into the prompt file
- write only the compilation summary into the output file
