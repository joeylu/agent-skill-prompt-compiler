# Target Skill Compiler Contract

This document defines how a target skill should expose a validator-compiler-facing contract so `skill-core-validator-prompt-compiler` can consume it safely.

## Two valid modes

A target may support the compiler in either of these ways:

| mode | meaning |
|---|---|
| native contract | the target skill itself ships a compiler-facing target contract |
| adapter contract | the target skill stays as-is, and one canonical adapter contract maps generic target roles to existing concrete target files |

Both modes use the same schema shape.

## Canonical contract file

Preferred path:
- `contracts/target-validator.contract.json`

Preferred schema:
- `schemas/target-validator-contract.schema.json` from this compiler skill

If the target is using an adapter, that adapter file should still resolve to one authoritative target contract path.
That adapter contract is the only allowed added target-side file for pair compilation; aside from the corresponding `# Reference Map` lines, do not add other documents or runtime content to make missing roles appear closed.

## What the target contract must expose

The target contract must say, explicitly:
- whether it is a `native_skill_contract` or `adapter_contract`
- the authoritative target skill name
- which generic target roles are provided
- which concrete relative file paths close each role
- what kind of source each bound file is

## Path rule

Every bound path must be:
- explicit
- relative to the target bundle root
- file-level, not directory-level

Do not point the compiler at a whole folder and expect it to infer the right files.

## Native-vs-adapter guidance

Use a native contract when:
- the target skill already has stable validator-facing materials
- you can modify the target skill directly

Use an adapter contract when:
- the target skill already exists
- its current authoritative runtime materials already close the needed roles
- you only need an explicit mapping from generic target roles to those existing files

## Fail-fast rule

If the validator requires a target role and the target contract does not provide it, the compiler should block.
The target contract must not pretend a role exists when the actual bound file is missing.
If a required role cannot be closed by existing authoritative target files plus the canonical target contract surface, block.
Do not add new boundary docs, examples, schemas, or rule prose merely to satisfy pair compilation.

## Example mapping

One target family might map its local files like this:

| generic role | possible concrete file |
|---|---|
| `target_identity_contract` | `field.definition.json` |
| `target_output_contract` | `payload.schema.json` |
| `target_boundary_rules` | `references/20-boundary.md` |
| `target_validation_rules` | `validator-checks.md` |
| `target_validation_catalog` | `validator-checks.catalog.json` |

That mapping is only illustrative.
The compiler cares about the generic role semantics, not about one domain's local filenames.
The example assumes those files already exist and are already authoritative.
It does not authorize creating new non-contract files just to match the table.

## Long-term recommendation

If you are designing a brand-new target skill, prefer shipping the native target contract from the beginning.
If you are adapting an older target skill, bind one adapter contract to existing authoritative files.
If those files do not close the required roles, block instead of adding other documents.
