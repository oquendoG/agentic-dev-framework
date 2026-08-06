# Backend Testing Patterns & Guidelines (TDD)

Guidelines and code patterns for authoring unit and integration tests using xUnit, Moq, Shouldly, and Fixture/Bogus under the TDD (Red-Green-Refactor) workflow.

> [!IMPORTANT]
> **Layered Precedence Rule:**
> This document and `AGENTS.md` strictly override any recommendation or preference suggested by global or local testing skills (such as `create-tests` or `run-tests`). In case of conflict, **this document takes absolute precedence**.

---

## 1. Core Testing Stack & Conventions

| Purpose | Framework / Library | Syntax Rule |
| :--- | :--- | :--- |
| **Test Runner** | xUnit | Use `[Fact]` for single unit tests, `[Theory]` for parameterized tests. |
| **Assertions** | **Shouldly** | **FORBIDDEN**: xUnit `Assert.Equal`, `Assert.True`. **MANDATORY**: `result.ShouldBe(expected)`, `result.ShouldNotBeNull()`. |
| **Mocking** | Moq | Mock interfaces using `Mock<IType>`. Use `Setup()` for return values and `Verify()` for call assertions. |
| **Fake Data** | AutoFixture / Bogus | Prefer AutoFixture for automatic object graphs; use Bogus for complex domain-specific fake data. |

---

## 2. Naming & AAA Pattern

- **Test Method Naming**: `MethodName_Should_ExpectedBehavior_When_Condition` in English.
  - Example: `ListarEstudiantes_Should_ReturnSuccessResult_When_EstudiantesExist`
  - Example: `CrearEstudiante_Should_ReturnValidationError_When_EmailIsInvalid`

- **AAA Structure**: Every test MUST explicitly separate three sections using comments:
  ```csharp
  // Arrange
  // Act
  // Assert
  ```

---

## 3. Testing Patterns for `Result<T>`

### Testing Success (`Ok<T>`)
```csharp
[Fact]
public async Task GetById_Should_ReturnOkResult_When_EntityExists()
{
    // Arrange
    Ulid entityId = Ulid.NewUlid();
    Estudiante estudianteFake = new() { Id = entityId, Nombre = "Carlos" };
    _mockRepo.Setup(r => r.GetByIdAsync(entityId, It.IsAny<CancellationToken>()))
             .ReturnsAsync(estudianteFake);

    // Act
    Result<EstudianteDto> result = await _service.GetByIdAsync(entityId, CancellationToken.None);

    // Assert
    result.IsSuccess.ShouldBeTrue();
    result.IsFailed.ShouldBeFalse();
    result.Value.ShouldNotBeNull();
    result.Value.Nombre.ShouldBe("Carlos");
}
```

### Testing Failure (`NotFound<T>` or `Error<T>`)
```csharp
[Fact]
public async Task GetById_Should_ReturnNotFoundResult_When_EntityDoesNotExist()
{
    // Arrange
    Ulid entityId = Ulid.NewUlid();
    _mockRepo.Setup(r => r.GetByIdAsync(entityId, It.IsAny<CancellationToken>()))
             .ReturnsAsync((Estudiante?)null);

    // Act
    Result<EstudianteDto> result = await _service.GetByIdAsync(entityId, CancellationToken.None);

    // Assert
    result.IsSuccess.ShouldBeFalse();
    result.IsFailed.ShouldBeTrue();
    result.ErrorMessages.ShouldContain("Entity not found");
}
```

---

## 4. Mocking `DbContext` / In-Memory Database

When testing services that inject `DbContext` directly without repositories:
- Use EF Core **InMemory Provider** or **SQLite In-Memory** for fast, isolated DbContext integration tests.
```csharp
public DbContextOptions<ApplicationDbContext> CreateInMemoryOptions()
{
    return new DbContextOptionsBuilder<ApplicationDbContext>()
        .UseInMemoryDatabase(databaseName: Guid.NewGuid().ToString())
        .Options;
}
```

---

## 5. TDD Red-Green-Refactor Checklist

1. **Red**: Write test before writing service code. Verify test fails by running `dotnet test`.
2. **Green**: Implement minimal code in service/endpoint until `dotnet test` passes.
3. **Refactor**: Clean up syntax (`Tipo var = new()`, pattern matching) while tests remain green.
