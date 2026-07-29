---
type: ArchitectureIndex
title: Frontend Index
description: Layout, routes, UI feature maps, and file directories for Angular SPA.
timestamp: 2026-07-29T10:00:00-05:00
---

# Frontend Index

## Routes Map
| Route | Layout | Guard | Audience |
|---|---|---|---|
| `/` | MainLayoutComponent | — | Public |

## Directory Layout
```
src/app/
├── core/             # Auth configurations, interceptors, routing guards
├── shared/           # Generic buttons, tables, pipes, direct UI elements
├── features/         # Page modules, states, services
└── app.routes.ts     # Main routes configurations
```
