---
type: ArchitectureGuide
title: Backend Patterns
description: Result<T>, guards, entity behavior, and encapsulation patterns.
timestamp: 2026-07-29T10:00:00-05:00
---

# Backend Patterns

## Entity Pattern (Rich Domain)
```csharp
public class Entity
{
    public Ulid Id { get; private set; } = Ulid.NewUlid();
    public string Name { get; private set; } = string.Empty;
    
    // Factory method - validates invariants
    public static Entity Create(string name)
    {
        ArgumentException.ThrowIfNullOrWhiteSpace(name);
        return new Entity { Name = name };
    }
    
    // Update method - allows state changes
    public void Update(string name)
    {
        ArgumentException.ThrowIfNullOrWhiteSpace(name);
        Name = name;
    }
    
    // Behavior methods
    public bool IsActive() => true;
}
```

## Result<T> Pattern
```csharp
// Services return Result<T> for business errors
public Result<Entity> GetById(Ulid id)
{
    Entity? entity = _dbContext.Entities.FirstOrDefault(e => e.Id == id);
    if (entity is null)
        return Result<Entity>.NotFound();
    return Result<Entity>.Ok(entity);
}
```

## Guard Pattern
```csharp
// Input validation in services
ArgumentNullException.ThrowIfNull(dto);
ArgumentException.ThrowIfNullOrWhiteSpace(dto.Name, nameof(dto.Name));
```

## Exception vs Result<T>
- **Exceptions:** Invariant violations (programmer errors)
- **Result<T>:** Business errors (expected failures)
- **Guards:** Input validation in services (exceptions for null/empty)
