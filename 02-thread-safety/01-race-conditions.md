# Race Conditions

## 1. What is a Race Condition?

A **race condition** occurs when the result of a program depends on the timing or order in which multiple threads execute.

It commonly happens when multiple threads access shared data and at least one thread modifies that data.

```cpp
int counter = 0;

void increment()
{
    ++counter;
}
```

If multiple threads call `increment()` concurrently, the result is not safely defined.

---

## 2. Shared Data

Threads in the same process can access the same memory.

```text
Thread 1 ──┐
           ├──> Shared data
Thread 2 ──┘
```

Examples:

- Global variables
- Static variables
- Shared objects
- Shared containers
- Class members accessed by multiple threads

Shared data is not automatically thread-safe.

---

## 3. Why `counter++` Is Not Safe

Conceptually, incrementing can involve:

```text
1. Read counter
2. Add 1
3. Write counter
```

Two threads can interfere with these operations.

```text
Initial counter = 0

Thread 1: Read 0
Thread 2: Read 0
Thread 1: Write 1
Thread 2: Write 1
```

The expected result would be `2`, but the operations conflict.

More importantly, in C++, an unsynchronized conflicting access constitutes a **data race**, which results in **undefined behavior**.

---

## 4. Race Condition vs Data Race

### Race condition

A broader concept where behavior depends on timing or ordering.

### Data race

A specific C++ situation where:

- Two or more threads access the same memory location concurrently.
- At least one access is a write.
- The accesses are not properly synchronized.
- The accesses are not atomic.

A data race causes undefined behavior.

---

## 5. Example

```cpp
#include <thread>

int counter = 0;

void increment()
{
    ++counter;
}

int main()
{
    std::thread t1(increment);
    std::thread t2(increment);

    t1.join();
    t2.join();
}
```

Both threads access `counter` without synchronization.

This program contains a data race.

---

## 6. Thread Scheduling

Thread execution order is not generally deterministic.

```text
Run 1: Thread 1 → Thread 2
Run 2: Thread 2 → Thread 1
Run 3: Thread 1 → Thread 2 → Thread 1
```

The scheduler and hardware determine when threads execute.

Therefore, code that appears to work repeatedly may still contain a race.

---

## 7. Why Race Conditions Are Difficult to Reproduce

A race may appear only under particular timing.

Factors include:

- CPU load
- Number of CPU cores
- Compiler optimization
- Operating system scheduling
- Timing
- Debugging/logging
- Hardware

A program may work correctly many times and fail on another run.

---

## 8. How to Fix a Race

Common mechanisms include:

### Mutex

```cpp
std::lock_guard<std::mutex> lock(mtx);
++counter;
```

### Atomic

```cpp
std::atomic<int> counter{0};
++counter;
```

Other mechanisms include:

- Condition variables
- Semaphores
- `std::shared_mutex`
- Higher-level task synchronization

Choose the mechanism based on the problem.

---

## 9. Critical Section

A **critical section** is code that accesses shared state and must be protected from conflicting concurrent access.

```cpp
{
    std::lock_guard<std::mutex> lock(mtx);
    ++counter;
}
```

---

## 10. Read vs Write

A simplified rule:

```text
Read + Read
    ↓
Usually safe

Read + Write
    ↓
Needs synchronization

Write + Write
    ↓
Needs synchronization
```

Multiple threads can generally read the same immutable data concurrently. Concurrent modification requires appropriate synchronization.

---

## 11. Common Mistakes

### Assuming `join()` prevents races

`join()` waits for a thread to finish. It does not protect shared data while threads are executing.

### Assuming simple operations are always safe

```cpp
counter++;
```

A simple expression is not automatically atomic.

### Adding delays

```cpp
std::this_thread::sleep_for(...);
```

A delay does not fix a race. It only changes timing.

### Testing only once

Concurrency bugs may not appear on every execution.

---

## 12. Key Takeaways

- Race conditions are timing/order-dependent problems.
- A data race is a specific unsynchronized memory-access problem in C++.
- Data races cause undefined behavior.
- Shared mutable state is a major source of concurrency bugs.
- `counter++` is not automatically thread-safe.
- Thread scheduling is nondeterministic.
- Mutexes and atomics are common solutions.
- `join()` does not protect shared data.
