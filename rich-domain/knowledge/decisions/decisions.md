# Rich Domain Model — Design Decisions

## Entity Encapsulation
- Use `private set` for all entity properties
- EF Core can set properties via reflection during materialization
- Update methods (`entity.Update(...)`) for state changes after construction
- Factory methods (`Entity.Create(...)`) for creation with domain invariants

## Error Handling
- **Exceptions:** Only for invariant violations (programmer errors)
  - `ArgumentException` for invalid factory parameters
  - `ArgumentException.ThrowIfNullOrWhiteSpace(...)` for null/empty input in services
- **Result<T>:** For business errors (expected failures)
  - "Not found", "Already exists", "Invalid state transition"
  - Services return `Result<T>` instead of throwing exceptions

## Update Pattern
- Entities have `Update(...)` methods for state changes
- Services call `entity.Update(...)` instead of direct property assignment
- This maintains encapsulation while allowing EF Core to track changes

## Factory Pattern
- `Entity.Create(...)` validates domain invariants and creates valid entities
- Returns the entity on success, throws `ArgumentException` on invariant violation
- Services use factories for creation, not direct `new Entity()` with object initializers
