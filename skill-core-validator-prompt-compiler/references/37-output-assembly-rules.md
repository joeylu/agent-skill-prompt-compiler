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

Build this canonical prompt body first before applying any transport envelope.

## Single-placement rule

A semantic unit may appear only once in the runtime prompt.
If two source statements are exact duplicates, keep one canonical copy and record the deduplication in diagnostics.

## Atomic statement rule

Prefer one behavioral rule per bullet.
Prefer one workflow action per numbered step.

## Prompt-primary rule

Every runtime-critical semantic unit must appear in the canonical prompt body.
The output json may reference the unit for diagnostics, but it must not be the only place where the unit exists.
If the source pair contains multiple distinct runtime-critical invariants, preserve them as distinct prompt-body rules instead of one generalized summary.

## Identity rule

Always name:
- the validator skill
- the target skill

Do not present the artifact as a generic validator prompt.

## Transport envelope rule

After the canonical prompt body is assembled:

- if `transportMode = single_prompt`, emit `artifacts.singlePrompt.content` as the full canonical body
- if `transportMode = chat_roles`, emit `artifacts.chatRoles.messages` using the rules below
- if `transportMode = both`, emit both the single-prompt artifact and the chat-roles artifact from the same canonical body

## Chat-roles mapping rule

When emitting `chat_roles`:

1. If `targetRuntime.supportsSystemRole = false` and `targetRuntime.supportsDeveloperRole = true`, emit one `developer` message containing the full canonical body.
2. If `targetRuntime.supportsSystemRole = true` and `targetRuntime.supportsDeveloperRole = false`, emit one `system` message containing the full canonical body.
3. If both are `true`, or if `targetRuntime` is omitted, emit:
   - one `system` message containing `identity` + `constraints`
   - one `developer` message containing `runtime_contract` + `workflow` + `runtime_materials` + `schemas` + `examples`
4. If neither role is available and `chat_roles` was requested, block.

## Runtime materials rule

Use `runtime_materials` only when needed for closure.
Place embedded call contracts, embedded assets, target-role bindings, and shared algorithms there instead of leaving file-path lookups in workflow steps.

## Runtime-contract placement rule

Place auditable-versus-invalid gates, compared runtime surfaces, explicit selector bindings, and exact output-branch requirements in `runtime_contract`, `workflow`, `constraints`, or `schemas`.
Do not leave these rules implied only by provenance notes or diagnostic coverage.
Place non-schema runtime-critical invariants in a dedicated `Runtime-critical invariants` subsection inside `runtime_contract`.
Each invariant bullet must name the concrete surfaces, selectors, containers, quantifiers, or failure meaning that make the rule branch-critical.
Topic-level statements such as `check alignment`, `validate baseline`, or `preserve authority` do not count as invariant placement when the source defines more specific rules underneath.

## Schema placement rule

When nested schema constraints decide auditability, output branching, target selection, or item-versus-field validity, place those nested constraints in `schemas` or as equivalent explicit rules in `runtime_contract` or `constraints`.
Do not compress them into field-name-only bullets.

## No source-tag rule

Do not emit file-origin tags such as `--- [Source: ...] ---` in the runtime prompt.

## Persisted prompt rule

When compiler results are written to disk:
- derive `<normalized-validator-name>` by removing a leading `skill-` prefix when present
- derive `<normalized-target-name>` by removing a leading `skill-` prefix when present
- if `single_prompt` was emitted, write it to `prompt-<normalized-validator-name>--for--<normalized-target-name>.md`
- if `chat_roles` was emitted, write it to `prompt-<normalized-validator-name>--for--<normalized-target-name>.chat.json`
- write the full formal output object to `output-<normalized-validator-name>--for--<normalized-target-name>.json`
