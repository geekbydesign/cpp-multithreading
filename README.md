# C++ Multithreading & Concurrency

A structured set of notes and hands-on examples for learning **C++ multithreading and concurrency from beginner to advanced level**.

The notes are focused on **C++17** for core interview preparation, with **C++20 concurrency features** covered where they are part of the syllabus.

---

## 📚 Learning Path

The repository is organized into 13 levels covering **31 chapters**.

### Level 1 — Multithreading Fundamentals

**Chapter 1: Why Multithreading?**
- Process and thread
- Process vs thread
- Single-threaded vs multithreaded programs
- Concurrency vs parallelism
- CPU cores
- Context switching
- Thread scheduling
- Benefits and drawbacks of multithreading

**Chapter 2: Creating Threads with `std::thread`**
- Creating threads
- Thread functions
- Lambdas
- Passing arguments
- `join()`, `detach()`, `joinable()`
- Thread lifetime
- `std::this_thread`
- Thread IDs

**Chapter 3: Passing Data to Threads**
- Passing by value
- Passing by reference
- `std::ref`, `std::cref`
- Pointers and objects
- Lambdas
- `std::move`
- Lifetime and dangling-reference problems

---

### Level 2 — Thread Safety & Race Conditions

**Chapter 4: Race Conditions**
- Shared data
- Race conditions
- Data races
- Undefined behavior
- Read/read, read/write, write/write access
- Thread-safe vs non-thread-safe code

**Chapter 5: Mutex**
- `std::mutex`
- `lock()` / `unlock()`
- Critical sections
- Mutual exclusion
- Contention
- Deadlock introduction

**Chapter 6: RAII-Based Locking**
- `std::lock_guard`
- `std::unique_lock`
- `std::scoped_lock`
- Deferred locking
- `try_to_lock`
- `adopt_lock`
- Exception safety

---

### Level 3 — Synchronization

**Chapter 7: Deadlocks**
- Deadlock
- Four conditions for deadlock
- Lock ordering
- Nested locks
- `std::lock`
- `std::scoped_lock`
- `std::try_lock`
- Deadlock prevention

**Chapter 8: Condition Variables**
- `std::condition_variable`
- `wait()`
- `notify_one()`
- `notify_all()`
- Predicates
- Spurious wakeups
- Producer-consumer synchronization

**Chapter 9: Producer-Consumer**
- Shared queues
- Mutex + condition variable
- Producers and consumers
- Multiple producers
- Multiple consumers
- Shutdown mechanisms

---

### Level 4 — C++ Concurrency Utilities

**Chapter 10: Futures and Promises**
- `std::future`
- `std::promise`
- `get()`
- `wait()`
- `wait_for()`
- `wait_until()`
- Exception propagation

**Chapter 11: `std::async`**
- `std::async`
- `std::launch::async`
- `std::launch::deferred`
- Futures returned by `async`
- `async` vs `std::thread`

**Chapter 12: Packaged Tasks**
- `std::packaged_task`
- Callable → future
- `packaged_task` vs `promise`
- Use in thread pools

---

### Level 5 — Atomic Programming

**Chapter 13: Atomic Variables**
- `std::atomic`
- Atomic reads/writes
- Atomic increment/decrement
- `load()`
- `store()`
- `exchange()`
- Compare-and-exchange

**Chapter 14: Memory Ordering**
- Memory ordering
- Sequential consistency
- `memory_order_seq_cst`
- `memory_order_relaxed`
- Acquire/release
- `memory_order_acq_rel`
- `memory_order_consume`
- Happens-before
- Synchronizes-with

---

### Level 6 — Advanced Synchronization

**Chapter 15: Reader-Writer Synchronization**
- `std::shared_mutex`
- `std::shared_lock`
- Multiple readers
- Single writer
- Reader-writer problem
- Writer starvation

**Chapter 16: Semaphores (C++20)**
- `std::counting_semaphore`
- `std::binary_semaphore`
- `acquire()`
- `release()`
- `try_acquire()`
- Mutex vs condition variable vs semaphore

**Chapter 17: Latches and Barriers (C++20)**
- `std::latch`
- `std::barrier`
- One-time synchronization
- Reusable synchronization
- Phase-based synchronization

---

### Level 7 — Thread Pools & Task-Based Concurrency

**Chapter 18: Thread Pool**
- Worker threads
- Task queues
- Task submission
- Worker loop
- Shutdown
- Graceful shutdown
- Task synchronization

**Chapter 19: C++20 `std::jthread`**
- `std::jthread`
- Automatic joining
- `stop_token`
- `stop_source`
- Cooperative cancellation
- `stop_callback`
- `std::thread` vs `std::jthread`

---

### Level 8 — C++ Memory Model

