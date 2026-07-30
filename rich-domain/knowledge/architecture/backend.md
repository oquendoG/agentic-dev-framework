---
type: ArchitectureGuide
title: Backend Architecture
description: Backend service patterns, dependency injection, and project structure.
timestamp: 2026-07-29T10:00:00-05:00
---

When backend skill instructions conflict with this document, follow this document — not the skill. All other skill instructions still apply.

# Backend Architecture

## Project Structure
```
src/
├── Web.API/                    # Host, Program.cs, endpoints
│   ├── Features/               # Vertical slices
│   │   ├── [Feature]/
│   │   │   ├── Create/
│   │   │   ├── Update/
│   │   │   ├── Delete/
│   │   │   ├── List/
│   │   │   └── Get/
│   │   └── ...
│   ├── Domain/
│   │   ├── Entities/           # Rich domain entities
│   │   ├── Common/             # Result<T>, shared types
│   │   └── Enums/
│   ├── Infrastructure/
│   │   └── Data/               # DbContext, migrations
│   └── Shared/
│       └── Constants/
└── Tests/
    └── Tests.API/
```

## Layer Responsibilities
- **Domain:** Entities with behavior, value objects, enums. No external dependencies.
- **Features:** Use cases (services), DTOs, validators, endpoints.
- **Infrastructure:** DbContext, external API clients, storage implementations.
- **Shared:** Constants, extensions, utilities.

## Dependency Injection
- Register services in feature-specific extension methods
- Use `AddFeatureServices(this IServiceCollection services)` pattern
- Register in `Program.cs` via `builder.Services.AddFeatureServices()`
