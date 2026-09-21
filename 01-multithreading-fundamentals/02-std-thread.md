# std::thread

## 1. Introduction

C++ provides `std::thread` in the `<thread>` header for creating and managing threads.

```cpp
#include <thread>
```

A thread executes a callable object such as:

- Function
- Lambda
- Function object
- Callable member function

---

## 2. Creating a Thread

```cpp
#include <iostream>
#include <thread>

void task()
{
    std::cout << "Worker thread\n";
}

int main()
{
    std::thread t(task);

    t.join();

    return 0;
}
```

```text
main thread
     |
     +---- creates ----> worker thread
                           |
                           +---- task()
     |
     +---- join()
```

---

## 3. join()

`join()` makes the calling thread wait until the target thread finishes.

```cpp
std::thread t(task);

t.join();
```

After `join()` returns, the thread has completed.

### Important

A `std::thread` object must be joined or detached before it is destroyed if it is still joinable.

---

## 4. detach()

`detach()` separates the thread from the `std::thread` object.

```cpp
std::thread t(task);

t.detach();
```

The thread continues independently.

### Be Careful

Detached threads make lifetime management harder.

For example:

```cpp
void task()
{
    // Uses some object
}
```

If the object used by `task()` is destroyed before the detached thread finishes, the thread may access invalid memory.

Prefer `join()` unless you have a clear reason to detach.

---

## 5. joinable()

Use `joinable()` to check whether a `std::thread` object represents an active thread that has not been joined or detached.

```cpp
if (t.joinable())
{
    t.join();
}
```

This is useful during cleanup.

---

## 6. Thread with Lambda

```cpp
std::thread t([] {
    std::cout << "Hello from thread\n";
});

t.join();
```

Lambdas are commonly used for small thread tasks.

---

## 7. Passing Arguments

Arguments can be passed directly when creating the thread.

```cpp
void printNumber(int n)
{
    std::cout << n;
}

int main()
{
    std::thread t(printNumber, 10);

    t.join();
}
```

The value `10` is passed to the thread function.

---

## 8. Passing Multiple Arguments

```cpp
void add(int a, int b)
{
    std::cout << a + b;
}

std::thread t(add, 10, 20);

t.join();
```

---

## 9. Passing by Reference

By default, arguments passed to a thread are copied.

To pass an argument by reference, use `std::ref`.

```cpp
#include <functional>
#include <thread>

void increment(int& value)
{
    ++value;
}

int value = 10;

std::thread t(increment, std::ref(value));

t.join();
```

Now the thread modifies the original `value`.

---

## 10. Passing by Const Reference

Use `std::cref` when you need to pass a reference to const.

```cpp
#include <functional>

void print(const int& value)
{
    std::cout << value;
}

int value = 10;

std::thread t(print, std::cref(value));

t.join();
```

---

## 11. std::this_thread

The `<thread>` library also provides utilities for the currently executing thread.

### get_id()

```cpp
std::cout << std::this_thread::get_id();
```

### sleep_for()

```cpp
std::this_thread::sleep_for(
    std::chrono::seconds(1)
);
```

### sleep_until()

```cpp
auto time = std::chrono::steady_clock::now()
          + std::chrono::seconds(1);

std::this_thread::sleep_until(time);
```

### yield()

```cpp
std::this_thread::yield();
```

`yield()` tells the scheduler that the current thread is willing to give up its current execution opportunity.

It does not guarantee that another specific thread will run.

---

## 12. std::thread::id

A thread can be identified using `std::thread::id`.

```cpp
std::thread t(task);

std::cout << t.get_id();

t.join();
```

The current thread ID can be obtained using:

```cpp
std::this_thread::get_id();
```

---

## 13. Thread Lifetime

Consider:

```cpp
void task()
{
    // Work
}

int main()
{
    std::thread t(task);
}
```

This is incorrect because `t` is destroyed while it is still joinable.

The program will call `std::terminate()`.

Correct:

```cpp
std::thread t(task);

t.join();
```

---

## 14. join vs detach

| `join()` | `detach()` |
|---|---|
| Waits for thread completion | Does not wait |
| Easy to reason about lifetime | Lifetime is harder to manage |
| Thread completion is guaranteed before return | Thread continues independently |
| Usually preferred | Use only when appropriate |

---

## 15. Common Mistakes

### Mistake 1: Forgetting join()

```cpp
std::thread t(task);
// Missing join/detach
```

### Mistake 2: Using references incorrectly

```cpp
void task(int& x)
{
    // ...
}
```

Remember to use:

```cpp
std::ref(x)
```

when passing the reference to `std::thread`.

### Mistake 3: Detaching without considering lifetime

A detached thread may outlive objects it uses.

### Mistake 4: Assuming execution order

Creating:

```cpp
std::thread t1(task1);
std::thread t2(task2);
```

does not guarantee that `task1` executes before `task2`.

---

## 16. Key Takeaways

- `std::thread` is the basic C++ thread abstraction.
- Use `join()` to wait for a thread.
- Use `detach()` only when independent lifetime is intentional.
- Use `joinable()` before joining when necessary.
- Thread arguments are normally copied.
- Use `std::ref` for references.
- Use `std::cref` for const references.
- `std::this_thread` provides utilities for the current thread.
- Thread execution order is generally not deterministic.
