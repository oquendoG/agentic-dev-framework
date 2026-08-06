---
description: Scaffold a new Vertical Slice feature
---

Read:
- @AGENTS.md
- @knowledge/architecture/backend.md
- @knowledge/architecture/frontend.md

When asked to create a new feature:

1. **Understand Feature Intent & Scope**:
   - Identify Feature area name (e.g., `Estudiantes`, `Orders`).
   - Identify the specific Action (e.g., `Create`, `List`, `GetById`, `Update`, `Delete`).

2. **Backend Vertical Slice Scaffolding**:
   - Folder location: `Features/[FeatureName]/[Action]/`
   - Files to create following Screaming Architecture:
     - `I[Action][FeatureName]Service.cs`: Interface returning `Result<T>`.
     - `[Action][FeatureName]Service.cs`: Implementation injecting `DbContext` directly.
     - `[Action][FeatureName]Endpoint.cs` (or Controller): Single Action Endpoint transforming `Result<T>` into `ActionResult`.
     - `[Action][FeatureName]Validator.cs`: FluentValidation rules.
     - `[Action][FeatureName]Dto.cs`: Nominal records with `{ get; init; }`.

3. **TDD First (Red-Green-Refactor)**:
   - Create corresponding unit test file in `tests/` project: `[Action][FeatureName]ServiceTests.cs`.
   - Write failing unit test first using xUnit + Moq + Shouldly.
   - Run `dotnet test` to confirm test fails (Red).
   - Implement service & endpoint until `dotnet test` passes (Green).

4. **Frontend Vertical Slice Scaffolding** (If UI requested):
   - Folder location: `src/app/features/[feature-name]/`
   - Files to create:
     - `[feature-name].models.ts`: Using TS `type` (never `interface`).
     - `[feature-name].service.ts`: Using `HttpClient` with `inject()`.
     - `[feature-name].routes.ts`: Standalone route declarations.
     - Component files using Signal Forms & Angular Signals.

5. **Self-Check & Decision Logging**:
   - Verify explicit `Tipo varName = new();` (no `var` for named types).
   - Create `Features/[FeatureName]/AGENTS.md` if feature has non-obvious rules or gotchas.
