# Frontend Architecture & Conventions

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
- Prefer **Signals** (`WritableSignal`, `computed`, `effect`, `input`, `output`) over RxJS for local state and component communication.
- If Observables are consumed, convert them immediately via `toSignal()`.

### Code Style & Modifiers
- Prefer `#` for private fields instead of `private`.
- Always use `public` keyword explicitly for public members.
- Use `inject()` function (e.g., `private _http = inject(HttpClient);`) instead of constructor injection.
- Standalone components (`standalone: true`) are mandatory.
- Separate templates and styles into `.html` and `.scss` files (inline templates allowed only for trivial components).
- Async/await is mandatory for Promises. `.then()` is forbidden.

### Exception Handling
- Do NOT use `try-catch` inside components or HTTP services. Global exception handlers and interceptors cover those automatically.

---

## 2. Framework & UI Architecture

### Component Architecture
- **Vertical Slice (Screaming Architecture)**: Group by Feature (`features/feature-name/...`) containing components, services, models, and routes.
- **Smart / Dumb Separation**: Containers manage logic/API; Presentational components manage UI via inputs and outputs.
- **Page Headers**: Use `<app-page-header>` with `gradient` input per feature. Creating gradient divs manually is forbidden.

### PrimeNG 21 & Styling Guidelines
- Single `<p-toast />` in `app.html` root. Do NOT place `<p-toast>` in individual component templates.
- **PrimeNG 21 Classes**: `styleClass` is deprecated for `p-card`, `p-table`, `p-inputicon`, `p-dialog`. Use `class` instead.
- **`p-button` Exception**: `p-button` MUST use `styleClass` (NOT `class`).
- **Tailwind CSS**: First choice for styling. Pure CSS/SCSS only when Tailwind lacks required functionality.

### Global Interceptors & Toast Handling
- `MessageService` is provided globally in `app.config.ts`.
- **Prohibited**: Calling `messageService.add()` manually for HTTP success responses. `OkInterceptor` + `GlobalNotificationService` automatically display toasts for `POST/PUT/DELETE/PATCH` using response details. Inject `MessageService` directly ONLY for non-HTTP local flows.

---

## 3. Skills Guidance for Frontend

The agent must trigger available skills in the environment based on context:
- **Frontend / Angular**: Trigger available modern web guidance and Angular development skills.
- **Token Efficiency**: Trigger token efficiency skills for context optimization.
