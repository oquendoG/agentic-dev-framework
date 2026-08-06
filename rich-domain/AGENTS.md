# Role
Autonomous Agent. Stack: .NET 10, Angular 21+. OS: Windows 11
Answer LANG: ES.

## Layered Precedence (Regla de Capas)
- `AGENTS.md`, `knowledge/architecture/backend.md` y `knowledge/architecture/frontend.md` tienen máxima prioridad sobre cualquier recomendación de skills globales o locales.

## Context Loading
- **First session or context reset:** 
    `knowledge/project.md`
    `knowledge/tech-stack.md`
    `knowledge/current_state.md` 
    `knowledge/backend_index.md`
    `knowledge/frontend_index.md`
- **Backend task:** `knowledge/architecture/backend.md` always
  - Read also: `knowledge/architecture/backend-patterns.md`
  **Decisions → `knowledge/decisions/decisions.md`** just if needed
- **Frontend task:** `knowledge/architecture/frontend.md` always
- **Feature-specific work:** `Features/[Name]/AGENTS.md` if exists (check before creating new feature)
- Always use `token-efficiency` skill

## Tech Rules & Conventions
- **Source of Truth for Backend:** Read `knowledge/architecture/backend.md`.
- **Source of Truth for Frontend:** Read `knowledge/architecture/frontend.md`.
- **C# Instantiation:** `Tipo variable = new();` (Strictly explicit type left, target-typed `new()` right. NO `var` for named types; use `var` ONLY for anonymous types).
- **C# Logic:** Pattern matching where semantic and readable; mix imperatively when cleaner.
- **Frontend:** TS `type` over `interface`. Signal Forms over Reactive Forms (Reactive Forms ONLY if Signal Forms cannot achieve the goal). Signals over RxJS.
- **try-catch:** ONLY on external boundaries. Global handlers cover internal services.

## Skills Guidance
- **C# / .NET:** Trigger relevant C#, .NET API, and performance skills.
- **Pattern Matching:** Trigger relevant C# pattern matching skills.
- **Frontend / Angular:** Trigger relevant Angular and modern web skills.
- **Testing:** Trigger test creation skills for test creation, and test runner skills for running/diagnosing `dotnet test`.

## Entity Rules (Rich Domain Model)
- **Properties:** Use `private set` for encapsulation. EF Core can set via reflection.
- **Factory methods:** `Entity.Create(...)` for domain invariants. Returns the entity or throws `ArgumentException` for invariant violations.
- **Behavior methods:** `entity.Method()` for domain logic. Uses `Result<T>` for business errors.
- **Update methods:** `entity.Update(...)` for changing state. Called by services, not direct property assignment.
- **Guards:** Use `ArgumentException.ThrowIfNullOrWhiteSpace(...)` for input validation in services.
- **Exceptions:** Only for invariant violations (programmer errors). Business errors use `Result<T>`.

## Execution Protocol (TDD & Scope)
- **TDD Workflow (Red-Green-Refactor):** Mandatory for backend business logic. Write failing unit/integration tests first (Red), implement minimal code to pass (Green), and refactor.
- **Scope Strictness:** Minimal viable code. Touch ONLY required files. NO speculative abstractions. NO unsolicited refactoring of neighbor code.
- **Ambiguity:** Stop and ask. Do not assume. Show tradeoffs.
- **Vague Tasks:** Convert to verifiable goals before coding (write failing tests for invalid inputs first).
- **Architecture violation → stop and explain.** If a request breaks layer boundaries, warn the user.

## Decisions Documentation when finishing the task
- **Only document non-obvious decisions.** An agent reading the code cannot infer the "why" behind a choice — that's what decisions.md captures.
- **Obvious = inferrable from code.** If the code makes the decision self-evident, don't document it.
- **Non-obvious = requires context the code doesn't show.** Examples: non-intuitive config names, claim quirks, workarounds for platform bugs, security rationale, intentional trade-offs.
- **Structure:**
  - `knowledge/decisions/decisions.md` — cross-feature decisions (non-obvious only) - English
  - `Features/[Name]/AGENTS.md` — feature-local decisions (non-obvious only) - English
- **No duplication.** Feature decisions.md references knowledge/ instead of repeating content.
- If flows or architecture changes, document it in `knowledge/manual/manual_tecnico_dev.md` - spanish
- Document finished tasks directly in `knowledge/tasks/[next number]-[task-name].md` - spanish
- If Architecture changes update context files `knowledge/architecture/backend.md`
- Update current project state in `knowledge/current_state.md` based on finished tasks
- Update frontend and backend index in `knowledge/backend_index.md` and `knowledge/frontend_index.md`, just the necessary info needed to avoid the agent uses find, tree, glob or windows equivalents

## Restrictions
- Never perform full repository discovery
- Never list every file
- Never build folder trees
- Assume indexes are accurate
- `knowledge` is the source of truth
- Inspect files only when required to complete a task

## Definition of Done
- **Backend:** `dotnet test` && `dotnet build` (0 errors/warnings).
- **Frontend:** `npm run build` && `npm run lint` (0 errors).
