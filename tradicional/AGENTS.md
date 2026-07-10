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
- **Backend task:** `knowledge/architecture/backend.md` always
  - Read also: `knowledge/architecture/arquitecture-backend.md`, `knowledge/architecture/backend-patterns.md`
  **Decisions → `knowledge/decisions/traditional-decisions.md`** just if needed
- **Frontend task:** `knowledge/architecture/frontend.md` just if needed
  - Read also: `knowledge/architecture/arquitecture-frontend.md`
- **Feature-specific work:** `Features/[Name]/AGENTS.md` if exists (check before creating new feature)
- Always use `token-efficiency` skill

## Decisions Documentation when finishing the task
- **Only document non-obvious decisions.** An agent reading the code cannot infer the "why" behind a choice — that's what decisions.md captures.
- **Obvious = inferrable from code.** If the code makes the decision self-evident, don't document it.
- **Non-obvious = requires context the code doesn't show.** Examples: non-intuitive config names, claim quirks, workarounds for platform bugs, security rationale, intentional trade-offs.
- **Structure:**
  - `knowledge/decisions/traditional-decisions.md` — cross-feature decisions (non-obvious only) - English
  - `Features/[Name]/decisions.md` — feature-local decisions (non-obvious only) - English
- **No duplication.** Feature decisions.md references knowledge/ instead of repeating content.
- If flows or architecture changes, document it in `knowledge/manual/manual_tecnico_dev.md` - spanish
- Document finished tasks directly in `knowledge/tasks/[next number]-[task-name].md` - spanish
- If Architecture changes update context files `knowledge/architecture/backend.md` and `knowledge/architecture/arquitecture-backend.md`
- Update current project state in `knowledge/current_state.md` based on finished tasks
- Update frontend and backend index in `knowledge/backend_index.md` and `knowledge/frontend_index.md`, just the necessary info needed to avoid the agent uses find, tree, glob or windows equivalents

## Tech Rules
- C#: explicit types (no var).
- TS: type over interface. Prefer Signal over RxJS (convert observables via `toSignal()`).
- Forms: MANDATORY Reactive Forms (FormGroup + FormControl). NEVER template-driven (ngModel).
- Forms + Signals: use `toSignal(form.valueChanges)` for reactive state, never `[(ngModel)]`.
- Code entity names: Spanish. Knowledge/docs: English.
- Standards: SOLID, DRY, KISS, YAGNI, OWASP, Clean Code.
- Always search for Exception Middleware and Global interceptor before using try-catch — don't use try-catch.
- Generate short summaries(<Summary> tag) and comments in spanish for human devs where could be confusing or unclear

## Execution Protocol
- Minimal viable code. Touch ONLY required files.
- No speculative abstractions. No unsolicited refactoring.
- Ambiguity → ask. Vague task → convert to verifiable goals first.
- **Architecture violation → stop and explain.** If a request breaks layer boundaries, warn the user.
- Phase 1 context: single tenant. Scaffold for Phase 2 but don't pre-implement.

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
