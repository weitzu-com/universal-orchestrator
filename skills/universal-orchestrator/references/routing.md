# Routing Reference

Use this reference when deciding whether to invoke the full orchestration loop.

Use full orchestration for:

- code, file, repository, website, automation, data, or document work that requires local execution
- tasks with meaningful risk, multiple steps, or uncertain state
- user requests that explicitly mention ChatGPT -> Codex -> ChatGPT, planner/executor/reviewer, handoff, evidence, repair loop, or first-principles execution

Answer directly without full orchestration for:

- simple explanations, translations, rewrites, summaries, or one-step commands
- tasks where no file, command, tool, account, or external state is needed

Escalate to `NEEDS_HUMAN` before:

- production deployment
- deleting or overwriting data
- sending emails/messages
- publishing posts/pages
- purchases or financial trades
- modifying secrets or credentials
- changing account permissions
