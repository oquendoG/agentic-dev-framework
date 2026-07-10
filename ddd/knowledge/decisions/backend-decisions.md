---
type: DecisionLog
title: Backend Decisions
description: Architectural and platform decisions log for C# DDD backend framework (D0–D60).
timestamp: 2026-07-10T10:50:54-05:00
---

# Backend Decisions

## Cross-Feature

### D0: EF migrations applied manually
Generated with `dotnet ef migrations add` but NOT auto-applied on startup. Apply explicitly:
```bash
dotnet ef database update --startup-project src/Web.API --project src/Infrastructure
```

## Auth & Identity

### D1: AzureAd section name
Keep `AzureAd` config key intact because existing User Secrets rely on it.

### D2: ValidAudiences[] list
Multiple client apps require tokens. Authenticate both in JWT validation config.

### D10: ID tokens rejected, Access token required
ID tokens fail signature check. Frontend must request access tokens matching the custom API scope: `api://<clientId>/access_as_user`.

### D11: OID claim maps to full schema URI
Access tokens map object identifier to `http://schemas.microsoft.com/identity/claims/objectidentifier` instead of `oid`. Handle this claim path in `ICurrentUserService`.

### D8: Auth rate limit partition by user
1000 req/60s for authenticated endpoints, partitioned by resolved User ID instead of IP.

### D17: OID-only log templates (PII safety)
GDPR/OWASP Compliance: emails must never appear in structured message templates. Store/log `OID` only.

### D19: Login initialization rate limit
POST `/me` uses a stricter rate limit policy (10 req/60s, queue limit = 0) to prevent brute force.

## Architecture

### D13: Controllers located in API Features folder
Controllers live in `Web.API/Features/` (not `Application/`). Keeps the Application layer completely clean of ASP.NET Core dependencies.

### D12: Auth secrets excluded from appsettings
`appsettings.json` houses only empty placeholders. Actual credentials live exclusively in development local User Secrets.

## DDD / Organization

### D21: Catalog entities mapping
Catalog lookup entities use `private init` + `Crear()` factory. Added `ParaSeed()` method to enable EF database seeding with explicit numeric IDs.

### D22: DomainErrors centralized
Error constants stored in `SharedKernel/DomainErrors.cs`. No inline error strings in `Result.Error(...)` calls.

### D23: Core infrastructure abstraction ports
`IEmailService`, `IBlobStorageService` interfaces belong to `Application/Abstractions/`. Concrete implementations live in Infrastructure.

### D24: Seed data loaded from JSON files
JSON seeds are loaded using `AppContext.BaseDirectory` in order to resolve correctly both in development and production environments.

## Pagination & Auditing

### D27: Paged results in SharedKernel
`ResultadoPaginado<T>` lives in SharedKernel. Placing it in Application would create a circular dependency with kernel types.

### D25: Audit.EntityFramework User Resolver
`AuditContext.CurrentUser` (AsyncLocal) bridges the static `Audit.Core` setup with the per-request DI scope user resolver.

### D26: Audit values mapping
`AuditLogs` fields `OldValues` & `NewValues` use `jsonb`. Values serialized as JSON objects to avoid PG parser failures.

## Memory Optimization

### D29: Span<T> and streaming for files
Never copy full uploads to `MemoryStream` heap. Use `Span<T>`, `ReadOnlySpan<T>` or direct `IFormFile.OpenReadStream()` buffers.

## Resilience & Outbox

### D31: Transactional Outbox for emails
Emails written to `outbox_mensajes` in the same database transaction. Processed asynchronously by a background worker using channel signaling.

### D50: Resilience belongs to external boundaries, not controllers
Polly v8 is applied exclusively to external ports: HttpClients, SMTP, Storage. API controllers must remain thin.

### D51: Named pipelines per-dependency
Separate circuit breakers for each boundary (`msgraph-pipeline`, `blob-storage-pipeline`, etc.). A global circuit breaker would cascade outages across unrelated services.

### D52: ResilienceOptions in Infrastructure
Config options declared in `Infrastructure/Options/` to prevent circular dependencies. Validated on startup via `ResilienceOptionsFluentValidationAdapter`.

### D54: HttpClient.Timeout set to InfiniteTimeSpan
Ensure `HttpClient.Timeout` is infinite when Polly Timeout strategy is configured to avoid racing timeouts.

### D55: Circuit-breaker fallback
Catch `BrokenCircuitException` explicitly during external API calls to return graceful degradations (e.g. empty lists) instead of throwing 500.

### D56: Blob SAS url offline generation
Offline SAS builder signatures are not wrapped in Polly pipelines as they perform no network I/O.

### D57: EF Core default retry strategy
`EnableRetryOnFailure()` Npgsql execution strategy is sufficient for transaction retries.

### D58: Frontend retry limits
Idempotent methods (`GET`/`HEAD`/`OPTIONS`) retry up to 3 times on 408/429/5xx. Non-idempotent methods (`POST`/`PUT`/etc.) only retry on 429.

### D59: Silent authentication renewal retry
Silently handle interactive renewal warnings (`InteractionRequiredAuthError`) via redirect auth flow.

### D60: SignalR client-side reconnection
Clients handle reconnection backoff. Server stores no connection states.

### D61: Angular GlobalErrorHandler toast notifications
Runtime errors are mapped and dispatched to the global Message/Toast Service.

### D63: Blob streams seek guard
Resets `Position = 0` only when `CanSeek` is true to support non-seekable streams.

### D46: [FromServices] for single-action controller dependencies
Avoid constructor bloating. Inject single-use helpers (PDF, Excel) directly at the controller method level.
