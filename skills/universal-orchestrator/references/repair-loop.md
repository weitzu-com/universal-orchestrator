# Repair Loop

Use this only after ChatGPT review returns `FAIL` or a fixable `PARTIAL`.

```markdown
# CODEX REPAIR REQUEST

## Previous Verdict
FAIL | PARTIAL

## Failed Criteria
- exact criterion and why it failed

## Minimal Repair Scope
Only the smallest change needed to satisfy the failed criteria.

## Do Not Change
Files, behavior, evidence, or user work that should remain untouched.

## Verification Required
Checks that must pass before returning.

## Attempt Count
1 of 3, 2 of 3, or 3 of 3.
```

Stop the loop when:

- attempt 3 completes without a `PASS`
- a human approval gate is reached
- the same blocker appears twice
- the repair would require widening scope beyond the original plan
- evidence would require external access that is unavailable

After stopping, report the status honestly as `PARTIAL`, `BLOCKED`, or `NEEDS_HUMAN`.
