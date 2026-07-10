---
type: ArchitectureGuide
title: Frontend State & Data (DDD)
description: SignalStore conventions, HTTP resources, API service wrapper rules, and Angular 21 async data fetching.
timestamp: 2026-07-10T10:50:54-05:00
---

## SignalStore Pattern
Implemented using `@ngrx/signals`:
```ts
export const FeatureStore = signalStore(
  withState<T>({ ... }),
  withComputed(({ state }) => ({ derived: computed(...) })),
  withMethods((store, service = inject(FeatureService)) => ({
    load: rxMethod<Params>(pipe(
      switchMap(params => service.getData(params).pipe(
        tapResponse({
          next: data => patchState(store, { data }),
          error: error => console.error(error)
        })
      ))
    ))
  }))
);
```
- Localized scoping: Instantiated at the smart page component level `providers: [FeatureStore]`. One store per feature (no shared/global stores).
- State changes must occur via methods wrapping `patchState`.

## httpResource
- Use Angular's reactive `httpResource` or `resource` for declarative read-only queries:
  ```ts
  readonly data = httpResource<T>(() => `/api/v1/resource/${this.id()}`);
  ```
- Replaces traditional RxJS subscription or `toSignal(httpClient.get)`.

## API Service Rules
- Component code must never import `HttpClient` directly.
- Feature operations are declared as methods in a service class (in `services/`) utilizing typed DTO contracts (`models/`).
- **Queries:** Return observable or resource signal.
- **Mutations (Commands):** Use `firstValueFrom` to return a `Promise` for async/await usage in store methods or smart component flows:
  ```ts
  await firstValueFrom(this.service.crear(data));
  ```
