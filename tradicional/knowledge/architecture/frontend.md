---
type: ArchitectureGuide
title: Traditional Frontend Guidelines
description: Coding rules, Signals, PrimeNG configurations, HTTP boundaries, and Tailwind styling for traditional setups.
timestamp: 2026-07-10T10:50:54-05:00
---

## Frontend Preferences
- **Angular Signals:** Use `WritableSignal`, `computed`, `effect`, `input()`, `output()` and reactive forms binding.
- **Private Fields:** Use ECMAScript private hash `#` (e.g. `#http = inject(HttpClient)`) instead of `private`.
- **Public Fields:** Always explicitly declare public properties/methods with `public`.
- **UI Toolkit:** PrimeNG 21+. Use a single `<p-toast />` in the root `app.html` template. Never duplicate `<p-toast>` in feature components.
- **Global Toast Service:** `MessageService` is provided globally in `app.config.ts`. Inject this global instance to dispatch notifications.
- **Routing & Interceptors:** Angular Standalone Router, functional HttpClient interceptors.
- **Error Handling:** BANNED: `try-catch` inside components. Use global exception interceptors and toast messaging.
- **Forms:** Always implement Reactive Forms (`FormGroup` + `FormControl`) with debounce on `valueChanges`.
- **Control Flow:** Mandatorily use `@if`, `@for`, `@defer`, `@switch`.
- **Architecture:** Standalone components only. Separate Smart containers (API/State) from Dumb components (Pure UI, Input/Output driven).
- **Dependency Injection:** Use `inject()` function over constructor injection.
- **Feature Folder Structure:** Group components by feature (`features/auth/components/`) instead of technical folders (Screaming architecture).
- **Separation of Files:** Always separate templates into `.html` and styles into `.scss` (except for extremely trivial components).
- **Model Types:** Prefer TypeScript `type` aliases over `interface` for representing client DTO models.
- **Styling:** Tailwind CSS is the primary choice for layout styling. Pure CSS/SCSS should only be used when Tailwind utilities are insufficient.

For directory structure details, read [/architecture/arquitecture-frontend.md](/architecture/arquitecture-frontend.md).
