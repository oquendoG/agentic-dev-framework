# Agentic Development Framework

A structured, agent-friendly development framework designed for rapid software delivery using AI coding agents under the **Google OKF v0.1** knowledge standard.

This framework is split into two independent architectural boilerplates, allowing you to choose the design pattern that best fits your project scope.

---

## Repository Structure

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

---

## Boilerplate Flavors

### 1. DDD (Domain-Driven Design)
Designed for larger, complex enterprise systems requiring strong domain logic segregation.
* **Backend:** .NET 10, Mediator CQRS, tactical DDD elements (Aggregates, Value Objects, Domain Events, explicit factory patterns, error management via `Result<T>` instead of exceptions).
* **Frontend:** Angular 21, Signals, state management using local `@ngrx/signals` stores, functional guards/interceptors.
* **Storage/Auditing:** Entity Framework Core + Npgsql, Audit.EntityFramework tracking.

### 2. Tradicional (Services & Controllers)
Designed for lightweight, data-centric systems or simpler microservices.
* **Backend:** Thin controllers directly injecting Transactional Services and DbContext (no Mediator, no repository abstractions).
* **Frontend:** Standard standalone Angular 21 components with Signal reactive bindings.
* **Data Access:** Entity Framework Core with direct projections to DTOs.

---

## How to Use

When initializing a new project:

1. **Choose your flavor:** Copy either the `/ddd/` or the `/tradicional/` folder directly to the root of your new project repository.
2. **Setup Workspace Rules:** Ensure the copied `AGENTS.md` is placed at the workspace root, as your AI agent relies on it for loading rules and context.
3. **Configure Project Metadata:** Initialize your project context inside `knowledge/project.md` and document the system status in `knowledge/current_state.md`.
4. **Define Tasks:** Create your task files directly inside `knowledge/tasks/[next-number]-[task-name].md` when starting development milestones.

---

## Workflows and Commands

The `/commands/` directory contains structured Markdown workflows that you can feed or mention to your agent (e.g. `/plan`, `/execute`, `/review` commands):

* `plan.md` — Guides the agent in breaking down tasks into atomic steps inside `knowledge/tasks/`.
* `execute.md` — Enforces execution standards, code validation, and history logging.
* `refactor.md` — Instructions for safe refactoring.
* `review.md` — Performs final architecture linting (strict type checks, builds, no raw try/catch).
* `update-context.md` — Minimizes task lists to save tokens.