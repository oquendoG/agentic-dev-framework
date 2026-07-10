---
description: Refactor the current task
---

Read:
- @knowledge/tasks/[next number]-[task-name].md
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
- save history: Ensure the task file @knowledge/tasks/[next number]-[task-name].md is updated with the implemented changes and test outcomes.
- Documentación de la feature: create/update AGENTS.md (summary token-friendly) and decisions.md backend and frontend.
- In AGENTS.md, reference decisions.md as decision source.
