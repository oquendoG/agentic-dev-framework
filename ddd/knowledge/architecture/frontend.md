---
type: ArchitectureGuide
title: Frontend Guidelines (DDD)
description: Coding rules, package manager, state, HTTP, auth, and boundaries in Angular 21 for DDD setups.
timestamp: 2026-08-06T15:31:00-05:00
---

# Frontend Guidelines (DDD Architecture & Conventions)

> [!IMPORTANT]
> **Layered Precedence Rule:**
> This document and `AGENTS.md` strictly override any recommendation or preference suggested by global or local skills. In case of conflict between a skill's suggestion and this document, **this document takes absolute precedence**.

---

## 1. TypeScript & Angular Syntax Conventions

### TypeScript Types
- Strictly prefer `type` over `interface` for data models and DTOs.

### Angular Forms
- Prefer **Signal Forms** over Reactive Forms.
- Use Reactive Forms (`FormGroup` + `FormControl`) *if and only if it is not possible to achieve the goal using Signal Forms*.
- Usage of template-driven forms (`ngModel`) is strictly prohibited.

### Reactivity & State
- **Angular Signals**: Prefer Angular Signals over RxJS. Use `input()`, `output()`, `viewChild()`, `model()`. BANNED: `@Input`, `@Output`, `@ViewChild`.
- If Observables are consumed, convert them immediately via `toSignal()`.

### Code Style & Modifiers
- Prefer `#` for private fields instead of `private`.
- Always use `public` keyword explicitly for public members.
- Use `inject()` function, never constructor injection.
- Standalone components (`standalone: true`) are mandatory. No `NgModules`.
- Control flow: Use `@if`, `@for`, `@defer`, `@switch`. Structural directives `*ngIf`/`*ngFor` are BANNED.
- Separate templates and styles into `.html` and `.scss` files.

### Exception Handling
- Do NOT use `try-catch` inside components or HTTP services. Global exception handlers and interceptors cover those automatically.

---

## 2. Package Manager & Architecture

### Package Manager
- **pnpm**: Mandatory. Lockfile-only (`pnpm-lock.yaml`), strict store, no hoisting. BANNED: npm, yarn, `package-lock.json`.

### Architecture & Boundaries
- **Smart Pages vs Presentational Components**: Smart pages contain orchestration/store; presentational components manage pure UI.
- **Boundaries**: `shared/` must have ZERO dependencies on features. Cross-feature imports are forbidden.
- **Styling & Components**: Tailwind CSS or design SCSS tokens. Fully responsive. PrimeNG components. Only 1 global `p-toast` at root layout level.

### State & HTTP
- **State**: Localized `SignalStore` (`@ngrx/signals`) per feature. No global stores except `AuthService`.
- **HTTP**: Inject `HttpClient` directly in services. Requests prefixed automatically by `base-url.interceptor.ts`. Use `httpResource` (Angular 19+) for read-only actions, `firstValueFrom` for command/mutations.
- `error.interceptor.ts` intercepts and parses RFC 7807 `ProblemDetails`. Never parse raw HTTP errors in components.

### Auth
- **Auth**: MSAL Guard evaluates roles parsed from Microsoft Entra CIAM JWT.

---

## 3. Skills Guidance for Frontend

The agent must trigger available skills in the environment based on context:
- **Frontend / Angular**: Trigger available modern web guidance and Angular development skills.
- **Token Efficiency**: Trigger token efficiency skills for context optimization.

---

## 4. Sub-files
- State & HTTP detail: [/architecture/frontend-state.md](/architecture/frontend-state.md)
- Auth & Guards: [/architecture/frontend-auth.md](/architecture/frontend-auth.md)
- Folder Structure: [/architecture/frontend-structure.md](/architecture/frontend-structure.md)
