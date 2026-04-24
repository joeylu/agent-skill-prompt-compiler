# Scope

This skill compiles one explicit validator skill bundle plus one explicit target skill bundle into:
1. one target-specialized validator prompt artifact
2. one diagnostic output json

## Supported source material

Each bundle may contain text-bearing files such as:
- `SKILL.md`
- `contracts/*.json`
- `schemas/*.json`
- `references/*.md`
- `examples/*`
- `_shared/*` text files when explicitly discoverable
- other text-bearing files that are explicitly discoverable

## Semantic fidelity meaning

For this compiler, semantic fidelity means the normalized prompt preserves:
- when the validator skill should trigger
- what the validator is trying to accomplish
- what validator inputs and outputs matter
- what validator steps, gates, and dependencies govern execution
- what target semantic roles the validator requires
- what target-side schemas, boundaries, rule surfaces, and examples must be consulted for the chosen target
- what runtime templates, assets, and shared algorithms must be consulted during execution
- what boundaries and precedence rules limit execution
- what failures must block or warn
- what schema constraints shape valid payloads
- what example distinctions teach success, failure, or boundary behavior
- whether the compiled artifact is closed without external validator-folder or target-folder lookups

The normalized prompt does not need to preserve:
- original file grouping
- repeated bridge prose
- provenance annotations in the prompt body
- authoring-only or maintenance-only guidance that is not runtime-critical

## Default posture

Prefer one compact target-specialized validator prompt over a file-by-file transport artifact.
Prefer blocking over emitting a compact prompt with weakened pair semantics.
Prefer embedded target-specific validator contracts over a prompt that still points at external `references/`, `_shared/`, or contract files.
