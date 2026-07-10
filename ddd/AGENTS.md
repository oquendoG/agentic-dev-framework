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
  - Read also: `knowledge/architecture/backend-ddd.md`, `knowledge/architecture/backend-data.md`
  **Decisions → `knowledge/decisions/backend-decisions.md`** just if needed
- **Frontend task:** `knowledge/architecture/frontend.md` just if needed
  - Read also: `knowledge/architecture/frontend-structure.md`, `knowledge/architecture/frontend-state.md`
  **Decisions → `knowledge/decisions/frontend-decisions.md`** just if needed
- **Feature-specific work:** ALWAYS read `Features/[Name]/AGENTS.md` before touching any feature code (adding, modifying, or debugging). If it references a `decisions.md`, read that too. Never skip — it contains critical invariants, FSM rules, and gotchas the code alone doesn't show.
- Always use `token-efficiency` skill

## Decisions Documentation when finishing the task
- **Only document non-obvious decisions.** An agent reading the code cannot infer the "why" behind a choice — that's what decisions.md captures.
- **Obvious = inferrable from code.** If the code makes the decision self-evident, don't document it.
- **Non-obvious = requires context the code doesn't show.** Examples: non-intuitive config names, claim quirks, workarounds for platform bugs, security rationale, intentional trade-offs.
- **Structure:**
  - `knowledge/decisions/backend-decisions.md` — cross-feature backend decisions (non-obvious only) - English
  - `knowledge/decisions/frontend-decisions.md` — cross-feature frontend decisions (non-obvious only) - English
  - `Application/Features/[Name]/AGENTS.md` with general info of feature, and the rules and patterns it must follow, it references `Application/Features/[Name]/decisions.md` if exists - English
  - `Application/Features/[Name]/decisions.md` — feature-local decisions (non-obvious only, reference knowledge/ for cross-feature ones) - English
- **No duplication.** Feature decisions.md references knowledge/ instead of repeating content.
- If flows or architecture changes, document it in `knowledge/manual/manual_tecnico_dev.md` - spanish
- Copy `current.md` and rename it to `[next number][task-name].md` to `knowledge/tasks/history/` - spanish
- If Architecture changes update context files `knowledge/architecture/backend.md` and `knowledge/architecture/frontend-structure.md` - English
- Update current project state in `knowledge/current_state.md` based on finished tasks - English
- Update frontend and backend index in `knowledge/backend_index.md` and `knowledge/frontend_index.md`, just the necessary info needed to avoid the agent uses find, tree, glob or windows equivalents - English

## Tech Rules
- C#: explicit types (no var).
- TS: type over interface. Prefer Signal over RxJS (convert observables via `toSignal()`).
- Forms: MANDATORY Reactive Forms (FormGroup + FormControl). NEVER template-driven (ngModel).
- Forms + Signals: use `toSignal(form.valueChanges)` for reactive state, never `[(ngModel)]`.
- Code entity names: Spanish. Knowledge/docs: English.
- Standards: SOLID, DRY, KISS, YAGNI, OWASP, Clean Code.
- **try-catch**: ONLY on external boundaries (MSAL, `JSON.parse`, Browser APIs like FileReader/localStorage). NEVER on internal services/HTTP calls — global handlers cover those. Use `.catch(() => null)` for silent fallbacks. Use `finalize()` for spinner/loading state cleanup instead of try/finally.
- Generate short summaries(<Summary> tag) and comments in spanish for human devs where could be confusing or unclear

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
