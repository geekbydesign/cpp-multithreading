# Mutex

## 1. What is a Mutex?

A **mutex** (mutual exclusion) is used to protect shared data from conflicting concurrent access.

```cpp
#include <mutex>
```

Basic idea:

```text
Thread 1 ──> lock ──> critical section ──> unlock
Thread 2 ──> wait ────────────────────────> lock
```

Only one thread can own a mutex at a time.

---

## 2. Why Use a Mutex?

```cpp
int counter = 0;

void increment()
{
    ++counter;
}
```

Multiple threads can access `counter` concurrently.

Protect it with a mutex:

```cpp
std::mutex mtx;

void increment()
{
    mtx.lock();
    ++counter;
    mtx.unlock();
}
```

Only one thread can execute the protected section at a time.

---

## 3. `std::mutex`

```cpp
std::mutex mtx;

mtx.lock();

// Critical section

mtx.unlock();
```

`lock()` waits until the mutex can be acquired.

---

## 4. Complete Example

```cpp
#include <iostream>
#include <thread>
#include <mutex>

int counter = 0;
std::mutex mtx;

void increment()
{
    mtx.lock();
    ++counter;
    mtx.unlock();
}

int main()
{
    std::thread t1(increment);
    std::thread t2(increment);

    t1.join();
    t2.join();

    std::cout << counter;
}
```

The mutex protects the modification of `counter`.

---

## 5. Critical Section

The code between `lock()` and `unlock()` is the protected region.

```cpp
mtx.lock();

++counter;
updateState();

mtx.unlock();
```

Keep critical sections as small as practical.

---

## 6. Why Manual `unlock()` Is Dangerous

```cpp
mtx.lock();

doSomething();

mtx.unlock();
```

If `doSomething()` throws an exception, `unlock()` may never execute.

```text
lock()
  ↓
exception
  ↓
unlock() not reached
  ↓
mutex remains locked
```

This is why RAII locking is preferred.

---

## 7. `try_lock()`

`try_lock()` attempts to acquire the mutex without waiting.

```cpp
if (mtx.try_lock())
{
    ++counter;
    mtx.unlock();
}
else
{
    // Mutex was unavailable
}
```

Returns:

```text
true  → acquired
false → not acquired
```

---

## 8. Mutex Ownership

A mutex has an owner while locked.

```text
Thread A
   |
   | lock()
   ↓
 Mutex
   |
   ↓
Thread A owns mutex
```

The thread that owns a `std::mutex` must unlock it. Unlocking it from another thread is undefined behavior.

---

## 9. Mutex Contention

If many threads compete for one mutex:

```text
Thread 1 ──┐
Thread 2 ──┤
Thread 3 ──┼──> Mutex
Thread 4 ──┤
Thread 5 ──┘
```

Only one enters the critical section at a time.

The others wait.

This is **lock contention**.

Too much contention can reduce performance.

---

## 10. Keep Critical Sections Small

Prefer:

```cpp
{
    std::lock_guard<std::mutex> lock(mtx);
    ++counter;
}
```

instead of holding the mutex during unrelated expensive work.

A useful rule:

> Protect the shared state, not unrelated work.

---

## 11. One Mutex vs Multiple Mutexes

One mutex is simple:

```text
Shared state
     ↓
One mutex
```

Multiple mutexes can reduce contention:

```text
Data A ← Mutex A
Data B ← Mutex B
```

But multiple mutexes increase deadlock risk.

There is a trade-off between simplicity, concurrency, contention, and deadlock risk.

---

## 12. Mutex vs Atomic

### Mutex

Useful for:

- Multiple related variables
- Complex operations
- Maintaining class invariants
- Larger critical sections

### Atomic

Useful for:

- Individual atomic values
- Simple counters and flags
- Atomic operations where appropriate

Do not automatically replace every mutex with an atomic.

---

## 13. Common Mistakes

### Forgetting to unlock

```cpp
mtx.lock();
++counter;
// Missing unlock
```

### Locking too much code

This increases contention.

### Locking too little

All relevant accesses to protected shared state must follow the synchronization design.

### Inconsistent lock ordering

This can lead to deadlocks.

### Unnecessary locking

Synchronization adds complexity and can add overhead.

---

## 14. Key Takeaways

- `std::mutex` provides mutual exclusion.
- Only one thread can own a mutex at a time.
- A mutex protects shared mutable state.
- `lock()` may block until the mutex is available.
- `try_lock()` attempts acquisition without blocking.
- Manual `lock()`/`unlock()` is error-prone.
- RAII locking is preferred.
- Keep critical sections small.
- Excessive contention can hurt performance.
- Multiple mutexes require careful design.
