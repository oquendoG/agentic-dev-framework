---
type: ArchitectureGuide
title: Backend Data Access (DDD)
description: Database conventions, EF Core rules, auditing, and seeding configurations in C# for DDD setups.
timestamp: 2026-07-10T10:50:54-05:00
---

## PK & Unique Keys
- **PK:** `Ulid` (C#) mapped to `char(26)` (PostgreSQL). Production: `Ulid.NewUlid()`. Tests: `Ulid.Parse("01H...")`.
- **Business Keys:** Unique indexes in DB (National ID, Email), never primary keys.

## EF Core Rules
- **Reads:** Mandatory `AsNoTracking()` + direct `.Select()` projection to DTOs.
- **Eager Loading:** Use `.AsSplitQuery()` to prevent cartesian explosion/N+1 queries.
- **Idempotency:** Implement "Find or Create" logic by business keys before insertion.
- **Tech Stack:** Npgsql provider, PostgreSQL 17.

## DbContext & Auditing
- **Phase 1:** Single connection string plain `DbContext`. No manual `TenantId` filtering (physical database-per-tenant isolation planned for Phase 2).
- **Audit Trail (Audit.EntityFramework.Core):** `AppDbContext` inherits `AuditDbContext`. Automatically logs entity changes, old/new values, timestamps, and active user to fulfill audit compliance requirements.

## Configurations
- Place EF Core fluent API mappings in `Persistence/Configurations/` (one class per entity).
- Use `jsonb` column types for flexible metadata dictionary mappings: `.HasColumnType("jsonb")`.
