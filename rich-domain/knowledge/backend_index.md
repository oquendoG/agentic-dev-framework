---
type: ArchitectureIndex
title: Backend Index
description: Repository codebase structure, namespaces, layer layouts, and project files map.
timestamp: 2026-07-29T10:00:00-05:00
---

# Backend Index

## Project Structure
```
src/
├── Web.API/           # Host, Program.cs, controllers, middlewares
├── Application/       # Use cases, interfaces/ports, services/handlers
├── Infrastructure/   # Database, external API clients, services implementations
└── Domain/           # Entities with behavior, value objects, enums
```

## Entity Pattern (Rich Domain)
```
Domain/Entities/
├── Entity.cs                    # Entity with private set
│   - Id (private set)
│   - Properties (private set)
│   - Create(...) factory method
│   - Update(...) method
│   - Behavior methods
│   - ToDto() extension method
```

## Feature Folder Pattern
Each feature contains:
- `Create/`, `Update/`, `Delete/`, `List/`, `Get/` — Use cases
- `*Dto.cs` — Request/response DTOs (records)
- `*Service.cs` — Business logic (uses Result<T>)
- `*Endpoint.cs` — Minimal API endpoint
- `*Validator.cs` — FluentValidation rules
- `AGENTS.md` — Feature-specific rules

## Key Patterns
- **Encapsulation:** `private set` on entity properties
- **Factory methods:** `Entity.Create(...)` for domain invariants
- **Update methods:** `entity.Update(...)` for state changes
- **Result<T>:** Business errors return Result, exceptions only for invariant violations
