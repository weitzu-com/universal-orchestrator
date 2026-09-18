# Review Schema

ChatGPT reviews after Codex returns a result. The review is independent: compare the original objective and acceptance criteria to the evidence, not just Codex's confidence.

```markdown
# CHATGPT REVIEW

## Verdict
PASS | FAIL | PARTIAL | BLOCKED | NEEDS_HUMAN

## Acceptance Criteria Check
- criterion: PASS | FAIL | UNCLEAR
  evidence: concrete proof or missing proof

## Evidence Assessment
What evidence is strong, weak, missing, or contradictory.

## Risk / Regression Notes
Residual risks, dirty-state concerns, unrun tests, production gaps, or authority conflicts.

## Required Repair
Minimal repair request if verdict is `FAIL` or fixable `PARTIAL`; otherwise `None`.

## Human Approval Gate
Approval still needed before any high-risk action; otherwise `None`.
```

Verdict rules:

- `PASS`: all acceptance criteria are met with adequate evidence and no unresolved approval gate blocks completion.
- `FAIL`: evidence shows at least one required criterion is not met and Codex can attempt a bounded repair.
- `PARTIAL`: useful progress was made but some criteria remain unmet or unverified.
- `BLOCKED`: progress cannot safely continue because of missing access, conflicting authority, unavailable evidence, or repeated verifier failure.
- `NEEDS_HUMAN`: the next meaningful action requires human approval or user-supplied information.
