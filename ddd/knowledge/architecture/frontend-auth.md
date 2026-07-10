---
type: ArchitectureGuide
title: Frontend Authentication (DDD)
description: MSAL setup, app roles, global auth service state, functional guards, and HTTP interceptors.
timestamp: 2026-07-10T10:50:54-05:00
---

## Auth Setup
- **Technology:** Integration with Identity Providers (e.g. `@azure/msal-angular` or OAuth2 standard libraries).
- **Backend:** Middleware validates incoming Bearer JWT tokens.

## App Roles (JWT claim 'roles')
- Typical claims role structure: `Admin`, `User`, `Guest`, etc.

## AuthService (Global State)
- Injected at root. Manages session life-cycle.
- Exposes helper signals:
  - `isLoggedIn`: Computed from auth status.
  - `hasRole(roleName)`: Returns boolean by inspecting the roles array claim.

## Guards (Functional `CanActivateFn`)
- `authGuard`: Confirms active login session. Redirects to `/auth/login` on failure.
- `roleGuard(...roles)`: Verifies role claims. Redirects to `/403` (Forbidden) on violation.

## Interceptors (`HttpInterceptorFn`)
- `auth.interceptor.ts`: Attaches the acquired Bearer token to requests.
- `base-url.interceptor.ts`: Appends the API target base path.
- `error.interceptor.ts`: Catch HTTP exceptions, logging them or throwing custom alerts.
