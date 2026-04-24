# Contract Authoring Guide

This document defines how to author compiler-facing contracts for `skill-core-validator-prompt-compiler`.

It is intentionally generic.
It does not assume any one project, framework, validator family, or target-skill layout.

## Purpose

Use this guide when you are designing or retrofitting:
- one validator skill that will be pair-compiled against concrete targets
- one target skill that may be consumed by such a validator

This guide explains:
- what belongs in the validator contract
- what belongs in the target contract
- how to choose between native and adapter target contracts
- how to keep contract files discoverable without turning them into runtime-behavior duplicates

## Retrofit boundary

For this compiler workflow, the only allowed additions are:
- the canonical validator compiler-facing contract file
- the canonical target validator-facing contract file
- the corresponding `# Reference Map` lines when discoverability is needed

All other runtime materials must already exist and already be authoritative.
Do not create new non-contract documents, schemas, examples, rule prose, or filler content just to give the compiler something to bind.
If required roles still do not close through existing authoritative files plus those allowed contract surfaces, block.

## Core posture

The compiler-facing contracts are compile-time metadata.
They exist so the compiler can close one validator-target pair safely without guessing.

The contracts are not:
- a replacement for the ordinary `SKILL.md`
- a replacement for runtime schemas, rules, or examples
- a place to restate the whole skill behavior in different words

Keep them narrow.
They should declare pairing dependencies and file bindings, not duplicate the target or validator workflow.

## Authoritative file rule

Each bundle should expose one authoritative compiler-facing contract file.

Preferred paths:
- validator bundle: `contracts/validator-compiler.contract.json`
- target bundle: `contracts/target-validator.contract.json`

The compiler may also accept explicit contract paths passed in input.
That is an invocation convenience, not a reason to keep multiple competing contract files in one bundle.

## Validator contract authoring

The validator contract answers one question:

> What target-side semantic roles must be present so this validator can execute correctly after specialization?

Author it by working backward from actual validator execution behavior.

### Validator authoring steps

1. Read the validator workflow, schemas, local rules, templates, and runtime references.
2. List every target-side material the validator truly depends on.
3. Convert each dependency into the smallest matching generic target role.
4. Mark the role `required` only when missing that role would make safe validator execution impossible.
5. Mark the role `optional` only when the validator can still execute correctly without it.

### Required-versus-optional rule

Use `requiredTargetRoles` for target-side materials that affect:
- acceptance or rejection of input shape
- authoritative target output interpretation
- target boundary or jurisdiction
- target-specific validation logic
- any runtime branch that would become ambiguous without the target-side material

Use `optionalTargetRoles` only for materials that improve precision, examples, or clarity without changing whether the validator can run safely.

If you are unsure whether a role is required, prefer `required`.
Do not weaken runtime closure for compactness.

### Smallest-role rule

Request the narrowest role that closes the validator safely.

Good:
- request `target_validation_catalog` when the validator needs the target's machine-readable check map

Bad:
- request `target_reference_context` just because it is broad and convenient

Broad roles should not become catch-all escape hatches.

## Target contract authoring

The target contract answers one question:

> Which concrete target-side files satisfy the generic roles that validators may request?

It does not define new runtime behavior.
It maps already-authoritative target materials into the compiler's generic role vocabulary.

### Target authoring steps

1. Identify the target's real authoritative files.
2. Decide whether the target should publish a native contract or an adapter contract.
3. Bind each provided generic role to one or more explicit file paths.
4. Use the smallest honest set of roles that the target can actually support.
5. Omit roles that the target does not truly provide.

### Native-versus-adapter rule

Use `native_skill_contract` when the target skill itself is being authored or maintained with validator compilation in mind.

Use `adapter_contract` when:
- the target skill already exists
- its current runtime materials are good enough
- you only need a mapping layer from generic roles to existing files

An adapter contract should not rewrite the target architecture.
It should expose the target as it already exists.
It must not be used as justification to create extra non-contract files for missing roles.

## Role-binding rule

Every provided role must bind to explicit file items.

Each item should be:
- real
- relative to the target bundle root
- file-level
- typed with the most accurate `sourceKind`

Bind multiple files to one role only when all of them are genuinely needed to close that role.

Examples:
- one rule prose file plus one machine catalog may close two different roles
- one role may bind to several examples when discriminative example coverage really matters

Those examples assume the bound files already exist and are already authoritative.
They are not permission to create new non-contract files for missing roles.

Do not bind a whole directory.
Do not rely on the compiler to infer the right file from a folder.

## Reference Map guidance

For discoverability, the validator `SKILL.md` should reference its validator contract in `# Reference Map`.
The target `SKILL.md` should reference its target contract in `# Reference Map`.

This is recommended because strict whitelist discovery reads the authoritative `# Reference Map`.
These `# Reference Map` lines are the only allowed discoverability additions outside the canonical contract files themselves.

At the same time, the contracts should remain metadata-oriented.
Do not turn the contract files into alternate copies of runtime rules just because ordinary prompt compilers may also collect referenced files.

## Explicit-path guidance

Passing `validatorContractPath` or `targetContractPath` explicitly in compiler input is valid.

Use explicit paths when:
- you need temporary migration support
- you are validating a draft contract that is not yet in the canonical path
- you intentionally do not want the ordinary skill `# Reference Map` to expose the contract yet

Do not use explicit paths as a long-term substitute for maintaining one authoritative contract file.

## No-duplication rule

Do not copy large sections of runtime behavior into the contract files.

The contracts should not become:
- alternate `SKILL.md` files
- mini target specs
- mini validator specs

Instead:
- validator contract declares target-role needs
- target contract declares role-to-file bindings
- ordinary runtime files continue to own runtime behavior

## Safe contract content

Good contract content:
- required role declarations
- optional role declarations
- role binding lists
- short role descriptions
- short notes that clarify why a role is needed or how a role is closed

Bad contract content:
- vague instructions such as "read whatever seems relevant"
- directory-level discovery hints
- free-form fallback advice that asks the compiler to guess
- restated runtime behavior that disagrees with the source files
- project-specific assumptions disguised as generic compiler rules
- newly created non-contract documents whose only purpose is to satisfy a missing role binding

## Failure alignment rule

Contracts must align with fail-fast compilation.

That means:
- validators must not request hidden or inferred target materials
- targets must not claim roles that are not actually backed by files
- callers must not expect the compiler to reconstruct missing pair contracts from prose
- callers must not create extra non-contract materials to avoid a block

If the pair cannot close through existing authoritative files plus the canonical contract files and their corresponding `# Reference Map` lines, the correct result is to block.

## Minimal authoring checklist

Before treating a validator-target pair as compiler-ready, check:

1. The validator has one authoritative compiler-facing contract.
2. The target has one authoritative native or adapter contract.
3. Every required validator target role is explicit.
4. Every provided target role is bound to real file paths.
5. No role is present only because it "might be useful later".
6. The contracts do not duplicate the full runtime skill behavior.
7. The contracts can be discovered either by canonical path or explicit input path.
8. Missing required roles would correctly block pair compilation.
9. No extra non-contract files were created just to make a missing role appear closed.
