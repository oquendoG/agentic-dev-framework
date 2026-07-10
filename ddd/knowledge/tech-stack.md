---
type: TechStack
title: Tech Stack
description: Backend and frontend technology versions, baseline dependencies, and database engines.
timestamp: 2026-07-10T10:50:54-05:00
---

# Technology Stack

## Backend
- .NET 10 LTS + C# 14, ASP.NET Core 10.
- Mediator.SourceGenerator (API) + Mediator.Abstractions (Application).
- EF Core + Npgsql, FluentValidation + FluentValidation.AspNetCore.
- Microsoft.Extensions.Resilience (Polly v8).
- Audit.EntityFramework.Core (AuditDbContext inheritance).
- Local Development: Docker Compose setup.

## Frontend
- Angular 21 (standalone default, no NgModules), TypeScript 5.9.x, Signals + @ngrx/signals.
- pnpm package manager (strict-store, lockfile-only).
- Tailwind CSS / design SCSS tokens.
- Unit Testing: Vitest.
