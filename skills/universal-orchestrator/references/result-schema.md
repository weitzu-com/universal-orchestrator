# Codex Result Schema

Codex must return this structure after execution or attempted execution.

```markdown
# CODEX RESULT

## Status
DONE | PARTIAL | BLOCKED | NEEDS_HUMAN

## Result
Short description of what changed or what was learned.

## Changed Files
- `/absolute/path`: change summary

## Diff Summary
Human-readable summary of the important diff. Include no unrelated changes.

## Tests
- command/check: PASS | FAIL | NOT RUN
- evidence: key output or reason

## Artifacts / Evidence
- local file, URL, screenshot, log, commit, PR, generated artifact, or inspection proof

## Unresolved Issues
- issue, impact, and recommended next step

## Human Approval Needed
- action requiring approval, or `None`
```

Quality bar:

- Evidence must prove the acceptance criteria, not merely show that work occurred.
- If tests were skipped, explain why and state residual risk.
- If files changed outside the intended scope, call it out explicitly.
- If a verifier had to be weakened, report `BLOCKED` instead of claiming success.
