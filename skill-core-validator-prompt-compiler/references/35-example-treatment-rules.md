# Example Treatment Rules

## Default threshold

If the estimated total example cost is `<= 2000` tokens, preserve all examples.
If the estimated total example cost is `> 2000` tokens, compile a minimal discriminative example set.

## Minimal discriminative example set

Retain the smallest set of examples that still covers distinct behavioral boundaries such as:
- one valid pair-specialization success path
- one blocked or failure path when present
- one boundary-misuse path when present
- one materially different target-role or output-shape variant when present

## What may be compacted

When example compaction is needed, the compiler may shorten:
- decorative scene-setting
- repeated filler wording
- repeated restatements of the same boundary

## What must remain

Do not remove:
- the signal that makes the example distinct
- the expected decision or outcome
- the reason the example matters
- the difference between positive, blocked, and misuse cases

## Runtime template boundary

Do not treat scoring rubrics, branch thresholds, null-versus-present behavior, or output-shape instructions inside a runtime-called prompt template as discardable examples.
Those belong to runtime call contracts, not to optional example compaction.
Do not collapse labeled score bands into a bare numeric range when the band meanings calibrate evaluation behavior.

## Output posture

Example provenance and omission notes belong in the output json.
The runtime prompt should carry only the retained example content.