**Chapter 20: C++ Memory Model**
- Objects and memory locations
- Visibility
- Compiler and CPU reordering
- Cache
- Cache coherence
- Happens-before
- Synchronizes-with
- Data races
- Atomicity
- Visibility vs ordering

**Chapter 21: Hardware-Level Concepts**
- CPU cores
- L1/L2/L3 caches
- Cache lines
- Cache coherence
- False sharing
- Context switching
- Hyper-threading / SMT
- Memory barriers/fences
- NUMA basics

---

### Level 9 — Lock-Free & Advanced Atomics

**Chapter 22: Lock-Free Programming**
- Lock-free vs wait-free
- Atomic operations
- CAS
- Spinlocks
- Atomic flags
- Lock-free counters
- Lock-free queues
- ABA problem
- Memory reclamation

**Chapter 23: Atomic Smart Pointers & Advanced Techniques**
- `std::atomic<std::shared_ptr<T>>`
- Atomic ownership
- Safe publication
- Object lifetime under concurrency
- Hazard pointers
- Epoch-based reclamation

---

### Level 10 — Performance & Scalability

**Chapter 24: Multithreading Performance**
- Thread creation overhead
- Synchronization overhead
- Lock contention
- CPU utilization
- Throughput
- Latency
- Scalability
- Amdahl's Law
- Granularity
- Work partitioning
- Thread affinity
- Oversubscription

**Chapter 25: False Sharing & Cache Optimization**
- Cache lines
- False sharing
- Padding
- `alignas`
- Data locality
- Structure layout
- Contiguous data
- Avoiding unnecessary synchronization

---

### Level 11 — Real-World Concurrency Patterns

**Chapter 26: Common Concurrency Patterns**
- Producer-consumer
- Thread pool
- Pipeline
- Work queue
- Fork-join
- Parallel reduction
- Reader-writer
- Barrier synchronization
- Task parallelism
- Data parallelism
- Active object
- Future/promise pattern

**Chapter 27: Thread-Safe Design**
- Thread-safe classes
- Immutable objects
- Shared ownership
- Ownership transfer
- Protecting class invariants
- Thread-safe singleton
- Double-checked locking
- Initialization-order issues
- Static initialization guarantees
- Concurrent API design

---

### Level 12 — Debugging & Testing Concurrent Code

**Chapter 28: Debugging Concurrent Programs**
- Debugging multiple threads
- Thread IDs
- Breakpoints
- Deadlock debugging
- Race-condition debugging
- Logging from multiple threads
- Core dumps
- Thread dumps

**Chapter 29: Sanitizers & Tools**
- ThreadSanitizer
- AddressSanitizer
- UndefinedBehaviorSanitizer
- GDB
- Visual Studio debugger
- Linux `perf`
- CPU profiling
- Concurrency stress testing

---

### Level 13 — Advanced Interview Preparation

**Chapter 30: C++ Multithreading Interview Questions**
- Process vs thread
- Concurrency vs parallelism
- Race condition
- Data race
- Mutex
- Locking mechanisms
- Deadlocks
- Condition variables
- Producer-consumer
- Atomics
- CAS
- Memory ordering
- Futures/promises
- `async`
- Thread pools
- `jthread`
- Semaphores
- `shared_mutex`
- False sharing
- Cache coherence
- Lock-free programming

**Chapter 31: Multithreading Coding Problems**
- Thread-safe counter
- Producer-consumer queue
- Thread-safe queue
- Thread-safe singleton
- Thread pool
- Parallel sum
- Parallel matrix processing
- Reader-writer system
- Bounded blocking queue
- Rate limiter
- Task scheduler
- Lock-free stack
- Lock-free queue

---

## 📁 Repository Structure

```text
cpp-multithreading/
│
├── README.md
│
├── 01-multithreading-fundamentals/
│   ├── 01-why-multithreading.md
│   ├── 02-std-thread.md
│   └── 03-passing-data-to-threads.md
│
├── 02-thread-safety/
│   ├── 01-race-conditions.md
│   ├── 02-mutex.md
│   └── 03-raii-locking.md
│
├── 03-synchronization/
│   ├── 01-deadlocks.md
│   ├── 02-condition-variables.md
│   └── 03-producer-consumer.md
│
├── 04-concurrency-utilities/
│   ├── 01-futures-promises.md
│   ├── 02-async.md
│   └── 03-packaged-task.md
│
├── 05-atomics/
│   ├── 01-atomic.md
│   └── 02-memory-ordering.md
│
├── 06-advanced-synchronization/
│   ├── 01-shared-mutex.md
│   ├── 02-semaphores.md
│   └── 03-latches-barriers.md
│
├── 07-thread-pools/
│   ├── 01-thread-pool.md
│   └── 02-jthread.md
│
├── 08-cpp-memory-model/
│   ├── 01-memory-model.md
│   └── 02-hardware-concurrency.md
│
├── 09-lock-free-programming/
│   ├── 01-lock-free.md
│   ├── 02-cas.md
│   ├── 03-aba-problem.md
│   └── 04-advanced-atomic-techniques.md
│
├── 10-performance/
│   ├── 01-performance.md
│   └── 02-cache-false-sharing.md
│
├── 11-concurrency-patterns/
│   ├── 01-common-concurrency-patterns.md
│   └── 02-thread-safe-design.md
│
├── 12-debugging-testing/
│   ├── 01-debugging-concurrent-programs.md
│   └── 02-sanitizers-and-tools.md
│
├── 13-interview-preparation/
│   ├── 01-interview-questions.md
│   └── 02-coding-problems.md
│
└── projects/
    ├── thread-safe-queue/
    ├── producer-consumer/
    └── thread-pool/
```

