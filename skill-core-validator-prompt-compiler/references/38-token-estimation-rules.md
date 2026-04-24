# Token Estimation Rules

## Goal

Estimate the size of the normalized prompt artifact so the caller can decide whether the target runtime has enough room.

This is advisory-only reporting.
Token estimation does not block compilation and does not trigger automatic runtime-shape changes.

## Counting method priority

Use this order:
1. provider-native counting when available
2. local tokenizer when reliable
3. character heuristic when no tokenizer is available

## Heuristic guidance

For coarse estimation:
- English-heavy text: about 1 token per 4 characters
- CJK-heavy text: about 1 token per 1.5 characters
- Mixed text: use the more conservative side when uncertain

## Reporting rule

Report both:
- the chosen counting method
- a short note explaining the estimate

If `requestContext.tokenBudget` is supplied:
- use `maxPromptTokens` only as a caller-facing comparison point in notes
- use `reserveForUserPayload` only as a caller-facing reservation hint in notes
- do not auto-trim examples, support units, or runtime material to satisfy it
- do not block only because the estimate is above the advisory budget
