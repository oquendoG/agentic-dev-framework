## LIBRARIES & TOOLS
- Entity Framework Core (Latest)
- Services with Dependency Injection (Sin MediatR)
- Serilog
- FluentValidation
- Manual mapping con Extension Methods
- JWT Authentication + Role-Based Authorization con Identity Framework
- xUnit, Moq, Bogus, Autofixture, Shouldly para tests con nombres del estilo `MethodName_Should_Result_When_Condition`

### General & Style
- PascalCase (Clases/Métodos), camelCase (Variables/Params), `_camelCase` (Campos privados)
- No usar var solo tipos explicitos `User user = new()`
- Usa `var` SOLO para tipos anónimos
- Prefiere **Programación Funcional** (LINQ, Inmutabilidad, Pure Functions)
- Es válido usar estilo imperativo si mejora la legibilidad o mezclar ambos
- `async/await` obligatorio en I/O.
- **Nullability (Strict)**:
  - Distingue siempre entre `Type` (no nulo) y `Type?` (nullable)
  - Prohibido ignorar advertencias. Usa `is not null`, `??` o `ArgumentNullException.ThrowIfNull()` debes manejar nulos con las mejores practicas posibles

## Arquitecture and pattens
- Vertical slice arquitecture + screamming arquitecture
- Solo si es necesario para tu contexto lee `./arquitecure_backend.md` para más detalles
- No usamos excepciones para lógica de negocio. Usamos el patrón `Result<T>` para ser explícitos sobre el éxito, fallo o no encontrado
- lee `./backend_patterns.md` para ver la implementación exacta de `Result<T>` solo si es necesario y no lo inferiste en tu contexto
- **Pattern**: Endpoint -> Service -> DbContext
- **Services**:
  - Deben implementar interfaz (`I...Service`)
  - Inyectan `DbContext` directamente
  - Retornan `Result<T>`
- Prohibido Repository Pattern
- El Endpoint transforma el `Result<T>` en `ActionResult` usando Pattern Matching
- Cada Feature debe ser autocontenida (`Endpoint`, `Service`, `Validator`, `Dto`). Evita agrupar por tipo (no carpetas `Services` o `Controllers` globales) preferir screamming arquitecture (`CreateUser/CreateUserService.cs`)
- Usa nombres largos y descriptivos que incluyan el verbo y la entidad

## Estilo de DTOs (Records con Init)
- Preferimos inicializadores de objetos para mayor claridad, pero mantenemos la inmutabilidad
- Propiedades `public type Name { get; init; }`

### Data & EF Core
- Para PKs Usa SIEMPRE el tipo `Ulid` (C#) mapeado a `char(26)` (PostgreSQL)
- Para datos únicos (ej: Cédula, Email), usa SIEMPRE un `Unique Index` en la base de datos. NUNCA los uses como Primary Key
- En registros de personas, aplica lógica **"Find or Create"**. Nunca asumas que el registro no existe; búscalo primero por su llave de negocio (Cédula)
- **No Magic Strings**:
  - **Prohibido**: Usar strings literales para mensajes de error, roles o claves de configuración dentro de la lógica de negocio (ej: `return Result.Error("Usuario no encontrado")`)
  - **Obligatorio**: Usa clases estáticas de constantes organizadas por contexto en `Domain/Constants` (ej: `Messages.User.NotFound`)
  Si encuentras valores asi en alguna parte de la aplicación arreglalos
- Usamos **Proyección Directa** (`.Select`) para mapear de Entidad a DTO en la base de datos (SQL eficiente) y evitar traer datos innecesarios.
- `AsNoTracking()` obligatorio para lecturas
- Usar join solo si es necesario, prefiere carga diligente con `AsSplitQuery()` para mejorar la performance y evitar problemas de N+1

## Métodos de extensión modernos
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
```