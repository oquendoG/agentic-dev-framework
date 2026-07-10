---
type: ArchitectureGuide
title: Backend Guidelines (DDD)
description: Coding conventions, vertical slice structure, controller style, and resilience in C# for DDD setups.
timestamp: 2026-07-10T10:50:54-05:00
---

## C# Conventions
- **Naming:** PascalCase classes/methods, camelCase vars/params, _camelCase private fields.
- **Explicit types:** No `var` except for anonymous types (e.g. `User user = new()`).
- **Collection expressions:** Use `[.. items]` over `.ToList()`. Prefer `IEnumerable<T>` over `List<T>` when immutable.
- **Pattern matching:** Use `is not null` and pattern matching. Mandatory `async`/`await` for I/O.
- **Records:** Always use nominal syntax with explicit `{ get; init; }`. Positional syntax `record Foo(string X)` is BANNED.
- **No magic strings:** Place constants in static classes: `Messages.[Entity].[Key]`, `Claims.[Claim]`, `RateLimitPolicies.[Policy]`.
- **Language:** Code entities (classes/methods/properties) in Spanish. Everything else (docs, comments, logs, constants) in English.
- **Null Safety:** Distinguish between `Type` (non-nullable) and `Type?` (nullable). Use `ArgumentNullException.ThrowIfNull()`.
- **Memory Optimization:** Use `Span<T>`, `ReadOnlySpan<T>`, `Memory<T>` for parsing and formatting. Avoid `string.Split`, `Substring`, `string.Join` when allocating on heap.
- **I/O Optimization:** Avoid streaming to `MemoryStream` for read/upload; use `OpenReadStream()` and stream directly with pooled/stackalloc buffers.

## Solution Layout (Vertical Slice + DDD)
`Solution.slnx` contains:
- **SharedKernel:** No dependencies. Aggregate root base, entity, domain events dispatcher, and centralized error constants.
- **Application:** Bounded contexts `Features/[BoundedContext]/(Domain, Commands, Queries)`. Ports/interfaces in `Abstractions/`. Deps: `SharedKernel`, `Mediator.Abstractions`, `FluentValidation`. BANNED: EF Core, Npgsql.
- **Infrastructure:** Persistence, external client implementations, Storage (Blob), BackgroundJobs. Deps: `Application` + external libraries.
- **API:** Controllers, Middlewares, Program.cs. Deps: `Infrastructure` + `Mediator.SourceGenerator`.
- **Tests:** xUnit unit testing for queries/commands handlers without real database interaction.

## Controller Style
- **Fail-fast:** Guard clauses first, early return on failure, happy path last (no nesting).
- Check `result.IsFailed` → return error response → then `return Ok(result.Value)`.

## Error & Resilience
- `Result<T>` for business logic (see [/architecture/backend-ddd.md](/architecture/backend-ddd.md)).
- `DomainException` ONLY for system invariants (never user errors).
- RFC 7807 ProblemDetails middleware handles exceptions. No try/catch for control flow.
- Polly v8 via `Microsoft.Extensions.Resilience`: Standard resilience on HttpClients, custom pipelines (retry+CB+timeout) for other boundaries.

## DI & Mediator
- Method injection preferred (`[FromServices]`) unless dependency is used by all methods.
- Commands dispatched via Mediator (`ISender`). Queries read directly from `AppDbContext` using `AsNoTracking()`.
- Mediator.SourceGenerator performs compile-time dispatch (no runtime reflection).

## Configuration (appsettings)
- Always use Options Pattern: `services.AddOptions<TOptions>().BindConfiguration("Section").ValidateDataAnnotations().ValidateOnStart()`.
- Place options classes in API or Infrastructure layer next to the consumer. Validate via FluentValidation/DataAnnotations.

## Local Dev Emulators
- **Blob Storage:** Azurite (Docker) at `UseDevelopmentStorage=true`. Real blob storage in prod, toggled via environment.
- **Email:** Mailpit (Docker, port 1025/8025). Concrete implementation injected via Application interfaces.

## Sub-files
- DB & EF Core: [/architecture/backend-data.md](/architecture/backend-data.md)
- DDD & CQRS: [/architecture/backend-ddd.md](/architecture/backend-ddd.md)
- Multi-tenancy: [/architecture/backend-mt.md](/architecture/backend-mt.md)
