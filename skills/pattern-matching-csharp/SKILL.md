---
name: pattern-matching-csharp

description: Guide agent to code with modern style with pattern marching
---

## CORE KNOWLEDGE BASE

### 1. Pattern Types & Taxonomy

Master and correctly recommend the appropriate pattern variant:

- **Type & Declaration Patterns:** `is T variable` (e.g., `if (obj is Stream stream)`).

- **Constant Patterns:** `is null`, `is 42`, `is "completed"`.

- **Relational Patterns:** Operators `>`, `<`, `>=`, `<=` (e.g., `x is > 0 and <= 100`).

- **Logical Patterns:** Combining conditions using `and`, `or`, and `not` (e.g., `is not null`, `is >= 18 and <= 65`).

- **Property Patterns:** Matching properties/fields recursively with `{ }` (e.g., `user is { IsActive: true, Address.Country: "CO" }`).

- **Positional Patterns:** Deconstruction matching using tuples or type `Deconstruct` methods (e.g., `point is (0, 0)`).

- **Var Pattern:** Capturing any expression into a local variable without type checks (e.g., `expr is var x`).

- **List Patterns:** Matching array or collection elements by position/slice (e.g., `array is [1, 2, .. var rest]`, `items is [_, >= 10, ..]`).

- **Discard Pattern:** `_` as a wildcard for unmatched elements or fallthrough arms.

---

## GUIDING RULES & ARCHITECTURAL PATTERNS

### Rule 1: Switch Expressions Over Switch Statements

- Always prefer `switch` expressions over traditional `switch` statements when returning a value or mapping states.

- Ensure expressions are **exhaustive**. Always supply the default arm (`_ => ...`) unless all enum/discriminated union states are proven to be fully covered by the compiler.

**Good:**

```csharp
public decimal GetDiscount(Customer customer) => customer switch
{
    { IsVIP: true, Orders.Count: > 10 } => 0.20m,
    { IsVIP: true }                     => 0.10m,
    { AccountAge.TotalDays: > 365 }     => 0.05m,
    _                                   => 0.00m
};
```
