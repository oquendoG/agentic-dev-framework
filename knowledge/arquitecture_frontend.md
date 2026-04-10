## Vertical Slice Architecture (Frontend)
Regla: Sigue el principio de Screaming Architecture. Cada feature debe ser autocontenida y declarar explícitamente sus dependencias.

Features: Contienen TODO lo necesario para funcionar (Services, Guards, Interceptors, Models, Components).
Core: Solo código base o utilidades compartidas entre múltiples features (ej. Configuración de API global). No debe contener lógica de negocio específica de un feature.
Shared: Solo estilos compartidos (SCSS) y componentes de UI "tontos" (sin lógica de negocio) reutilizables.

src/app/
├── core/               # Singleton services globales, config
├── shared/             # UI Components, Pipes, Directives compartidos
├── features/
│   ├── auth/           # Feature autocontenida
│   │   ├── services/   # AuthService (Pertenece al feature)
│   │   ├── guards/     # AuthGuard (Pertenece al feature)
│   │   ├── interceptors/ # AuthInterceptor
│   │   ├── components/ # O pages/
│   │   ├── auth.models.ts
│   │   └── auth.routes.ts
│   └── dashboard/