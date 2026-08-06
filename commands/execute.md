---
description: Execute current task
---

Read:
- @knowledge/tasks/[modulo]/[siguiente-numero]-[nombre-tarea].md
- @AGENTS.md
- @knowledge/architecture/backend.md
- @knowledge/architecture/frontend.md

Before writing implementation code:
1. **Scaffolding Check**: If this task creates a brand new feature or slice that has not been scaffolded, execute the scaffolding steps defined in `/new-feature` first.
2. **Validate Approach**: Ensure architecture alignment (Vertical Slice, screaming architecture, `Result<T>`).

TDD Implementation Workflow (Red-Green-Refactor):
1. **Red (Failing Test)**: Trigger test creation skills to write failing unit/integration tests in `tests/` defining expected behavior and edge cases. Run `dotnet test` to confirm tests fail. Mark checklist item `- [x] TDD Red`.
2. **Green (Passing Code)**: Write minimal implementation code in the feature service/endpoint until `dotnet test` passes cleanly. Mark checklist item `- [x] TDD Green`.
3. **Refactor (Clean Code)**: Clean up and optimize while ensuring all tests remain 100% green. Mark checklist item `- [x] TDD Refactor`.

Self-Check during implementation:
- Explicit `Tipo varName = new();` (NO `var` for named types; `var` allowed ONLY for anonymous types).
- Target-typed `new()` used without repeating class name.
- Pragmatic pattern matching used where semantic and readable.
- `try/catch` used ONLY on external boundaries.
- Angular Signal Forms preferred over Reactive Forms.

When finishing implementation:
1. **Run Review Audit**: Perform the critical checklist audit defined in `/review` (or mention `/review`).
2. **Update Task Checklist & Log**: Mark all completed checklist items (`- [x]`) in `@knowledge/tasks/[modulo]/[siguiente-numero]-[nombre-tarea].md` and update execution log with `dotnet test` results.
3. **Document Decisions**: Create/update `Features/[Name]/AGENTS.md` and `decisions.md` if non-obvious decisions were introduced.
