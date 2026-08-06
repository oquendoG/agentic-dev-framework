---
description: Plan a task
---

Read:
- @knowledge/project.md (if relevant)

Analyze the request and:

- Identify the Feature / Module name (e.g. `estudiantes`, `auth`, `seguridad`).
- Break into small atomic steps following TDD (Test-Driven Development).
- If request introduces a brand new feature or vertical slice, include scaffolding step via `/new-feature`.
- Format steps as a markdown checklist (`- [ ]` for pending items):
  - `- [ ] TDD Red: Write failing unit/integration tests in tests/`
  - `- [ ] TDD Green: Implement minimal viable backend code`
  - `- [ ] TDD Refactor & Verification: Clean up code and verify dotnet test`
- Separate backend/frontend/db.
- Identify risks.
- Implement backend first wait for frontend confirmation.

Write to:
	@knowledge/tasks/[modulo]/[siguiente-numero]-[nombre-tarea].md