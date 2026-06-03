# Role
Autonomous Agent. Stack: .NET 10, Angular 21. OS: Windows 11
LANG: ES.

## Context Loading
- **First session or context reset:** 
    `knowledge/project.md`
    `knowledge/tech-stack.md`
    `knowledge/current_state.md` 
    `knowledge/backend_index.md`
    `knowledge/frontend_index.md`
- **Backend task:** `knowledge/backend.md` always
  **Decisions → `knowledge/backend-decisions.md`** just if needed
- **Frontend task:** `knowledge/frontend.md` just if needed
- **Frontend task:** `knowledge/frontend.md` always
  **Decisions → `knowledge/frontend-decisions.md`** just if needed
- **Feature-specific work:** `Features/[Name]/AGENTS.md` if exists (check before creating new feature)
- Always use `token-efficiency` skill

## Decisions Documentation when finishing the task
- **Only document non-obvious decisions.** An agent reading the code cannot infer the "why" behind a choice — that's what decisions.md captures.
- **Obvious = inferrable from code.** If the code makes the decision self-evident, don't document it.
- **Non-obvious = requires context the code doesn't show.** Examples: non-intuitive config names, claim quirks, workarounds for platform bugs, security rationale, intentional trade-offs.
- **Structure:**
  - `knowledge/backend-decisions.md` — cross-feature backend decisions (non-obvious only)
  - `knowledge/frontend-decisions.md` — cross-feature frontend decisions (non-obvious only)
  - `Application/Features/[Name]/decisions.md` — feature-local decisions (non-obvious only, reference knowledge/ for cross-feature ones)
- **No duplication.** Feature decisions.md references knowledge/ instead of repeating content.
- If flows or arquitecture changes, document it in `knowledge/manual/manual_tecnico_dev.md` in spanish
- Save history in `knowledge/` based on pattern in folder
- If Arquitecture changes update context files knowledge/backend.md and knowledge/frontend_structure.md
- Update current project state in `knowledge/current_state.md` based on finished tasks for example
```markdown
# Current state

Authentication
- Entra ID

Frontend
- Angular 21
- Signals
- SignalStore

Backend
- Vertical Slice Architecture

Completed
- Users

In progress
- Permissions
``` 

- Save frontend and backend index in `knowledge/backend_index.md` and `knowledge/frontend_index.md`, just the necesary info needed to avoid the agent uses find, tree, glob or windows equivalents, for example
```markdown
Application
    Students/

Domain
    Shared/

Infrastructure
    Persistence/

Api
    Controllers/
```

## Tech Rules
- C#: explicit types (no var).
- TS: type over interface. Prefer Signal over RxJS (convert observables via `toSignal()`).
- Forms: MANDATORY Reactive Forms (FormGroup + FormControl). NEVER template-driven (ngModel).
- Forms + Signals: use `toSignal(form.valueChanges)` for reactive state, never `[(ngModel)]`.
- Code entity names: Spanish. Knowledge/docs: English.
- Standards: SOLID, DRY, KISS, YAGNI, OWASP, Clean Code.
- Always search for Exception Middleware and Global interceptor before use try catch - don't use try catch
- Generate short summaries(<Summary> tag) and comments in spanish for human devs where could be confuse or unclear

## Execution Protocol
- Minimal viable code. Touch ONLY required files.
- No speculative abstractions. No unsolicited refactoring.
- Ambiguity → ask. Vague task → convert to verifiable goals first.
- **Architecture violation → stop and explain.** If a request breaks layer boundaries, DDD rules, or project conventions (e.g., putting controllers in Application, adding web deps to domain layer), warn the user with the specific rule being violated and why before proposing or implementing anything.
- Phase 1 context: single tenant, no Finbuckle. Scaffold for Phase 2 but don't pre-implement.

## Restrictions
- Never perform full repository discovery
- Never list every file
- Never build folder trees
- Assume indexes are accurate
- `knowledge` is the source of truth
- Inspect files only when required to complete a task

## Definition of Done
- Backend: dotnet test && dotnet build (0 errors/warnings)
- Frontend: npm run build && npm run lint (0 errors)