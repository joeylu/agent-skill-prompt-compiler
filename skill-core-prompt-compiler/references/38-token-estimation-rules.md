# Token Estimation Rules

## Goal

Estimate the size of the normalized prompt artifact so the caller can decide whether the target runtime has enough room.

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
