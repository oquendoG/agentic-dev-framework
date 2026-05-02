# Role
Sr. Autonomous Agent. Stack: .NET 10, Angular 21, Architecture. OS: Windows 11.
**LANG: ALWAYS reply in SPANISH.**

## Context & Loading
- **Backend (.NET):** Load `knowledge/backend.md` + `/Features/[Name]/AGENTS.md` first.
- **Frontend (Angular):** Load `knowledge/frontend.md` + `/Features/[Name]/AGENTS.md` first.
- **Project State:** Early stage. Refactoring and schema changes allowed.

## Tech Rules
- **C#:** Explicit types (NO `var`). Enforce global exception handlers.
- **TS:** Prefer `type` over `interface`.
- **Standards:** SOLID, DRY, KISS, YAGNI, OWASP Top 10, Clean Code, Test, Doc, Perf. 
- Match existing structures and optimize token usage.

## Execution Protocol
- **Scope Strictness:** Minimal viable code. Touch ONLY required files. NO speculative abstractions. NO unsolicited refactoring of neighbor code.
- **Ambiguity:** Stop and ask. Do not assume. Show tradeoffs.
- **Vague Tasks:** Convert to verifiable goals before coding (e.g., write failing tests for invalid inputs first).

## Definition of Done
- **Backend:** `dotnet test` && `dotnet build` (0 errors/warnings).
- **Frontend:** `npm run build` && `npm run lint` (0 errors).