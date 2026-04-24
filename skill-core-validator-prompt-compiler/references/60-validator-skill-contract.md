# Validator Skill Compiler Contract

This document defines how to write a validator skill so `skill-core-validator-prompt-compiler` can consume it safely.

## Canonical contract file

Preferred path:
- `contracts/validator-compiler.contract.json`

Preferred schema:
- `schemas/validator-compiler-contract.schema.json` from this compiler skill

The compiler may also accept an explicit path passed in input, but a validator skill should still keep one authoritative compiler-facing contract file.
Beyond that contract file and the corresponding `# Reference Map` lines, do not add new validator-side documents or runtime content just to satisfy pair compilation.

## What the validator must expose

A validator skill must expose:
- its ordinary `SKILL.md`
- one compiler-facing validator contract
- any validator-local schemas, rules, templates, and references needed for execution

The compiler-facing validator contract exists so the compiler does not have to guess what the validator needs from the target side.

## Required contract meaning

The validator contract must say, explicitly:
- that this validator is `target_specialized_only`
- which target semantic roles are required
- which target semantic roles are optional
- that missing required target roles must block pair compilation

## Standard target role vocabulary

Use these generic role keys:

| role key | meaning |
|---|---|
| `target_identity_contract` | target identity and ownership contract |
| `target_input_contract` | target-side input or precondition contract |
| `target_output_contract` | authoritative target output schema or equivalent output contract |
| `target_boundary_rules` | target boundary and jurisdiction rules |
| `target_validation_rules` | target-specific validator-facing rule prose |
| `target_validation_catalog` | target-specific machine-readable validation catalog |
| `target_examples` | target examples that are runtime-relevant or discriminative |
| `target_reference_context` | other target-side reference context that the validator truly needs |

Use only the roles the validator really needs.
If a role is not needed for correct execution, leave it out.

## Fail-fast rule

Do not write validator prose such as:
- "read whatever target schema seems relevant"
- "inspect the target folder for checks"
- "load the target rules if available"

Those phrases are not compiler-safe.
The validator contract must make the required target roles explicit.
If the validator cannot express its true pair requirements without inventing extra non-contract material, the correct result is to block and fix the underlying skill separately.

## Reference Map rule

The validator `SKILL.md` should reference the compiler-facing contract in its `# Reference Map`.
That keeps the contract discoverable even when strict whitelist discovery is used.
Those `# Reference Map` lines are the only allowed discoverability additions outside the canonical contract file itself.

## Example

A writing validator might require:
- `target_output_contract`
- `target_boundary_rules`
- `target_validation_rules`
- `target_validation_catalog`

That is only an example of the generic role vocabulary.
Those names are not writing-specific.
