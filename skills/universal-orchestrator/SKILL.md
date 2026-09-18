---
name: universal-orchestrator
description: Use ChatGPT as planner and reviewer while Codex executes scoped work; applies to multi-step tasks that need planning, file/code/command execution, evidence, review, and repair loops.
metadata:
  short-description: ChatGPT plans and reviews, Codex executes
---

# Universal Orchestrator

Use this skill when the user wants a general ChatGPT -> Codex -> ChatGPT workflow, or when a task is broad enough that planning, execution evidence, and independent review materially reduce risk.

The invariant is:

```text
PLAN in ChatGPT -> EXECUTE in Codex -> REVIEW in ChatGPT
```

## Operating Rules

- In Codex desktop, use the actual native bridge described in [native-bridge.md](references/native-bridge.md) before executing this workflow. Read local `bridge.local.json` for the configured ChatGPT task. Do not simulate ChatGPT planning or review with the executor itself.

- ChatGPT owns task understanding, scope, constraints, acceptance criteria, approval gates, review judgment, and user-facing synthesis.
- Codex owns bounded execution: reading files, editing files, running commands, tests, collecting diffs, and reporting evidence.
- Keep irreversible or high-risk actions behind a human approval gate: production deploys, destructive deletes, credential changes, external sends, purchases, publishing, bulk data writes, and any action the user marked as approval-required.
- Do not hide conflicts by weakening acceptance criteria or verifiers. Return `PARTIAL`, `BLOCKED`, or `NEEDS_HUMAN` when evidence is insufficient.
- Preserve existing user work. Inspect dirty state before edits; isolate work when risk is high; never revert unrelated changes.
- Allow one initial execution and at most three repair executions unless the user explicitly authorizes more.

## Workflow

1. Route the request. If the task is simple conversation, answer directly. If it needs execution, use the orchestration flow.
2. Create a plan using [plan-schema.md](references/plan-schema.md). Include objective, current state, desired state, scope, non-goals, constraints, acceptance criteria, verification, risks, and approval gates.
3. Convert the plan into a Codex handoff using [handoff-contract.md](references/handoff-contract.md). Make the request minimal, executable, and bounded.
4. Require Codex to report using [result-schema.md](references/result-schema.md): result, changed files, diff summary, tests, artifacts/evidence, unresolved issues, and any approval needs.
5. Review independently using [review-schema.md](references/review-schema.md). Compare the original objective and acceptance criteria against evidence. Return exactly one status: `PASS`, `FAIL`, `PARTIAL`, `BLOCKED`, or `NEEDS_HUMAN`.
6. On `FAIL` or fixable `PARTIAL`, send a minimal repair request from [repair-loop.md](references/repair-loop.md). Stop after three repairs, or earlier when a human gate is reached.

## Output To User

After the loop completes, report:

- final status and why
- what was created or changed
- tests and evidence
- unresolved issues
- human approvals still required
- how to use or continue the result

Keep the user-facing summary concise; preserve detailed evidence in artifacts when useful.
