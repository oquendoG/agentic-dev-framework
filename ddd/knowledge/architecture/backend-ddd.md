---
type: ArchitectureGuide
title: Backend DDD Guidelines
description: Tactical Domain-Driven Design (DDD) rules, Value Objects, Aggregates, Result pattern, and CQRS in C#.
timestamp: 2026-08-06T15:31:00-05:00
---

# Backend Tactical DDD Guidelines

> [!IMPORTANT]
> **Layered Precedence Rule:**
> This document and `AGENTS.md` strictly override any recommendation or preference suggested by global or local domain modeling skills (such as `dotnet-csharp/references/domain-modeling.md`).
> In case of conflict, **this document takes absolute precedence**:
> 1. **Result<T> over Exceptions**: Business rules DO NOT throw exceptions. Return `Result<T>` (`Ok`, `NotFound`, `Error`, `ValidationError`).
> 2. **NO Repository Pattern**: Command Handlers inject `DbContext` directly. Repositories are strictly BANNED.
> 3. **Ulid for Aggregate IDs**: Use `Ulid` via `Entity<Ulid>` and `Aggregate` base class (never raw `Guid`).
> 4. **Compile-time Mediator**: Use `Mediator.SourceGenerator`/`Mediator.Abstractions` (never runtime `MediatR`).
> 5. **Nominal Records**: Always explicit `{ get; init; }`. Positional `record Foo(...)` syntax is BANNED.

---

## 1. Entity & Aggregate (SharedKernel)
- `Entity<TId>` : `IEquatable<Entity<TId>>` — identity by `Id` + type, immutable `Id { get; init; }`.
- `Aggregate` : `Entity<Ulid>` — adds `_domainEvents` list, `AddDomainEvent`, `GetDomainEvents`, `ClearDomainEvents`.
- Aggregates inherit `Aggregate`. Private constructor. Factory `public static Result<T> Crear(...)` is the sole entry point.

---

## 2. Result<T> (Exact implementation)
- `IResult<TValue>`: `{ IsSuccess, IsFailed, Value?, ErrorMessages }`.
- `Result<TValue>` is abstract, derived into: `Ok<TValue>`, `NotFound<TValue>`, `Error<TValue>`, `ValidationError<TValue>`.
- Factory methods: `Result<TValue>.Ok(val)`, `.NotFound()`, `.Error(errors[])`.
- `ValidationError<TValue>` contains `FieldErrors: IReadOnlyDictionary<string, string[]>` (field → messages). Grouped by camelCase name by `ValidationBehavior` on FluentValidation failure.

---

## 3. Guard vs DomainGuards (SharedKernel)

| Feature | `Guard` (SharedKernel/Guard.cs) | `DomainGuards` (SharedKernel/DomainGuards.cs) |
|---|---|---|
| **Used in** | Value Object `init` accessors | Aggregate/Entity `Crear()` factory |
| **Prefix** | `Ensure*` | `Require*` |
| **Returns** | Value (throws `ArgumentException` on fail) | `Result<T>` |
| **Meaning** | Programming bug (FluentValidation missed it) | Expected business validation outcome |
| **Caller** | `init => field = value.EnsureNotNull(nameof(Prop))` | `result.IsFailed → return Error(...)` |

Symmetric check methods: `NotNull`/`RequireNotNull`, `MinLength`/`MaxLength`/`ExactLength`, `Matches(regex)`, `GreaterThan`/`GreaterThanOrEqual`, `Range`, `NotPast`/`NotFuture`.

---

## 4. Value Objects
- `sealed record`, private constructor, immutable (`init` only, no setters).
- Validation via `Guard` extension methods directly in `init` accessors:
  ```csharp
  public string Calle { get; init => field = value.EnsureNotNull(nameof(Calle)).EnsureMaxLength(100, nameof(Calle)); }
  ```
- Instantiate via Object Initializer: `new() { Prop = val }`.
- All behavior/characteristics via extension methods in `[VO]Extensions` class.

---

## 5. Entities (Non-Aggregate)
- Inherit `Entity<TId>`. Private constructor (EF parameterless `private ClassName() : base(default) { }`).
- Factory `public static Result<T> Crear(...)` validates with `DomainGuards` → returns `Result<T>`.
- Properties use `private init` for immutability. Mutable state via behavior methods with `private set`.
- No Domain Events (only Aggregates raise them).

---

## 6. Aggregates
- Inherit `Aggregate` (SharedKernel). Private constructor.
- Invariants validated via `DomainGuards` → returns `Result<string>`.
- Factory `public static Result<T> Crear(...)` returns `Result<T>.Error(...)` on failure.
- Behavior methods return `Result` for business failures, throw `DomainException` for invariant breaches.
- Domain Events: `AddDomainEvent(...)`, dispatched after `SaveChangesAsync` via `dispatcher.DispatchAsync(aggregate, ct)`.

---

## 7. FluentValidation (Application layer)
- One `AbstractValidator<TCommand>` per command/query, colocated in the same folder.
- `ValidationBehavior` interceptor validates format, nulls, and length constraints.
- On failure: returns `ValidationError<T>` with `FieldErrors` — never throws.

---

## 8. ResultExtensions (API layer)
- `result.ToErrorActionResult(controller, title)` maps Result subtypes to HTTP responses:
  - `ValidationError<T>` → 422 `ValidationProblemDetails` (field-level errors).
  - `NotFound<T>` → 404 `ProblemDetails`.
  - `Error<T>` → 400 `ProblemDetails`.

---

## 9. Domain Events
- `sealed record` nominal : `IDomainEvent`, `INotification`. Past-tense naming (e.g. `DocumentoRadicadoEvent`).
- Raised by Aggregate, dispatched after `SaveChanges`.

---

## 10. Commands & Queries
- **Commands:** nominal records returning ID or Unit. Validated via FluentValidation. Handlers inject DbContext directly (NO Repository Pattern).
- **Queries:** nominal records bypassing Aggregates. Use direct DbContext + `AsNoTracking()` + `.Select()` projection. Never dispatch mediator queries within a query/command handler.

---

## 11. STRICT PROHIBITIONS
- DO NOT use Repository Pattern (inject DbContext).
- DO NOT throw exceptions for business rules (use `Result.Error()`).
- DO NOT expose public constructors on VOs/Aggregates/Entities.
- DO NOT place business logic in Controllers.
- DO NOT query aggregates directly in Queries (use projection).
- DO NOT return full Aggregates from Commands (return ID/Unit).
- DO NOT use MediatR (use `Mediator.SourceGenerator`/`Mediator.Abstractions`).
