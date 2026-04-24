# Discovery Rules

## Discovery path selection

Use exactly one discovery path per bundle.

For each bundle:
1. resolve the authoritative entry surface
2. attempt to parse the authoritative `# Reference Map`
3. if that map is present and parseable, use strict whitelist discovery for that bundle
4. if that map is missing or cannot be parsed safely, use fallback reference-chain discovery for that bundle

## Compiler-facing contract supplement

In addition to ordinary entry discovery, always collect:
- the resolved validator compiler-facing contract
- the resolved target validator-facing contract

After reading the target contract, also collect every explicit target path listed in `providedRoles[].items[].path`.
These paths are explicit role bindings, not directory discovery.

## Strict whitelist discovery

In strict whitelist discovery for one bundle, collect only:
1. the authoritative entry surface
2. paths listed in that bundle's authoritative `# Reference Map`
3. the resolved compiler-facing contract path for that bundle
4. for the target bundle only, the explicit role-bound paths declared by the target contract

Do not discover by directory scan.
Do not chase secondary references recursively unless the authoritative `# Reference Map` already lists them.

## Fallback reference-chain discovery

In fallback reference-chain discovery for one bundle, collect only:
1. the authoritative entry surface
2. the resolved compiler-facing contract path for that bundle
3. supplied text-bearing files that are explicitly path-referenced by already collected fallback files
4. for the target bundle only, the explicit role-bound paths declared by the target contract

Supported fallback references must be syntactic file-path literals that can be parsed safely.

## Missing file rule

If an allowed file is missing from either bundle:
- record the bundle
- record the path
- record its expected role when resolvable
- continue or block according to policy

## Reporting rule

Always report:
- the validator discovery path
- the target discovery path
- the validator `# Reference Map` status
- the target `# Reference Map` status
- the resolved contract paths
- files excluded by strict whitelist or fallback policy
