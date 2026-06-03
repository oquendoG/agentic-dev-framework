---
description: Refactor the current task
---

Read:
- @knowledge/tasks/current.md
- @AGENTS.md

Before writing code:
- Validate approach against project rules

During implementation:
- Follow all critical rules strictly

After implementation:
- Self-check:
  - No `var` used
  - No try/catch for flow
  - Architecture respected

If violations exist:
- Fix them before finishing

If the task involves business logic:
- Read @knowledge/project.md

When finishing the task:
- save history: copy @knowledge/tasks/current.md to @knowledge/tasks/history/[task-name].md (overwritten later).
- Documentación de la feature: create/update AGENTS.md (summary token-friendly) and decisions.md backend and frontend.
- In AGENTS.md, reference decisions.md as decision source.
