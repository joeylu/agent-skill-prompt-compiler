# Output Contract

The output must match `schemas/compiler-output.schema.json`.

## Ready result

A ready result must contain:
- `status = ready`
- `artifacts`
- a `summary`
- `consumedFiles`
- a `coverageReport`
- a `normalizationReport`
- a `tokenReport`
- a `fidelityReport`
- `warnings` and `blockingReasons`

Its diagnostics must also make clear:
- which validator skill was compiled
- which target skill it was specialized against
- whether runtime closure was achieved
- whether the target contract came from a native target contract or an adapter contract
- whether templates were embedded or still referenced
- whether runtime assets were embedded or still referenced
- whether shared algorithms were embedded or still referenced
- which critical rule classes were detected in the source pair
- where each critical rule class lands in the emitted runtime artifact
- whether each critical rule class was preserved explicitly, preserved by faithful rewrite, or caused blocking

`artifacts` must match `schemas/prompt-artifact.schema.json`.
`summary.requestedTransportMode` is the authoritative requested transport mode in the output json.
`artifacts.transportMode` is the authoritative emitted transport mode for ready results.
For ready results, those two values must match.
`schemas/compiler-output.schema.json` references `schemas/prompt-artifact.schema.json` by schema id, so validators must load both schemas into the same schema registry when checking instances.
`status = ready` is allowed only when closure is fully closed.
`normalizationReport.closureStatus` must therefore be `closed` for every ready result.

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

A blocked result must not contain emitted prompt artifacts.
If blocking happened because closure could not be closed, use `normalizationReport.closureStatus = blocked_open`.
If blocking happened before closure could even be evaluated, use `normalizationReport.closureStatus = not_evaluated`.

## Prompt body rule

The emitted prompt artifact is the runtime deliverable.
The formal output object must include both:
- the emitted artifact envelope
- the diagnostics that explain how it was assembled

The diagnostics must explicitly record any closure-sensitive material that was embedded, compacted, or blocked.
The diagnostics must also include `fidelityReport.criticalRuleCoverage`.
For ready results, no `criticalRuleCoverage` entry may remain in `blocked` state.

When strategy fields are emitted:
- `embedded_verbatim` means the runtime material was embedded without semantic rewriting
- `embedded_rewritten` means the runtime material was embedded as a semantic-faithful rewrite, such as a compact call contract

## Disk materialization rule

When the compiler result is written to disk for this workflow:
- derive `<normalized-validator-name>` by removing a leading `skill-` prefix when present
- derive `<normalized-target-name>` by removing a leading `skill-` prefix when present
- if `artifacts.transportMode` includes `single_prompt`, emit `prompt-<normalized-validator-name>--for--<normalized-target-name>.md`
- if `artifacts.transportMode` includes `chat_roles`, emit `prompt-<normalized-validator-name>--for--<normalized-target-name>.chat.json`
- emit exactly one output file named `output-<normalized-validator-name>--for--<normalized-target-name>.json`
- write the emitted artifact envelope into the prompt file or files
- write the full formal output object into the output file
