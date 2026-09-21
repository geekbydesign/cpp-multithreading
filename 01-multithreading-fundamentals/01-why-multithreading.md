# Why Multithreading?

## 1. What is a Thread?

A **thread** is the smallest unit of execution within a process.

A process can contain one or more threads.

```text
Process
├── Thread 1
├── Thread 2
└── Thread 3
```

Threads within the same process share resources such as:

- Heap memory
- Global/static variables
- Open files
- Other process resources

Each thread has its own:

- Stack
- Registers
- Program counter
- Thread-local state

---

## 2. Single-Threaded vs Multithreaded

### Single-threaded

Only one thread executes the program's work.

```text
Task A → Task B → Task C
```

Tasks execute sequentially.

### Multithreaded

Multiple threads can execute different tasks concurrently.

```text
Thread 1 → Task A
Thread 2 → Task B
Thread 3 → Task C
```

---

## 3. Concurrency vs Parallelism

### Concurrency

Multiple tasks make progress during overlapping periods.

They may not literally execute at the same instant.

```text
CPU
│
├── Task A
├── Task B
├── Task A
└── Task B
```

### Parallelism

Multiple tasks execute at the same time on different CPU cores.

```text
Core 1 → Task A
Core 2 → Task B
```

A multithreaded program can provide concurrency, parallelism, or both depending on the hardware and workload.

---

## 4. Why Use Multithreading?

### 4.1 Improve Responsiveness

A long-running task can run in another thread while the main thread remains responsive.

Example:

```text
Main thread → User interface
Worker thread → File processing
```

### 4.2 Perform Independent Tasks Concurrently

Independent operations can run at the same time.

```text
Thread 1 → Read sensor
Thread 2 → Process network data
Thread 3 → Log information
```

### 4.3 Utilize Multiple CPU Cores

CPU-intensive work can sometimes be divided across multiple cores.

```text
Core 1 → Part 1
Core 2 → Part 2
Core 3 → Part 3
Core 4 → Part 4
```

### 4.4 Hide Waiting Time

While one thread waits for I/O, another thread can perform useful work.

```text
Thread 1 → Waiting for file/network
Thread 2 → Processing data
```

---

## 5. When Multithreading May Not Help

Multithreading is not automatically faster.

Possible costs include:

- Thread creation overhead
- Context switching
- Synchronization overhead
- Mutex contention
- Cache effects
- More complex debugging
- Race conditions
- Deadlocks

For a very small task, creating another thread can cost more than simply executing the task directly.

---

## 6. Process vs Thread

| Process | Thread |
|---|---|
| Independent execution environment | Execution unit inside a process |
| Separate address space | Shares process address space |
| More expensive to create | Usually cheaper to create |
| Communication is more expensive | Communication can be easier |
| Failure is generally isolated | A serious thread failure can affect the process |

---

## 7. Context Switching

The operating system may switch the CPU from one thread to another.

```text
Thread A
   ↓
Save state
   ↓
Thread B
   ↓
Execute
   ↓
Restore Thread A
```

This is called a **context switch**.

Context switching has overhead, so creating many threads does not necessarily improve performance.

---

## 8. Thread Scheduling

The operating system scheduler decides which runnable thread gets CPU time.

Scheduling depends on factors such as:

- Number of CPU cores
- Thread priority
- OS scheduling policy
- Current system load
- Whether threads are waiting for I/O

The exact scheduling behavior should not be assumed from the order in which threads are created.

---

## 9. Shared Resources

Threads in the same process can access shared data.

Example:

```cpp
int counter = 0;
```

If multiple threads modify `counter` simultaneously, synchronization may be required.

This leads to important multithreading problems:

- Race conditions
- Data races
- Deadlocks
- Synchronization problems

These concepts are covered in later chapters.

---

## 10. Key Takeaways

- A thread is an execution unit inside a process.
- Threads in the same process share many resources.
- Each thread has its own stack and execution state.
- Concurrency means tasks can make progress during overlapping periods.
- Parallelism means tasks execute simultaneously.
- Multithreading can improve responsiveness and CPU utilization.
- Multithreading also introduces synchronization and debugging complexity.
- More threads do not automatically mean better performance.
