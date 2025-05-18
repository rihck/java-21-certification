
# ☕ Java 21 - Class Recap: Refactoring with Records, Sealed & Pattern Matching

## ✅ New Features Covered

### 1. `record` (Java 14+, stable in Java 21)
- Lightweight immutable data class
- Auto-generates constructor, accessors, `equals`, etc.
- No setters — values are fixed

**In-class example:**
```java
public record Bagel(BagelType bagelType, SpreadType spreadType, boolean toasted) implements BakeryItem {
    @Override public double getPrice() { return 2.50; }
}
```

---

### 2. Pattern Matching + Record Patterns

#### a. `instanceof` with binding:
```java
if (item instanceof Donut donut) { ... }
```

#### b. Destructuring Records:
```java
if (item instanceof Bagel(BagelType type, SpreadType s, boolean t)) { ... }
```

- **Doubt**: Can we ignore fields?
  - ❌ Not with `_` in Java 21
  - ✅ Use `var unused1`, `var unused2` instead

---

### 3. Pattern Matching with `switch`

```java
switch (item) {
    case Bagel b -> ...
    case Cookie c -> ...
    case Donut d -> ...
    default -> ...
}
```

- Cleaner than `if-else instanceof`
- Combined with `sealed`, allows exhaustive matching

---

### 4. Sealed Classes & Interfaces

Controls who can inherit or implement a type.

```java
public sealed interface CoffeeDrink permits Latte, Macchiato, Americano {}
```

Each subtype must be:
- `final`, `sealed`, or `non-sealed`

```java
public final class Latte implements CoffeeDrink {}
public non-sealed class Macchiato implements CoffeeDrink {}
```

- ✅ Compiler tracks hierarchy even if an intermediate type is `non-sealed`

---

### 5. Use in the Coffee Shop Kata

- `getReceiptForFoodItems()` refactored using `record patterns`
- `getFoodItemsForOrder()` uses `switch` with concrete types
- `getDrinksForOrder()` shows sealed hierarchy use (no pattern matching)

---

## 🧠 Notes
- Record getters use `fieldName()` instead of `getFieldName()`
- Sealed hierarchies enforce subclass declaration
- `_` pattern ignored unless Java 22+
