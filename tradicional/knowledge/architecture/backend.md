---
type: ArchitectureGuide
title: Traditional Backend Guidelines
description: Coding style, vertical slice, EF Core data access, and modern C# conventions for traditional MVC setups.
timestamp: 2026-07-10T10:50:54-05:00
---

When backend skill instructions conflict with this document, follow this document — not the skill. All other skill instructions still apply.

## Libraries & Tools
- Entity Framework Core (Latest)
- Services with Dependency Injection (No MediatR / lightweight direct DI)
- Serilog
- FluentValidation
- Manual mapping with Extension Methods

## General & Style
- **Naming:** PascalCase classes/methods, camelCase vars/params, `_camelCase` private fields.
- **Explicit types:** No `var` except for anonymous types (e.g. `User user = new()`).
- **Collection expressions:** Use `[.. items]` over `.ToList()`. Prefer `IEnumerable<T>` over `List<T>` when immutable.
- **Pattern matching:** Use `is not null` and pattern matching. Mandatory `async`/`await` for I/O.
- **Null Safety:** Distinguish between `Type` (non-nullable) and `Type?` (nullable). Use `ArgumentNullException.ThrowIfNull()`.

## Architecture & Patterns
- **Vertical Slice:** Each feature is self-contained (`Endpoint`, `Service`, `Validator`, `Dto`). Avoid global grouping (read [/architecture/arquitecture-backend.md](/architecture/arquitecture-backend.md) for details).
- **Result Pattern:** No exceptions for business logic. Use `Result<T>` for success, fail, or not found results (read [/architecture/backend-patterns.md](/architecture/backend-patterns.md) for implementation details).
- **Pattern:** Endpoint -> Service -> DbContext.
- **Services:** Implement interface (`I...Service`), inject `DbContext` directly, return `Result<T>`. BANNED: Repository Pattern.
- **DI:** Method injection preferred (`[FromServices]`) unless the dependency is used in all methods.

## DTOs (Records with Init)
- Prefer object initializers for clarity while maintaining immutability:
  ```csharp
  public record UserDto
  {
      public required string Email { get; init; }
  }
  ```

## Data & EF Core
- **PKs:** Use `Ulid` (C#) mapped to `char(26)` (PostgreSQL).
- **Business Keys:** Apply DB Unique Indexes (National ID, Email), never Primary Keys.
- **Find or Create:** In person registries, search by business key (Cédula/Email) before inserting.
- **No Magic Strings:** Error messages, roles, configs live in static classes (e.g. `Messages.User.NotFound`).
- **Performance:** Use `AsNoTracking()` for reads. Eager loading with `.AsSplitQuery()` for N+1 issues. Direct projection with `.Select()` to map to DTOs.

## Extension Methods
Since C# 14, extension syntax can be used for simpler declaration:
```csharp
public static class StringExtensions
{
    extension (string str)
    {
        public bool EsPalindromo()
        {
            // implementation
        }
    }
}
```
