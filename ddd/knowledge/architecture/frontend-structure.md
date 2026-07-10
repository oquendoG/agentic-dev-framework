---
type: ArchitectureGuide
title: Frontend Structure (DDD)
description: Routing surfaces, folder tree design, feature module standards, and coding constraints in Angular 21.
timestamp: 2026-07-10T10:50:54-05:00
---

## Surfaces
- `/admin` or `/dashboard` -> Internal staff view. Guarded by Auth & App Roles.
- `/portal` or `/public` -> External client / public portal view.

## Directory Layout
- `src/app/core/`: Interceptors, MSAL/Auth configuration, and core routing. Global infrastructure only (no UI/view elements).
- `src/app/shared/`: Generic UI components (paginator, fields, tags), pipes. Zero business logic. BANNED: importing from features.
- `src/app/features/`: UI Bounded Contexts / Surfaces. Divided into subfolders representing feature modules.

## Feature Structure
Every feature subdirectory inside a surface follows this pattern:
```
feature/
├── pages/          # Smart components (inject services/stores, handle routes/orchestration)
├── components/     # Presentational components (dumb, input/output, pure design)
├── services/       # Feature API HttpClient requests
├── store/          # Local SignalStore state management
├── models/         # TypeScript types/interfaces representing DTOs
└── *.routes.ts     # Route mapping (lazy-loaded)
```

## Constraints
- Standalone components only.
- Direct component lazy-loading in routing files via `loadComponent()`.
- Feature isolation: Cross-feature imports are forbidden.
- Components must never inject `HttpClient` directly; use feature services/stores instead.
