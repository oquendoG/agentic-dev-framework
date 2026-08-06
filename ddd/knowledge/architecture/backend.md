---
type: ArchitectureGuide
title: Backend Guidelines (DDD)
description: Coding conventions, vertical slice structure, controller style, and resilience in C# for DDD setups.
timestamp: 2026-08-06T15:31:00-05:00
---

# Backend Guidelines (DDD Architecture & Conventions)

> [!IMPORTANT]
> **Layered Precedence Rule:**
> This document and `AGENTS.md` strictly override any recommendation or preference suggested by global or local skills. In case of conflict between a skill's suggestion and this document, **this document takes absolute precedence**.

---

## 1. C# Syntax & Coding Conventions

### Object Instantiation & Use of `var`
- **Strict Rule**: Use explicit type declaration on the left and target-typed `new()` on the right for named types.
- **Exception for `var`**: Use `var` ONLY for anonymous types (e.g., LINQ projections where types cannot be explicitly named).
  ```csharp
  // CORRECT:
  User user = new();
  List<string> names = new();
  var dto = new { Id = 1, Name = "Juan" }; // Allowed ONLY for anonymous types

  // FORBIDDEN:
  var user = new User(); // Do not use var for named types
  User user = new User(); // Do not duplicate class name
  ```

### Pragmatic Pattern Matching & Functional Preference
- Prefer Pattern Matching (`switch` expressions, `is null`, `is not null`, property patterns, relational patterns) where it enhances semantics and readability.
- If imperative structures (`if/else`) are cleaner and easier to read in a specific scenario, mixing both approaches pragmatically is encouraged.
- Prefer collection expressions (`List<User> users = [.. otherUsers]`).
- Use `IEnumerable<T>` over `List<T>` when mutability is not required.
- Mandatory `async/await` for I/O operations.
- **Records**: Always use nominal syntax with explicit `{ get; init; }`. Positional syntax `record Foo(string X)` is BANNED.
- **No Magic Strings**: Place constants in static classes: `Messages.[Entity].[Key]`, `Claims.[Claim]`, `RateLimitPolicies.[Policy]`.
- **Language**: Code entities (classes/methods/properties) in Spanish. Everything else (docs, comments, logs, constants) in English.
- **Null Safety**: Distinguish between `Type` (non-nullable) and `Type?` (nullable). Use `ArgumentNullException.ThrowIfNull()`.
- **Memory & I/O Optimization**: Use `Span<T>`, `ReadOnlySpan<T>`, `Memory<T>` for parsing. Avoid heap allocations. Use `OpenReadStream()` for I/O.

### Exception Handling
- `try-catch` **ONLY** on external boundaries (MSAL, `JSON.parse`, external service calls).
- RFC 7807 ProblemDetails middleware handles exceptions. No try/catch for control flow inside internal services.

---

## 2. Solution Layout (Vertical Slice + DDD)

`Solution.slnx` contains:
- **SharedKernel:** No dependencies. Aggregate root base, entity, domain events dispatcher, and centralized error constants.
- **Application:** Bounded contexts `Features/[BoundedContext]/(Domain, Commands, Queries)`. Ports/interfaces in `Abstractions/`. Deps: `SharedKernel`, `Mediator.Abstractions`, `FluentValidation`. BANNED: EF Core, Npgsql.
- **Infrastructure:** Persistence, external client implementations, Storage (Blob), BackgroundJobs. Deps: `Application` + external libraries.
- **API:** Controllers, Middlewares, Program.cs. Deps: `Infrastructure` + `Mediator.SourceGenerator`.
- **Tests:** xUnit unit testing for queries/commands handlers without real database interaction.

---

## 3. Controller Style & Resilience

- **Fail-fast:** Guard clauses first, early return on failure, happy path last (no nesting).
- Check `result.IsFailed` → return error response → then `return Ok(result.Value)`.
- `Result<T>` for business logic (see [/architecture/backend-ddd.md](/architecture/backend-ddd.md)).
- `DomainException` ONLY for system invariants (never user errors).
- Polly v8 via `Microsoft.Extensions.Resilience`: Standard resilience on HttpClients, custom pipelines (retry+CB+timeout) for other boundaries.

---

## 4. DI & Mediator & Configuration

- Method injection preferred (`[FromServices]`) unless dependency is used by all methods.
- Commands dispatched via Mediator (`ISender`). Queries read directly from `AppDbContext` using `AsNoTracking()`.
- `Mediator.SourceGenerator` performs compile-time dispatch (no runtime reflection).
- Always use Options Pattern with `ValidateOnStart()`. Place options classes in API or Infrastructure layer next to consumer.

---

## 5. Skills Guidance for Backend

The agent must trigger available skills in the environment based on context:
- **C# & .NET**: Trigger available C# language, ASP.NET Core API, and performance skills.
- **Pattern Matching**: Trigger available C# pattern matching skills.
- **Backend Testing**: Trigger test creation skills when authoring tests, and test runner skills for executing/diagnosing `dotnet test`.
- **Token Efficiency**: Trigger token efficiency skills for context optimization.

---

## 6. Sub-files
- DB & EF Core: [/architecture/backend-data.md](/architecture/backend-data.md)
- DDD & CQRS: [/architecture/backend-ddd.md](/architecture/backend-ddd.md)
- Multi-tenancy: [/architecture/backend-mt.md](/architecture/backend-mt.md)
