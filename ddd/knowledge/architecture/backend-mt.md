---
type: ArchitectureGuide
title: Backend Multi-Tenancy (DDD)
description: Multi-tenancy architecture, phase strategy, isolation principles, and tenant admin setup for DDD setups.
timestamp: 2026-07-10T10:50:54-05:00
---

## Multi-Tenancy Model
- **Database-per-tenant:** Physical database isolation for each tenant on a shared PostgreSQL Flexible Server.
- **Finbuckle.MultiTenant:** Resolves tenant identity via subdomain (e.g. `tenant.domain.com`) in request pipeline.
- Injects resolved tenant connection string dynamically into scoped `AppDbContext`.

## Phase Approach
- **Phase 1 (Current):** Single tenant. Explicit connection string in `appsettings.json`. Finbuckle libraries disabled.
- **Phase 2 (Future):** Implement Finbuckle and register Tenant master database once multi-tenant client onboarding begins.

## Isolation
- Domain code (Aggregates, Value Objects, Handlers) is 100% tenant-agnostic.
- No `TenantId` column/query filters; isolation is enforced physically by connection strings.

## Tenant Admin
- User directory-level isolation using Microsoft Entra External ID (CIAM) or similar identity provider.
- Tenant Admin of one entity cannot search or edit users of another tenant.
- `GraphUserService` handles Graph API interaction for CRUD.
