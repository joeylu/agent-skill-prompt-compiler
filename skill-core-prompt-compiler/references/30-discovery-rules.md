# Discovery Rules

## Discovery path selection

Use exactly one discovery path per compilation.

1. Resolve the authoritative entry surface.
2. Attempt to parse the authoritative `# Reference Map`.
3. If that map is present and parseable, use strict whitelist discovery.
4. If that map is missing or cannot be parsed safely, use fallback reference-chain discovery.

## Strict whitelist discovery

In strict whitelist discovery, collect only:
1. the authoritative entry surface
2. paths listed in the authoritative `# Reference Map`

Do not discover by directory scan.
Do not chase secondary references recursively unless the authoritative `# Reference Map` already lists them.

## Fallback reference-chain discovery

In fallback reference-chain discovery, collect only:
1. the authoritative entry surface
2. supplied text-bearing files that are explicitly path-referenced by already collected fallback files

Supported fallback references must be syntactic file-path literals that can be parsed safely.

## Missing file rule

If an allowed file is missing from the supplied bundle:
- record the path
- record its expected role when resolvable
- continue or block according to policy

## Reporting rule

Always report:
- the selected discovery path
- the `# Reference Map` status
- files excluded by strict whitelist or fallback policy
