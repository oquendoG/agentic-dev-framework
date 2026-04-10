## LIBRARIES & TOOLS
- **ORM**: Entity Framework Core (Latest).
- **Communication**: Services with Dependency Injection (Sin MediatR).
- **Logging**: Serilog.
- **Validation**: FluentValidation.
- **Mapping**: **Manual** con Extension Methods.
- **Security**: JWT Authentication + Role-Based Authorization con Identity Framework.

### General & Style
- **Naming**: PascalCase (Clases/Métodos), camelCase (Variables/Params), `_camelCase` (Campos privados)
- **Typing**:
  - **Lado Izquierdo**: SIEMPRE explícito (ej: `User user`, NO `var user`)
  - **Lado Derecho**: Usa **Target-typed new** (`= new();`) si el tipo es evidente
  - **Excepción**: Usa `var` SOLO para tipos anónimos
- **Paradigm**: Prefiere **Programación Funcional** (LINQ, Inmutabilidad, Pure Functions)
  - *Nota*: Es válido usar estilo imperativo si mejora la legibilidad o mezclar ambos
- **Async**: `async/await` obligatorio en I/O.
- **Nullability (Strict)**:
  - **Definitions**: Distingue siempre entre `Type` (no nulo) y `Type?` (nullable)
  - **Handling**: Prohibido ignorar advertencias. Usa `is not null`, `??` o `ArgumentNullException.ThrowIfNull()` debes manejar nulos con las mejroes practicas posibles

## Estilo de DTOs (Records con Init)

**Contexto**: Preferimos inicializadores de objetos para mayor claridad, pero mantenemos la inmutabilidad.
**Evitar**: Positional records con constructores largos (ej: `public record User(string a, string b...)`).
**Imitar**: Propiedades `public type Name { get; init; }`.

### Data & EF Core
- **Primary Keys**: Usa SIEMPRE el tipo `Ulid` (C#) mapeado a `char(26)` (PostgreSQL)
- **Business Keys**: Para datos únicos (ej: Cédula, Email), usa SIEMPRE un `Unique Index` en la base de datos. NUNCA los uses como Primary Key.
- **Duplication Strategy**: En registros de personas (Votantes), aplica lógica **"Find or Create"**. Nunca asumas que el registro no existe; búscalo primero por su llave de negocio (Cédula)
- **Configuration**: Usa `IEntityTypeConfiguration<T>` separado
- **No Magic Strings**:
  - **Prohibido**: Usar strings literales para mensajes de error, roles o claves de configuración dentro de la lógica de negocio (ej: `return Result.Error("Usuario no encontrado")`)
  - **Obligatorio**: Usa clases estáticas de constantes organizadas por contexto en `Domain/Constants` (ej: `Messages.Voter.NotFound`). Si encuentras valores asi en alguna parte de la aplicación arreglalos
- Usamos **Proyección Directa** (`.Select`) para mapear de Entidad a DTO en la base de datos (SQL eficiente) y evitar traer datos innecesarios.

### API & Architecture Flow
- **Pattern**: Endpoint -> Service -> DbContext.
- **Endpoints**: Los archivos terminados en `Endpoint.cs` son **Controllers** (`[ApiController]`) con **UN solo método público** (Single Responsibility)
- **Services**:
  - Deben implementar interfaz (`I...Service`)
  - Inyectan `DbContext` directamente
  - Retornan `Result<T>`
- **No Repositories**: Prohibido Repository Pattern
- **Response**: El Endpoint transforma el `Result<T>` en `ActionResult` usando Pattern Matching.
- **values**: Evita valores hardcoded en su lugar usa clases estaticas con constantes para los valores, usa nombres significativos para las clases

### Data & EF Core
- **Configuration**: Usa `IEntityTypeConfiguration<T>` en archivos separados dentro de `Infrastructure/Data/Configurations`
- **Performance**: `AsNoTracking()` obligatorio para lecturas
- **Mapping**: Mapeo manual usando `.Select()` (proyecciones) en las consultas LINQ dentro del servicio.
- Usar join solo si es necesario, prefiere carga diligente con `AsSplitQuery()` para mejorar la performance

## TESTING RULES
- **Stack**: xUnit, Moq, Bogus, Autofixture, Shouldly
- **Naming**: `MethodName_Should_Result_When_Condition`
- **Prioridad**: Tests de integración para la capa `Features`

## PREFERENCIAS DE CÓDIGO

- **Backend**: Cada Feature debe ser autocontenida (`Endpoint`, `Service`, `Validator`, `Dto`). Evita agrupar por tipo (no carpetas `Services` o `Controllers` globales)
- **Nombres**: Usa nombres largos y descriptivos que incluyan el verbo y la entidad (ej: `RegistrarVotanteService.cs` en lugar de `VotanteService.cs` si el servicio solo registra). *Nota: En este proyecto usamos servicios por entidad (`VotanteService`) que agrupan métodos, pero el archivo del Endpoint sí debe ser específico (`RegistrarVotanteEndpoint`)*

## 9. Métodos de extensión modernos
Desde C# 14 podemos usar la palabra extensión para hacer más sencilla la creación de los métodos de extensión
```csharp
public static class ExtensoresString
{
    extension (string str)
    {
        public bool EsPalindromo()
        {
            //implementación
        }
    }
}

## Custom Result Pattern (Manejo Funcional de Errores)

**Contexto**: No usamos excepciones para lógica de negocio. Usamos este patrón `Result<T>` para ser explícitos sobre el éxito o fallo.
**Imitar**: El uso de **Pattern Matching** en los controladores para transformar el `Result<T>` en `ActionResult`.

### Definición del Patrón (Core/Common)

(Usa esta implementación exacta, no la modifiques).

```csharp
public interface IResult<out TValue>
{
    bool IsSuccess { get; }
    bool IsFailed { get; }
    TValue? Value { get; }
    IEnumerable<string> ErrorMessages { get; }
}

public abstract class Result<TValue> : IResult<TValue>
{
    public abstract bool IsSuccess { get; }
    public abstract bool IsFailed { get; }
    public virtual TValue? Value => default;
    public virtual IEnumerable<string> ErrorMessages => [];

    public static Result<TValue> Ok(TValue value) => new Ok<TValue>(value);
    public static Result<TValue> NotFound() => new NotFound<TValue>();
    public static Result<TValue> Error(params IEnumerable<string> errorMessages) => new Error<TValue>(errorMessages);
}

public class Ok<TValue>(TValue value) : Result<TValue>
{
    public override bool IsSuccess => true;
    public override bool IsFailed => false;
    public override TValue Value { get; } = value;
}

public class NotFound<TValue> : Result<TValue>
{
    public override bool IsSuccess => false;
    public override bool IsFailed => true;
}

public class Error<TValue> : Result<TValue>
{
    public override bool IsSuccess => false;
    public override bool IsFailed => true;
    public bool HasErrors => _errorMessages.Count != 0;

    private readonly List<string> _errorMessages = [];

    public override IEnumerable<string> ErrorMessages => _errorMessages;

    public Error(params IEnumerable<string> errorMessages)
    {
        if (errorMessages is not null)
        {
            _errorMessages.AddRange(errorMessages);
        }
    }

    public Error<TValue> AddError(string errorMessage)
    {
        _errorMessages.Add(errorMessage);
        return this;
    }
}

public static class ErrorExtensions
{
    public static string GetAllErrorsAsString<TValue>(this Error<TValue> error, string separator = "; ")
    {
        return string.Join(separator, error.ErrorMessages);
    }
}

```

Solo si es necesario para tu contexto lee `./arquitecure_backend.md` para más detalles.