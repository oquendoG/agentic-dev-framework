---
type: ArchitectureGuide
title: Frontend Guidelines (DDD)
description: Coding rules, package manager, state, HTTP, auth, and boundaries in Angular 21 for DDD setups.
timestamp: 2026-07-10T10:50:54-05:00
---

## Angular 21 Rules
- **Standalone:** Standard declaration `standalone: true` on all components. No `NgModules`.
- **Signals:** Prefer Angular Signals over RxJS. Use `input()`, `output()`, `viewChild()`, `model()`. BANNED: `@Input`, `@Output`, `@ViewChild`.
- **Control Flow:** Use `@if`, `@for`, `@defer`, `@switch`. Structural directives `*ngIf`/`*ngFor` are BANNED.
- **Dependency Injection:** Use `inject()` function, never constructor injection.
- **Architecture:** Smart pages (contain orchestration/store) vs Presentational components (pure UI, inputs/outputs).
- **Styling:** Tailwind CSS or design SCSS tokens. Fully responsive (mobile, tablet, desktop).
- **Libraries:** PrimeNG components. Only 1 global `p-toast` at root layout level.

## Package Manager
- **pnpm:** Mandatory. Lockfile-only (`pnpm-lock.yaml`), strict store, no hoisting. BANNED: npm, yarn, `package-lock.json`.

## State & HTTP
- **State:** Localized `SignalStore` (`@ngrx/signals`) per feature. No global stores except `AuthService`.
- **HTTP:** Inject `HttpClient` directly in services.
- Requests prefixed automatically by `base-url.interceptor.ts`.
- Use `httpResource` (Angular 19+) for read-only actions, `firstValueFrom` for command/mutations.
- `error.interceptor.ts` intercepts and parses RFC 7807 `ProblemDetails`. Never parse raw HTTP errors in components.

## Auth & Boundaries
- **Auth:** MSAL Guard evaluates roles parsed from the Microsoft Entra CIAM JWT.
- **Boundaries:** `shared/` must have ZERO dependencies on features. Cross-feature imports are forbidden.

## Sub-files
- State & HTTP detail: [/architecture/frontend-state.md](/architecture/frontend-state.md)
- Auth & Guards: [/architecture/frontend-auth.md](/architecture/frontend-auth.md)
- Folder Structure: [/architecture/frontend-structure.md](/architecture/frontend-structure.md)
