---
description: Execute current task
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
- Create a file with the name of the task in @knowledge/tasks/history/[task-name].md and copy all the content of the @knowledge/tasks/current.md file because this file will be overwritten in future tasks and we want to conserve history of tasks
- Crea un Archivo AGENTS.md en la raiz de la feature para documentar el contexto de la feature token friendly y ademas un archivo decisions.md donde se documentaran las decisiones importantes sobre la feature para que el agente tenga contexto y luego el AGENTS.md debe referenciar ese archivo indicando que se debe seguir las decisiones en el archivo decisions.md
