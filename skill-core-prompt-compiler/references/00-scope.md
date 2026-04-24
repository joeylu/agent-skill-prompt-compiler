# Scope

This skill compiles one explicit document-oriented source skill bundle into:
1. one normalized runtime prompt artifact
2. one diagnostic output json

## Supported source material

The source bundle may contain text-bearing files such as:
- `SKILL.md`
- `schemas/*.json`
- `references/*.md`
- `examples/*`
- `_shared/*` text files when explicitly discoverable
- other text-bearing files that are explicitly discoverable

## Semantic fidelity meaning

For this compiler, semantic fidelity means the normalized prompt preserves:
- when the source skill should trigger
- what the source skill is trying to accomplish
- what inputs and outputs matter
- what steps, gates, and dependencies govern execution
- what runtime templates, assets, and shared algorithms must be consulted during execution
- what boundaries and precedence rules limit execution
- what failures must block or warn
- what schema constraints shape valid payloads
- what example distinctions teach success, failure, or boundary behavior
- whether the compiled artifact is runtime-closed without external skill-folder lookups

The normalized prompt does not need to preserve:
- original file grouping
- repeated bridge prose
- provenance annotations in the prompt body
- authoring-only or maintenance-only guidance that is not runtime-critical

## Default posture

Prefer one compact runtime prompt over a file-by-file transport artifact.
Prefer blocking over emitting a compact prompt with weakened semantics.
Prefer an embedded runtime contract over a prompt that still points at external `references/` or `_shared/` files.
