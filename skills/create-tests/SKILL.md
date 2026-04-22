---
name: create-tests
description: Esta skill permite crear los tests del backend para la o las funcionalidades recien creadas o que indique el usuario, con el fin de garantizar la calidad del código y la funcionalidad del sistema
---

## Librerias
Debes usar las siguientes librerias:
- xunit
- Fixture o bogus (prefiere fixture, usa solo bogus si el requerimiento de datos fake es muy complejo)
- Moq
- Shoudly

## Estructura y Patrones:
- Sigue siempre el patrón AAA (Arrange, Act, Assert).
- Separa cada sección claramente con los comentarios `// Arrange`, `// Act` y `// Assert`
- Los tests deben tener nombres descriptivos y concisos
- Los nombres deben seguir el patrón `MethodName_Should_Result_When_Condition`
- Los nombres deben ser en inglés
- Evita usar abreviaturas o siglas en los nombres de los tests
- Usa `Fact` para tests unitarios y `Theory` para tests parametrizados

## Mocking Y Aislamiento (Moq):
- Aísla la clase bajo prueba (System Under Test - SUT) mockeando todas sus dependencias externas (interfaces)
- Usa `Moq` (ej. `var mockRepository = new Mock<IUserRepository>();`)
- Usa `Setup()` para simular retornos y `Verify()` en la sección de Assert si necesitas comprobar que un método de la dependencia fue llamado correctamente

## Asesciones (Shouldly):
   - ESTRICTAMENTE PROHIBIDO usar `Assert.Equal`, `Assert.True`, etc. de xUnit
   - Usa siempre la sintaxis fluida de `Shouldly`
   - Ejemplos válidos: `resultado.ShouldBe(esperado)`, `lista.ShouldNotBeEmpty()`, `excepcion.Message.ShouldContain("error")`, `objeto.ShouldNotBeNull()`

## Buenas prácticas
- Usa WebApplicationFactory cuando sea necesario
- Usa TestServer cuando sea necesario

Devuelve únicamente el código en C# necesario para la clase de pruebas, incluyendo los `using` correspondientes. El código debe compilar y ser fácil de leer.