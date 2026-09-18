# Plan Schema

Use this schema before handing work to Codex. Keep it specific enough that another agent can execute without rediscovering the task.

```markdown
# TASK PLAN

## Task ID
Stable short id, date-based when no project id exists.

## Objective
The user-visible outcome to achieve.

## Current State
Known facts, files, repo, environment, previous context, and uncertainty.

## Desired State
Observable conditions that should be true when complete.

## Scope
Work Codex is allowed to do.

## Non-Goals
Work explicitly excluded or deferred.

## Constraints
Safety, technical, product, style, compatibility, budget, data, and time boundaries.

## Acceptance Criteria
- [ ] Measurable condition.
- [ ] Measurable condition.
- [ ] Evidence requirement.

## Verification
Commands, checks, inspection steps, artifact review, or live proof needed.

## Approval Gates
List every irreversible or high-risk action that requires human approval.

## Stop Conditions
When to stop and return `BLOCKED` or `NEEDS_HUMAN`.
```

Planning principles:

- Prefer clear acceptance criteria over long task prose.
- Separate facts from assumptions.
- Keep high-risk operations outside Codex execution unless approval has already been granted.
- If the request references current external facts, verify them before planning.