---

## 🎯 Goals

By completing this repository, you should be able to:

- Understand how C++ threads work internally at a practical level
- Write and manage multithreaded C++ programs
- Identify race conditions and data races
- Use mutexes and RAII-based locking correctly
- Design producer-consumer systems
- Use condition variables effectively
- Understand futures, promises, `async`, and packaged tasks
- Use atomic operations and understand memory ordering
- Work with C++20 synchronization primitives
- Build a basic thread pool
- Understand the C++ memory model
- Understand lock-free programming concepts
- Identify performance problems such as contention and false sharing
- Design thread-safe classes and APIs
- Debug and test concurrent programs
- Solve common C++ multithreading interview problems

---

## 🛠️ Language Standard

### Primary
- **C++17**

### C++20 Features Covered
- `std::counting_semaphore`
- `std::binary_semaphore`
- `std::latch`
- `std::barrier`
- `std::jthread`
- `std::stop_token`
- `std::stop_source`
- `std::stop_callback`

Core interview preparation remains centered around **C++17**, while C++20 concurrency features are included where specified in the syllabus.

---

## 📌 Recommended Study Order

Follow the repository from **Level 1 → Level 13**.

The progression is intentional:

```text
Threads
   ↓
Thread Safety
   ↓
Synchronization
   ↓
Concurrency Utilities
   ↓
Atomics
   ↓
Advanced Synchronization
   ↓
Thread Pools
   ↓
Memory Model
   ↓
Lock-Free Programming
   ↓
Performance
   ↓
Concurrency Patterns
   ↓
Debugging & Testing
   ↓
Interview Preparation
```

Do not skip the fundamentals. Topics such as **mutexes, condition variables, atomics, memory ordering, and the C++ memory model** form the foundation for the advanced chapters.

---

## 💻 Practice Projects

The `projects/` directory contains larger implementations that combine concepts from multiple chapters.

### Thread-Safe Queue
Practice:
- Mutex
- Condition variable
- RAII locking
- Thread safety
- Shutdown

### Producer-Consumer
Practice:
- Shared queue
- Producers
- Consumers
- Condition variables
- Synchronization

### Thread Pool
Practice:
- Worker threads
- Task queue
- Condition variables
- Futures/tasks
- Graceful shutdown

---

## 🚀 Repository Philosophy

These notes are intended for:

- **Learning**
- **Revision**
- **C++ interview preparation**
- **Hands-on implementation**

The focus is on understanding **why** concurrency mechanisms are needed, **how** they work, and **when** to use them rather than memorizing APIs.

The repository deliberately stays within the defined **31-chapter multithreading/concurrency syllabus**.

---

## 📖 Notes Format

Each topic generally follows this structure:

1. Concept
2. Why it is needed
3. Syntax/API
4. Simple example
5. Important behavior
6. Common mistakes
7. Interview points
8. Key takeaways

Examples use **C++17** unless the topic specifically requires a **C++20** feature.

---

## ⭐ Progress

- [ ] Level 1 — Multithreading Fundamentals
- [ ] Level 2 — Thread Safety & Race Conditions
- [ ] Level 3 — Synchronization
- [ ] Level 4 — C++ Concurrency Utilities
- [ ] Level 5 — Atomic Programming
- [ ] Level 6 — Advanced Synchronization
- [ ] Level 7 — Thread Pools & Task-Based Concurrency
- [ ] Level 8 — C++ Memory Model
- [ ] Level 9 — Lock-Free & Advanced Atomics
- [ ] Level 10 — Performance & Scalability
- [ ] Level 11 — Real-World Concurrency Patterns
- [ ] Level 12 — Debugging & Testing
- [ ] Level 13 — Interview Preparation

---

## 📄 License

This repository is intended for personal learning, revision, and interview preparation.
