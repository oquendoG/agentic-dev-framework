## Fronted preferences
- Angular Signals (WritableSignal, computed, effect, input, output, reactive forms)
- preferir # para campos privados en lugar de `private`
- Siempre usar la palabra reservada `public` para campos publicos

- PrimeNG 21 (usando la nueva API de estilos si aplica)
    - Usar un solo `<p-toast />` en `app.html` (componente raíz). No agregar `<p-toast>` en templates de componentes individuales.

- Angular Router (Standalone)
- Angular HttpClient (Functional Interceptors)
- Usa observables cuando sea necesario, pero usa signals por encima de observables, prefiere legibilidad.

- No usar `try-catch`, usa manejo de excepciones globales con las mejores practicas.
- **MessageService**: Provisto globalmente en `app.config.ts`. No agregar `providers: [MessageService]` a nivel de componente. Los componentes que necesiten mostrar toasts de éxito pueden inyectar la instancia global directamente.
- **Forms**: Usar siempre reactive forms y debounce con `valueChanges`
- **Control Flow**: Usa `@if`, `@for`, `@switch`
- **Reactivity**: Usa **Signals** para estado local e `input signals`
- **Components**: `standalone: true` obligatorio
- **Smart/Dumb**: Separa componentes en Contenedores (Lógica/API) y Presentacionales (Inputs/Outputs)
- **Dependency Injection**: Usa la función `inject()` (ej: `private _http = inject(HttpClient);`) en lugar del constructor.
- Los componentes se agrupan por Feature (`features/feature-1/components/...`) y no por tipo técnico, procura seguir el estilo del backend adaptado al frontend (vertical slices + screamming architecture)
- No usar templates ni estilos inline. Siempre separar en archivos `.html` y `.scss`, solo si el componente es muy pequeño y no tiene mucha interacción puedes usar inline
- Usar `type` en lugar de `interface` para modelos de datos y DTOs del frontend (Obligatorio)
- Al usar promesas, si es que por algún motivo se deben usar, siempre usar async/await. No usar `.then()`
- Debes usar Tailwind como primera opción para estilos, css puro solo cuando sea estrictamente necesario y tailwind no tenga la funcionalidad requerida
- Solo si es necesario para tu contexto lee `./arquitecure_frontend.md` para más detalles.
