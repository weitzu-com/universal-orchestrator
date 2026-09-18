# Codex Handoff Contract

Use this when turning a ChatGPT plan into executable Codex work.

```markdown
# CODEX HANDOFF

## Objective
One paragraph copied or tightened from the plan.

## Working Area
Repository, directory, branch, worktree, or project context.

## Allowed Actions
File reads, edits, local commands, tests, local artifact generation, screenshots, or other allowed work.

## Forbidden Without Human Approval
Production deploys, destructive deletes, external sends, publishing, purchases, secret changes, credential exposure, or other high-risk actions.

## Execution Steps
1. Inspect current state.
2. Make the smallest sufficient changes.
3. Run verification.
4. Report result with evidence.

## Acceptance Criteria
Copy every checkbox from the plan.

## Required Result Format
Return the result using `references/result-schema.md`.

## Stop Conditions
Return without further mutation if blocked by missing access, ambiguous authority, conflicting acceptance criteria, dirty unrelated work that prevents safe edits, failed approval gate, or repeated verifier failure.
```

Handoff rules:

- Make Codex's task narrower than the whole user conversation.
- Include enough context to avoid re-planning the entire problem.
- Do not grant implicit permission for deploys, sends, publishes, deletes, or production writes.
- Ask Codex to preserve unrelated work and state what evidence it must provide.
