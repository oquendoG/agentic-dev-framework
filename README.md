# Agentic Development Framework

---

## English Version

A structured, agent-friendly development framework designed for rapid software delivery using AI coding agents under the **Google OKF v0.1** knowledge standard.

This framework is split into two independent architectural boilerplates, allowing you to choose the design pattern that best fits your project scope.

### Repository Structure

```text
📁 agentic-dev-framework
├── 📁 ddd/                      # Domain-Driven Design Boilerplate (Vertical Slice + Tactical DDD)
│   ├── AGENTS.md                # Agent instructions & context loader for DDD
│   └── 📁 knowledge/            # OKF v0.1 Knowledge Bundle
│
├── 📁 tradicional/              # Traditional Boilerplate (MVC / Services + Controllers)
│   ├── AGENTS.md                # Agent instructions & context loader for MVC
│   └── 📁 knowledge/            # OKF v0.1 Knowledge Bundle
│
└── 📁 commands/                 # Agent workflows (.md commands for plan, execute, review, etc.)
```

### Boilerplate Flavors

#### 1. DDD (Domain-Driven Design)
Designed for larger, complex enterprise systems requiring strong domain logic segregation.
* **Backend:** .NET 10, Mediator CQRS, tactical DDD elements (Aggregates, Value Objects, Domain Events, explicit factory patterns, error management via `Result<T>` instead of exceptions).
* **Frontend:** Angular 21, Signals, state management using local `@ngrx/signals` stores, functional guards/interceptors.
* **Storage/Auditing:** Entity Framework Core + Npgsql, Audit.EntityFramework tracking.

#### 2. Tradicional (Services & Controllers)
Designed for lightweight, data-centric systems or simpler microservices.
* **Backend:** Thin controllers directly injecting Transactional Services and DbContext (no Mediator, no repository abstractions).
* **Frontend:** Standard standalone Angular 21 components with Signal reactive bindings.
* **Data Access:** Entity Framework Core with direct projections to DTOs.

### How to Use

When initializing a new project:

1. **Choose your flavor:** Copy either the `/ddd/` or the `/tradicional/` folder directly to the root of your new project repository.
2. **Setup Workspace Rules:** Ensure the copied `AGENTS.md` is placed at the workspace root, as your AI agent relies on it for loading rules and context.
3. **Configure Project Metadata:** Initialize your project context inside `knowledge/project.md` and document the system status in `knowledge/current_state.md`.
4. **Define Tasks:** Create your task files directly inside `knowledge/tasks/[next-number]-[task-name].md` when starting development milestones.

### Workflows and Commands

The `/commands/` directory contains structured Markdown workflows that you can feed or mention to your agent (e.g. `/plan`, `/execute`, `/review` commands):

* `plan.md` — Guides the agent in breaking down tasks into atomic steps inside `knowledge/tasks/`.
* `execute.md` — Enforces execution standards, code validation, and history logging.
* `refactor.md` — Instructions for safe refactoring.
* `review.md` — Performs final architecture lints (strict type checks, builds, no raw try/catch).
* `update-context.md` — Minimizes task lists to save tokens.

---

## Versión en Español

Un entorno de desarrollo estructurado y optimizado para agentes de IA, diseñado para la entrega rápida de software utilizando agentes autónomos bajo el estándar de conocimiento **Google OKF v0.1**.

Este framework está dividido en dos boilerplates de arquitectura independientes, permitiéndote elegir el patrón de diseño que mejor se adapte al alcance de tu proyecto.

### Estructura del Repositorio

```text
📁 agentic-dev-framework
├── 📁 ddd/                      # Plantilla de Domain-Driven Design (Vertical Slice + DDD Táctico)
│   ├── AGENTS.md                # Instrucciones del agente y cargador de contexto para DDD
│   └── 📁 knowledge/            # Bundle de conocimiento OKF v0.1
│
├── 📁 tradicional/              # Plantilla Tradicional (MVC / Servicios + Controladores)
│   ├── AGENTS.md                # Instrucciones del agente y cargador de contexto para MVC
│   └── 📁 knowledge/            # Bundle de conocimiento OKF v0.1
│
└── 📁 commands/                 # Workflows del agente (comandos .md para planificar, ejecutar, revisar, etc.)
```

### Variantes de Arquitectura

#### 1. DDD (Domain-Driven Design)
Diseñado para sistemas empresariales complejos y de gran tamaño que requieren una estricta segregación de la lógica de dominio.
* **Backend:** .NET 10, Mediator CQRS, elementos de DDD táctico (Agregados, Objetos de Valor, Eventos de Dominio, patrones de factoría explícitos, gestión de errores con `Result<T>` en lugar de excepciones).
* **Frontend:** Angular 21, Signals, gestión de estado con almacenes locales `@ngrx/signals`, guards e interceptores funcionales.
* **Persistencia/Auditoría:** Entity Framework Core + Npgsql, seguimiento con Audit.EntityFramework.

#### 2. Tradicional (Servicios y Controladores)
Diseñado para sistemas ligeros orientados a datos o microservicios sencillos.
* **Backend:** Controladores delgados que inyectan directamente Servicios Transaccionales y el DbContext (sin Mediator, sin abstracciones de repositorio).
* **Frontend:** Componentes standalone estándar de Angular 21 con bindings reactivos basados en Signals.
* **Acceso a Datos:** Entity Framework Core con proyecciones directas a DTOs.

### Instrucciones de Uso

Al iniciar un nuevo proyecto:

1. **Elige la arquitectura:** Copia la carpeta `/ddd/` o la carpeta `/tradicional/` directamente en la raíz de tu nuevo repositorio de proyecto.
2. **Configura las Reglas del Workspace:** Asegúrate de que el archivo `AGENTS.md` copiado quede en la raíz del workspace, ya que tu agente de IA lo necesita para cargar reglas y contexto.
3. **Configura los Metadatos del Proyecto:** Inicializa el contexto de tu proyecto en `knowledge/project.md` y documenta el estado del sistema en `knowledge/current_state.md`.
4. **Define las Tareas:** Crea tus archivos de tareas directamente dentro de `knowledge/tasks/[siguiente-numero]-[nombre-tarea].md` al iniciar tus hitos de desarrollo.

### Workflows y Comandos

El directorio `/commands/` contiene flujos de trabajo estructurados en Markdown que puedes pasar o mencionar a tu agente (ej. comandos `/plan`, `/execute`, `/review`):

* `plan.md` — Guía al agente para desglosar tareas en pasos atómicos dentro de `knowledge/tasks/`.
* `execute.md` — Aplica estándares de ejecución, validación de código y registro de historial.
* `refactor.md` — Instrucciones para refactorización segura.
* `review.md` — Realiza la revisión final de arquitectura (validación estricta de tipos, compilación, control de try/catch).
* `update-context.md` — Minimiza las listas de tareas para ahorrar consumo de tokens.