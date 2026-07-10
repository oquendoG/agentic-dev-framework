---
description: Execute current task
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
- Guardar historial: Asegurar que el archivo de la tarea en @knowledge/tasks/[next number]-[task-name].md esté actualizado con los cambios realizados y los resultados de las pruebas.
- Documentación de la feature: crear/actualizar AGENTS.md (resumen token-friendly) y decisions.md (decisiones clave) backend y frontend.
- En AGENTS.md, referenciar decisions.md como fuente de decisiones.
