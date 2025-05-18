# Java 21 - Stream API & Best Practices (Lesson 2 and 3)

## ✨ Overview
Lessons focused on two key pillars of the OCP Java 21 certification:
- Concurrency with Threads, Safe Collections, Locks, and Virtual Threads (Lesson 3)

---

## 🧵 Lesson 3 – Concurrency and Virtual Threads (Concise Summary)

### 🧠 Thread Fundamentals:
- `Thread` and `Runnable` are basic ways to execute parallel tasks.
- Lambdas serve as overrides for functional interfaces like `Runnable`, `Supplier`, etc.
- Always use `start()` to run code on a new thread — never call `run()` directly.

### 🔒 Concurrent Collections:
- `Collections.synchronizedList(...)` synchronizes individual methods (like `add()`), but **not compound operations** like `contains() + add()`.
- Compound operations must be protected via external `synchronized` blocks or `ReentrantLock`.
- Prefer modern concurrent collections like `CopyOnWriteArrayList`, `ConcurrentHashMap`, `ConcurrentLinkedQueue`.

### 🔐 ReentrantLock:
- An advanced alternative to `synchronized` with features like timeouts (`tryLock()`), interruption, and fairness.
- Must be released manually via `unlock()`, ideally inside a `try/finally` block.

### ⚡ CompletableFuture & Executor:
- `CompletableFuture.supplyAsync(supplier, executor)` runs the logic asynchronously in a separate thread.
- While high-level and abstracted, shared data still needs proper synchronization inside the executed code.

### 🧵 Virtual Threads (Java 21):
- Lightweight, scalable threads capable of handling millions of tasks in parallel.
- Created using `Thread.startVirtualThread(...)` or `Executors.newVirtualThreadPerTaskExecutor()`.
- Avoid `synchronized`, use `ReentrantLock` to prevent carrier thread pinning.

### 📌 Key Takeaways:
- Threading abstraction (e.g., `CompletableFuture`, Executors) doesn't eliminate concurrency responsibilities.
- Atomic operations are indivisible; compound logic must be explicitly guarded.
- Virtual Threads simplify thread management but do not make code automatically thread-safe.

