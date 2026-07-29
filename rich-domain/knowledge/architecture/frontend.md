---
type: ArchitectureGuide
title: Frontend Architecture
description: Frontend component patterns, routing, and state management.
timestamp: 2026-07-29T10:00:00-05:00
---

# Frontend Architecture

## Component Structure
```
src/app/
├── core/
│   ├── auth/           # MSAL configuration
│   └── interceptors/   # HTTP interceptors
├── shared/
│   └── components/     # Generic UI components
├── features/
│   └── [feature]/
│       ├── [feature].component.ts
│       ├── [feature].service.ts
│       └── [feature].routes.ts
└── app.routes.ts
```

## Signal Pattern
- Use Signals for reactive state
- Prefer `signal()` over `BehaviorSubject`
- Use `computed()` for derived state
- Use `effect()` for side effects

## Form Pattern
- MANDATORY Reactive Forms (FormGroup + FormControl)
- NEVER template-driven (ngModel)
- Use `toSignal(form.valueChanges)` for reactive state
