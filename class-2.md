# Java 21 - Stream API & Best Practices (Aula 2)

## ✨ Overview

This session focused on mastering the Java Stream API through 15 practical exercises, reinforcing transformation, filtering, aggregation, grouping, and mapping operations in Java. It also covered practical caveats with Lombok annotations in JPA, and real-world use cases of functional interfaces like `Supplier`.

---

## 🔍 Lombok & JPA Caution

Using `@Data` on JPA entities is considered bad practice due to:

- **equals/hashCode() problems**: may break identity logic, affect caching, or create recursion.
- **toString() recursion**: especially with bidirectional relationships.
- **Encapsulation leaks**: `@Data` generates setters/getters for all fields.
- **Constructor conflicts**: conflicts with JPA-required no-args constructors.

### ✅ Recommended Alternative:

```java
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@ToString(onlyExplicitlyIncluded = true)
@EqualsAndHashCode(onlyExplicitlyIncluded = true)
@Entity
public class User {
    @Id
    @GeneratedValue
    @EqualsAndHashCode.Include
    private Long id;

    @ToString.Include
    private String name;

    private String email;

    @OneToMany(mappedBy = "user")
    private List<Order> orders = new ArrayList<>();
}
```

---

## 🔢 Java Stream API Exercises Summary

Based on [this GitHub repo](https://github.com/gavinklfong/stream-api-exercises) and the PDF guide.

### Exercise Highlights:

| #  | Topic                                           | Key API Concepts                        |
| -- | ----------------------------------------------- | --------------------------------------- |
| 1  | Filter tier 2 customers                         | `filter()`                              |
| 2  | Filter orders containing baby category          | `anyMatch()` / `stream()` inside stream |
| 3  | Apply 10% discount on Toys                      | `filter()`, `map()`                     |
| 4  | List of products by tier 2 customers in Feb/Apr | `flatMap()`, `distinct()`               |
| 5  | Get cheapest Book                               | `min()`, `Comparator.comparing()`       |
| 6  | Get 3 most recent orders                        | `sorted().reversed()`, `limit()`        |
| 7  | Log orders & return product list                | `peek()`, `flatMap()`                   |
| 8  | Total sum in Feb                                | `mapToDouble().sum()`                   |
| 9  | Book stats (avg, max...)                        | `summaryStatistics()`                   |
| 10 | Group products by category                      | `groupingBy()`                          |
| 11 | Map: orderId -> #products                       | `toMap()`                               |
| 12 | Map: customer -> orders                         | `groupingBy()`                          |
| 13 | Map: order -> total sum                         | `toMap()`, `Function.identity()`        |
| 14 | Map: category -> product names                  | `groupingBy()`, `mapping()`             |
| 15 | Most expensive product by category              | `groupingBy()`, `maxBy()`               |

---

## ☑️ Real-World Use of `Supplier` in `Collectors.toMap()`

### ⚙ Method Signature:

```java
public static <T, K, U, M extends Map<K,U>> Collector<T, ?, M> toMap(
    Function<? super T, ? extends K> keyMapper,
    Function<? super T, ? extends U> valueMapper,
    BinaryOperator<U> mergeFunction,
    Supplier<M> mapSupplier
)
```

### 🔹 Practical Example:

```java
List<String> names = List.of("Ana", "Luis", "Carlos", "Beatriz");

Map<Character, String> sortedMap = names.stream()
    .collect(Collectors.toMap(
        name -> name.charAt(0),
        Function.identity(),
        (s1, s2) -> s1, // keep first on key collision
        TreeMap::new
    ));
```

### 🌟 Why it's useful:

- Customize map type: `TreeMap`, `LinkedHashMap`, etc.
- Leverages method references: `TreeMap::new` as a `Supplier`
- Fully compatible with functional programming

---

## 📍 Key Takeaways

- Mastery of `Stream` transformations (filter, map, flatMap)
- Proper use of downstream collectors: `groupingBy`, `mapping`, `maxBy`, `toMap`
- Awareness of functional programming patterns (Supplier, Comparator, Function)
- Cleaner & safer alternatives to Lombok's `@Data`

---

Ready for next level: `partitioningBy`, `collectingAndThen`, `teeing()` and more advanced use of sealed types & record patterns (Java 21+).
