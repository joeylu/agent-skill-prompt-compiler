# Source Role Rules

Classify each collected file into one final source role.
The source role helps diagnostics and normalization.

## Classification priority

Apply rules in this order:

1. Path-based rules
   - `SKILL.md` is `entry`
   - `*.schema.json` is `schema`
   - `*.definition.json` is `definition`
   - files under `references/` are `reference`
   - files under `examples/` are `example`
   - files under `_shared/` are `shared_context` unless they are `*.schema.json`

2. Filename heuristics
   - names containing `rule`, `constraint`, `policy`, or `check` are `rule`
   - names containing `example` are `example`

3. Fallback
   - classify as `reference`

## Notes

- A shared schema is still `schema`, not `shared_context`.
- Source roles do not decide prompt inclusion by themselves.
- Prompt inclusion depends on semantic-unit criticality, not just file role.
- A file under `references/` or `_shared/` may still be runtime-critical when workflow steps depend on it at execution time.
