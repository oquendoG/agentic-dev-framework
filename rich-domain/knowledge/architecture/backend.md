# Backend Architecture & Conventions

> [!IMPORTANT]
> **Layered Precedence Rule:**
> This document and `AGENTS.md` strictly override any recommendation or preference suggested by global or local skills. In case of conflict between a skill's suggestion and this document, **this document takes absolute precedence**.

---

## 1. Libraries & Core Stack
- Entity Framework Core (Latest)
- Services with Dependency Injection (No MediatR in Traditional / Rich-Domain)
- Serilog
- Manual mapping with Extension Methods
- Problem Details for API error responses

---

## 2. C# Syntax & Coding Conventions

### Object Instantiation & Use of `var`
- **Strict Rule**: Explicit type declaration on the left, target-typed `new()` on the right for named types.
- **Exception for `var`**: Use `var` ONLY for anonymous types (e.g., LINQ projections where types cannot be explicitly named).
  ```csharp
  // CORRECT:
  Person person = new();
  List<string> names = new();
  Dictionary<int, Order> orders = new();
  var dto = new { Id = 1, Name = "Juan" }; // Allowed ONLY for anonymous types

  // FORBIDDEN:
  var person = new Person(); // Do not use var for named types
  Person person = new Person(); // Do not duplicate class name
  ```

### Pragmatic Pattern Matching & Functional Preference
- Prefer Pattern Matching (`switch` expressions, `is null`, `is not null`, property patterns, relational patterns) where it enhances semantics and readability.
- If imperative structures (`if/else`) are cleaner and easier to read in a specific scenario, mixing both approaches pragmatically is encouraged.
- Prefer collection expressions (`List<User> users = [.. otherUsers]`).
- Use Raw string literals for complex strings.
- Use `IEnumerable<T>` over `List<T>` when mutability is not required.
- Mandatory `async/await` for I/O operations.
- **Strict Nullability**: Distinguish between `Type` (non-null) and `Type?` (nullable). Handle nulls with `is not null`, `??`, or `ArgumentNullException.ThrowIfNull()`.

### Exception Handling
- `try-catch` **ONLY** on external boundaries (MSAL, `JSON.parse`, external service calls).
- NEVER use `try-catch` inside internal services or HTTP calls — global exception middleware and handlers cover those automatically.

---

## 3. Architecture & Design Patterns (Rich Domain Model)

- **Vertical Slice Architecture**: Entities control their own state (`private set`) and provide domain behavior.
- **Entity Rules**:
  - **Properties**: Use `private set` for encapsulation.
  - **Factory Methods**: `Entity.Create(...)` enforces domain invariants. Returns entity or throws `ArgumentException`.
  - **Behavior Methods**: `entity.Method()` executes domain logic, returning `Result<T>` for business outcomes.
  - **Update Methods**: `entity.Update(...)` mutates state cleanly.
  - **Guards**: `ArgumentException.ThrowIfNullOrWhiteSpace(...)` for input validation.
- **Layer Flow**: `Endpoint -> Service -> Rich Entity -> DbContext`.
- Prefer method injection over constructor injection.

---

## 4. Data & EF Core Rules

- **Primary Keys**: Use `Ulid` (C#) mapped to `char(26)` (PostgreSQL/SQL).
- **Unique Business Keys**: Use `Unique Index` in database. NEVER use as Primary Keys.
- **Find or Create Strategy**: For person/entity records, search by business key before creating.
- **No Magic Strings**: Use static constant classes in `Domain/Constants`.
- **Direct Projection**: Use `.Select()` to project directly from Entity to DTO.
- **Performance**: Mandatory `AsNoTracking()` for read queries. Use `AsSplitQuery()` for eager loading.

---

## 5. Skills Guidance for Backend

The agent must trigger available skills in the environment based on context:
- **C# & .NET**: Trigger available C# language, ASP.NET Core API, and performance skills.
- **Pattern Matching**: Trigger available C# pattern matching skills.
- **Backend Testing**: Trigger test creation skills when authoring tests, and test runner skills for executing/diagnosing `dotnet test`.
- **Token Efficiency**: Trigger token efficiency skills for context optimization.
