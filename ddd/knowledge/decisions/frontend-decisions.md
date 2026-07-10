---
type: DecisionLog
title: Frontend Decisions
description: Architectural and platform decisions log for Angular DDD frontend framework (D1–D9).
timestamp: 2026-07-10T10:50:54-05:00
---

# Frontend Decisions

## Auth & Identity

### D1 — Access token required, not ID token
See details in [/decisions/backend-decisions.md#d10-id-tokens-rejected-access-token-required](/decisions/backend-decisions.md#d10-id-tokens-rejected-access-token-required).

### D2 — Profile sync is non-blocking
Detailed implementation notes regarding async interceptors and scoped apiScope are documented in [Core Auth decisions](file:///F:/Usuario/Escritorio/agentic-dev-framework/src/app/core/auth/decisions.md).

## HTTP & Error Handling

### D7 — CSP Report-Only configuration
Backend sends `Content-Security-Policy-Report-Only`. PrimeNG libraries require `'unsafe-inline'` inside the `style-src` definition. Enforcing a strict nonce-based CSP is deferred until violation patterns are analyzed.

### D8 — Hybrid error messaging strategy
- **Backend Domain Errors (400, 404, 422, 500):** Parse and display `ProblemDetails.title/detail` as the single source of truth. Keeps error message catalogs in the backend only.
- **Transport Errors (429, 401, 403, 0):** Decoded from frontend `HttpErrorMessages` constants (network drops or timeout errors where backend didn't reply).
- **Fallback:** Maps status code to default messages if no custom body is found.

## Permissions

### D9 — PermisosService centralized signals
- Replaces repeated `hasRole('X') || hasRole('Y')` checks in layouts.
- Centralizes permissions inside `core/auth/` as `computed()` signals:
  - Base role checks: `esAdmin`, `esUser`, etc.
  - Compound permissions: `puedeEscribir`, `puedeVerDashboard`, `puedeAdministrar`.
- UI templates evaluate these permissions signals. Functional guards (`roleGuard`) evaluate roles directly.
