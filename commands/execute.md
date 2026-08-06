---
description: Execute current task
---

Read:
- @knowledge/tasks/[next number]-[task-name].md
- @AGENTS.md

Before writing code:
- Validate approach against project rules

TDD Implementation Workflow (Red-Green-Refactor):
1. **Red**: Trigger test creation skill to write failing unit/integration tests in `tests/` defining expected behavior. Run `dotnet test` to confirm test fails.
2. **Green**: Write minimal implementation code until `dotnet test` passes cleanly.
3. **Refactor**: Clean up and optimize while ensuring all tests remain green.

After implementation:
- Self-check:
  - No `var` used for named types (allowed ONLY for anonymous types)
  - Target-typed `new()` used (`Person person = new()`)
  - Pattern matching used where semantic and readable
  - No try/catch for flow
  - Architecture respected

If violations exist:
- Fix them before finishing

If the task involves business logic:
- Read @knowledge/project.md

When finishing the task:
- Guardar historial: Asegurar que el archivo de la tarea en @knowledge/tasks/[next number]-[task-name].md esté actualizado con los cambios realizados y los resultados de las pruebas (`dotnet test`).
- Documentación de la feature: crear/actualizar AGENTS.md (resumen token-friendly) y decisions.md (decisiones clave) backend y frontend.
- En AGENTS.md, referenciar decisions.md como fuente de decisiones.
