# RAII Locking

## 1. Why RAII Locking?

Manual mutex management is error-prone:

```cpp
mtx.lock();

doSomething();

mtx.unlock();
```

If `doSomething()` throws, `unlock()` may never execute.

C++ uses **RAII (Resource Acquisition Is Initialization)** to manage mutex ownership automatically.

Main RAII locking wrappers:

- `std::lock_guard`
- `std::unique_lock`
- `std::scoped_lock` (C++17)

---

## 2. `std::lock_guard`

`lock_guard` is the simplest RAII locking mechanism.

```cpp
std::lock_guard<std::mutex> lock(mtx);

++counter;
```

When `lock` is created, the mutex is locked.

When `lock` goes out of scope, the mutex is unlocked.

```text
lock_guard constructor
        ↓
mutex locked
        ↓
critical section
        ↓
lock_guard destructor
        ↓
mutex unlocked
```

---

## 3. Scope-Based Locking

```cpp
void increment()
{
    {
        std::lock_guard<std::mutex> lock(mtx);
        ++counter;
    }

    // Mutex is unlocked here
}
```

Use a smaller scope when only part of a function needs protection.

---

## 4. Exception Safety

```cpp
void update()
{
    std::lock_guard<std::mutex> lock(mtx);

    doSomethingThatMayThrow();
}
```

If the function throws, the destructor of `lock_guard` still runs and releases the mutex.

This is one of the main benefits of RAII.

---

## 5. `std::unique_lock`

`unique_lock` provides more flexibility.

```cpp
std::unique_lock<std::mutex> lock(mtx);
```

It supports:

- Deferred locking
- Manual unlock
- Re-locking
- `try_lock`
- Moving the lock object

It is especially important with condition variables.

---

## 6. Basic `unique_lock`

```cpp
void increment()
{
    std::unique_lock<std::mutex> lock(mtx);
    ++counter;
}
```

The mutex is automatically released when the lock object goes out of scope.

---

## 7. `defer_lock`

Create the lock object without locking immediately:

```cpp
std::unique_lock<std::mutex> lock(
    mtx,
    std::defer_lock
);

// Do some work

lock.lock();

// Critical section
```

---

## 8. `try_to_lock`

Try to acquire the mutex without blocking:

```cpp
std::unique_lock<std::mutex> lock(
    mtx,
    std::try_to_lock
);

if (lock.owns_lock())
{
    // Mutex acquired
}
else
{
    // Mutex unavailable
}
```

---

## 9. `adopt_lock`

`adopt_lock` tells the wrapper that the mutex is already locked by the current thread.

```cpp
mtx.lock();

std::unique_lock<std::mutex> lock(
    mtx,
    std::adopt_lock
);
```

The `unique_lock` now takes responsibility for unlocking it.

Use this only when the mutex has actually been locked by the current thread.

---

## 10. Manual Unlock and Re-lock

`unique_lock` can release and reacquire the mutex:

```cpp
std::unique_lock<std::mutex> lock(mtx);

doProtectedWork();

lock.unlock();

doUnprotectedWork();

lock.lock();

doMoreProtectedWork();
```

This flexibility is one of the main differences from `lock_guard`.

---

## 11. `owns_lock()`

```cpp
std::unique_lock<std::mutex> lock(
    mtx,
    std::try_to_lock
);

if (lock.owns_lock())
{
    // Protected work
}
```

`owns_lock()` tells whether the `unique_lock` currently owns the mutex.

---

## 12. `std::scoped_lock`

C++17 introduced `std::scoped_lock`.

It is especially useful for locking multiple mutexes:

```cpp
std::mutex mtx1;
std::mutex mtx2;

void function()
{
    std::scoped_lock lock(mtx1, mtx2);

    // Both mutexes are locked
}
```

It uses a deadlock-avoidance algorithm when locking multiple mutexes.

---

## 13. Comparison

| Feature | `lock_guard` | `unique_lock` | `scoped_lock` |
|---|---|---|---|
| RAII | Yes | Yes | Yes |
| Basic locking | Yes | Yes | Yes |
| Manual unlock/re-lock | No | Yes | No |
| Deferred locking | No | Yes | No |
| `try_to_lock` | No | Yes | No |
| Multiple mutexes | No | Possible | Yes |
| Introduced | C++11 | C++11 | C++17 |

---

## 14. Which One Should You Use?

### Use `lock_guard`

For a simple critical section:

```cpp
std::lock_guard<std::mutex> lock(mtx);
counter++;
```

### Use `unique_lock`

When you need:

- Condition variables
- Deferred locking
- Manual unlock/re-lock
- `try_to_lock`
- Flexible ownership

### Use `scoped_lock`

When locking multiple mutexes:

```cpp
std::scoped_lock lock(mtx1, mtx2);
```

---

## 15. Thread-Safe Class Example

```cpp
class Counter
{
private:
    int value = 0;
    mutable std::mutex mtx;

public:
    void increment()
    {
        std::lock_guard<std::mutex> lock(mtx);
        ++value;
    }

    int get() const
    {
        std::lock_guard<std::mutex> lock(mtx);
        return value;
    }
};
```

Both the read and write are protected.

The important principle is:

> Synchronize all relevant accesses to shared mutable state.

---

## 16. Common Mistakes

### Wrong scope

```cpp
{
    std::lock_guard<std::mutex> lock(mtx);
}

++counter; // Not protected
```

### Using `unique_lock` unnecessarily

If simple locking is enough, `lock_guard` is usually clearer.

### Incorrect `adopt_lock`

Do not use `adopt_lock` unless the mutex is already locked by the current thread.

### Holding locks during expensive work

```cpp
std::lock_guard<std::mutex> lock(mtx);

expensiveOperation();
```

If the operation does not need the mutex, perform it outside the critical section when appropriate.

---

## 17. Recommended Rule

```text
Simple critical section
        ↓
std::lock_guard

Flexible locking
        ↓
std::unique_lock

Multiple mutexes
        ↓
std::scoped_lock
```

---

## 18. Key Takeaways

- RAII automatically manages mutex lifetime.
- `std::lock_guard` is the simplest RAII lock.
- `std::unique_lock` provides more control.
- `std::scoped_lock` is useful for multiple mutexes.
- RAII improves exception safety.
- Keep lock scope as small as practical.
- Prefer the simplest locking mechanism that solves the problem.
