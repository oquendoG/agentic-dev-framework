---
description: Review implementation
---

## Review current changes:

- Code quality & TDD compliance
- Layered Precedence adherence (`AGENTS.md` & architecture docs > Skills)
- Architecture alignment (Vertical Slice, screaming architecture, Result<T>)
- Edge cases & security audits
- Performance checks (No N+1 queries, `AsNoTracking()`, `AsSplitQuery()`, Span/Memory optimization)

If issues found:
- Fix them immediately OR add them to the task checklist as `- [ ] TODO`.

If a new architectural or technical decision is introduced:
- Add it to the corresponding global decisions log in `knowledge/decisions/` (e.g., `traditional-decisions.md` or `backend-decisions.md`/`frontend-decisions.md`).
- Add it to `Features/[Name]/AGENTS.md` or `decisions.md` if feature-specific.
- Avoid duplicates, keep it concise.

---

## Check for rule violations (CRITICAL CHECKLIST):

### C# & .NET Violations (FAIL immediately if found)
- **`var` usage**: Any usage of `var` for named types → **FAIL** (Use explicit `Tipo name = new()`; `var` allowed ONLY for anonymous types).
- **Target-Typed `new()`**: Duplicating class name `Type name = new Type()` → **FAIL**.
- **Repository Pattern**: Creating Repository abstractions in traditional/rich-domain → **FAIL** (Inject `DbContext` directly).
- **Exception Flow**: Throwing exceptions for business errors instead of returning `Result<T>` → **FAIL**.
- **`try/catch`**: Any `try/catch` in internal services or controllers → **FAIL** (Global middleware/handlers cover internal flow).
- **Magic Strings**: Hardcoded error messages or claims in business logic → **FAIL** (Use static constants in `Domain/Constants`).

### TypeScript & Angular Violations (FAIL immediately if found)
- **Interfaces**: Using `interface` instead of `type` for models/DTOs → **FAIL**.
- **Forms**: Using template-driven forms (`ngModel`) or Reactive Forms when Signal Forms could be used → **FAIL**.
- **Reactivity**: Using RxJS when Signals (`WritableSignal`, `computed`, `toSignal()`) can be used → **FAIL**.
- **DI**: Using constructor injection instead of `inject()` function → **FAIL**.

### Automated Build & Test Checks
- **`dotnet build`**: Any build errors or warnings → **FAIL**.
- **`dotnet test`**: Missing tests or failing tests in `tests/` project → **FAIL**.

If any violation exists:
- Fix it immediately before accepting implementation.

---

## Update Task Progress:
Verify that all task checklist items are marked as completed (`- [x]`) in `@knowledge/tasks/[modulo]/[siguiente-numero]-[nombre-tarea].md` and record verified `dotnet test` results.