# C++ Multithreading

A structured set of **C++ multithreading notes**, covering concepts from beginner fundamentals to advanced concurrency, synchronization, atomics, the C++ memory model, lock-free programming, performance, debugging, and interview preparation.

The notes are written primarily for **C++17**, with **C++20 concurrency features** such as `std::jthread`, semaphores, latches, and barriers covered separately.

## Learning Goals

This repository is intended to help build a strong understanding of:

* C++ threads and concurrency
* Thread lifecycle and thread management
* Race conditions and data races
* Mutexes and locking
* Deadlocks
* Condition variables
* Producer-consumer patterns
* Futures, promises, and `std::async`
* Atomic operations
* Memory ordering
* Advanced synchronization primitives
* Thread pools
* C++20 `std::jthread`
* C++ memory model
* Cache and hardware-level concepts
* Lock-free programming
* Concurrency design patterns
* Performance optimization
* Debugging and testing concurrent programs
* C++ multithreading interview preparation

## How to Use This Repository

Follow the chapters in order.

Start with:

```text
01-multithreading-fundamentals
        ↓
02-thread-safety
        ↓
03-synchronization
        ↓
04-concurrency-utilities
        ↓
05-atomics
        ↓
06-advanced-synchronization
        ↓
07-thread-pools
        ↓
08-cpp-memory-model
        ↓
09-lock-free-programming
        ↓
10-performance
        ↓
11-concurrency-patterns
        ↓
12-debugging-testing
        ↓
13-interview-preparation
```

The directory contains practical implementations to reinforce the concepts.

## Recommended Learning Approach

For each topic, focus on four questions:

1. **What problem does it solve?**
2. **How does it work?**
3. **What can go wrong?**
4. **When should it be used?**

The goal is not just to memorize APIs such as `std::mutex` or `std::condition_variable`, but to understand the concurrency problems they solve.

## C++ Versions

### C++17

The core of the repository uses C++17 concepts and APIs, including:

* `std::thread`
* `std::mutex`
* `std::lock_guard`
* `std::unique_lock`
* `std::condition_variable`
* `std::future`
* `std::promise`
* `std::async`
* `std::packaged_task`
* `std::atomic`
* `std::shared_mutex`

### C++20

C++20-specific topics are covered where applicable:

* `std::jthread`
* `std::stop_token`
* `std::counting_semaphore`
* `std::binary_semaphore`
* `std::latch`
* `std::barrier`

## Repository Structure

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
│   └── 03-aba-problem.md
│
├── 10-performance/
│   ├── 01-performance.md
│   └── 02-cache-false-sharing.md
│
├── 11-concurrency-patterns/
│
├── 12-debugging-testing/
│
├── 13-interview-preparation/
│
└── projects/
    ├── thread-safe-queue/
    ├── producer-consumer/
    └── thread-pool/
```

## Practical Projects

The `projects/` directory contains implementations that apply concepts from the notes.

### Thread-Safe Queue

A queue designed for concurrent producers and consumers.

Concepts:

* Mutex
* Condition variable
* Thread synchronization
* Safe access to shared data

### Producer-Consumer

A practical implementation of the producer-consumer pattern.

Concepts:

* Shared queue
* `std::mutex`
* `std::condition_variable`
* Multiple producers
* Multiple consumers
* Graceful shutdown

### Thread Pool

A reusable pool of worker threads that executes submitted tasks.

Concepts:

* Worker threads
* Task queue
* Mutex
* Condition variable
* Futures
* Task management
* Graceful shutdown

## Important Concepts

The repository gradually progresses from simple thread creation:

```cpp
std::thread t(task);
t.join();
```

to synchronization:

```cpp
std::lock_guard<std::mutex> lock(mutex);
```

then condition-based synchronization:

```cpp
condition_variable.wait(lock, predicate);
```

and eventually advanced atomic and lock-free techniques:

```cpp
std::atomic<int> counter;
```

The goal is to understand **why each mechanism is needed**, not just how to use its syntax.

## Interview Preparation

The final section focuses on commonly asked C++ multithreading topics, including:

* Process vs thread
* Concurrency vs parallelism
* Race condition vs data race
* Mutex
* `lock_guard` vs `unique_lock`
* Deadlock
* Condition variables
* Producer-consumer
* Atomic operations
* Compare-and-swap
* Memory ordering
* Futures and promises
* `std::async`
* Thread pools
* `std::jthread`
* Semaphores
* Cache coherence
* False sharing
* Lock-free programming

## Revision Philosophy

These notes are intended as a **long-term reference and revision resource**.

The focus is on:

* Clear explanations
* Small C++ examples
* Important rules
* Common mistakes
* Comparisons between related concepts
* Interview-oriented points
* Practical implementations

Complex topics are introduced gradually so that advanced concepts build on the fundamentals.

---

**Language:** C++17 / C++20
**Focus:** Multithreading, Concurrency & Parallel Programming
**Purpose:** Learning, Revision, Interview Preparation & Practical Development
