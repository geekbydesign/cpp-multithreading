# Passing Data to Threads

## 1. Introduction

When creating a thread with `std::thread`, data can be passed to the thread function.

```cpp
std::thread t(function, argument1, argument2);
```

Understanding how these arguments are copied, referenced, moved, and kept alive is important for writing safe multithreaded code.

---

## 2. Passing by Value

By default, thread arguments are copied/moved into the thread's internal storage.

```cpp
void print(int value)
{
    std::cout << value;
}

int number = 10;

std::thread t(print, number);

t.join();
```

The thread receives its own value.

Changing the thread's local parameter does not change the original variable.

---

## 3. Passing by Reference

Suppose the function expects a reference:

```cpp
void increment(int& value)
{
    ++value;
}
```

Use `std::ref()`:

```cpp
#include <functional>

int value = 10;

std::thread t(increment, std::ref(value));

t.join();
```

Now both the main thread and worker thread refer to the same object.

### Important

Shared access introduces synchronization concerns.

If multiple threads modify the same object concurrently without proper synchronization, a data race can occur.

---

## 4. Passing by Const Reference

For a function like:

```cpp
void print(const std::string& text)
{
    std::cout << text;
}
```

Use `std::cref()` when you specifically want reference semantics:

```cpp
std::string text = "Hello";

std::thread t(print, std::cref(text));

t.join();
```

---

## 5. Passing a Pointer

A pointer can also be passed:

```cpp
void modify(int* value)
{
    ++(*value);
}

int number = 10;

std::thread t(modify, &number);

t.join();
```

The pointer itself is passed to the thread, but it points to the original object.

Therefore, the object's lifetime must remain valid while the thread uses it.

---

## 6. Passing Objects

Objects can be passed to a thread.

```cpp
class Worker
{
public:
    void process()
    {
        // ...
    }
};

Worker worker;

std::thread t(&Worker::process, &worker);

t.join();
```

Here the thread calls `process()` on the existing `worker` object.

The `worker` object must remain alive until the thread finishes.

---

## 7. Passing an Object by Value

A copy of an object can also be passed.

```cpp
void process(Worker worker)
{
    // Work with local copy
}

Worker worker;

std::thread t(process, worker);

t.join();
```

The thread works with its own object copy.

This can be useful when you want to avoid sharing mutable state.

---

## 8. Passing an Object by Reference

Use `std::ref()` when the thread should operate on the original object.

```cpp
void process(Worker& worker)
{
    // Work with original object
}

Worker worker;

std::thread t(process, std::ref(worker));

t.join();
```

Now the thread accesses the same object.

If multiple threads access or modify it, synchronization may be required.

---

## 9. Passing Temporary Objects

Temporary objects can be passed normally.

```cpp
std::thread t(process, Worker{});

t.join();
```

The thread receives the required argument according to `std::thread`'s argument handling rules.

---

## 10. Passing Move-Only Objects

Some objects cannot be copied.

Example:

```cpp
std::unique_ptr<int> ptr = std::make_unique<int>(10);
```

A `unique_ptr` must be moved into the thread.

```cpp
void process(std::unique_ptr<int> ptr)
{
    std::cout << *ptr;
}

std::thread t(process, std::move(ptr));

t.join();
```

After the move:

```cpp
ptr == nullptr
```

The thread now owns the `unique_ptr`.

---

## 11. Lambda Capture

Instead of passing arguments explicitly, a lambda can capture variables.

### Capture by value

```cpp
int value = 10;

std::thread t([value] {
    std::cout << value;
});

t.join();
```

The lambda has its own copy of `value`.

### Capture by reference

```cpp
int value = 10;

std::thread t([&value] {
    ++value;
});

t.join();
```

The lambda accesses the original variable.

The referenced object must remain alive until the thread finishes.

---

## 12. The Lifetime Problem

This is one of the most important concepts when passing data to threads.

Bad example:

```cpp
std::thread createThread()
{
    int value = 10;

    return std::thread([&value] {
        std::cout << value;
    });
}
```

`value` is a local variable.

After `createThread()` returns, `value` is destroyed.

The thread may then access a dangling reference.

### Safer approach

Capture by value:

```cpp
std::thread createThread()
{
    int value = 10;

    return std::thread([value] {
        std::cout << value;
    });
}
```

Now the lambda owns its copy.

---

## 13. Reference Lifetime Rule

When using:

```cpp
std::ref()
```

or:

```cpp
[&]
```

make sure the referenced object remains alive for the entire period in which the thread can access it.

Think:

```text
Referenced object lifetime
        |
        |--------------------------|
        |                          |
     thread starts             thread finishes
```

The object must remain valid throughout the thread's access.

---

## 14. Sharing Data Between Threads

Consider:

```cpp
int counter = 0;

std::thread t1([&] {
    ++counter;
});

std::thread t2([&] {
    ++counter;
});

t1.join();
t2.join();
```

Both threads access the same variable.

This creates a synchronization problem because `counter` is shared mutable state.

The solution may involve:

- `std::mutex`
- `std::atomic`
- Another appropriate synchronization mechanism

These are covered in later chapters.

---

## 15. Copy vs Reference vs Move

| Method | What thread receives | Original object affected? |
|---|---|---|
| Value | Copy/moved value | Usually no |
| `std::ref()` | Reference | Yes |
| `std::cref()` | Const reference | Read-only access |
| Pointer | Pointer to object | Yes, if object is modified |
| `std::move()` | Ownership/value transferred | Source becomes moved-from |

---

## 16. Practical Guideline

When deciding how to pass data:

```text
Need independent data?
        ↓
Pass by value

Need to modify existing object?
        ↓
std::ref()

Need read-only access to existing object?
        ↓
std::cref()

Need transfer ownership?
        ↓
std::move()

Using pointer?
        ↓
Ensure object lifetime is valid
```

---

## 17. Common Mistakes

### Mistake 1: Forgetting `std::ref`

```cpp
std::thread t(function, value);
```

If `function` expects `int&`, this is not how you request reference semantics.

Use:

```cpp
std::thread t(function, std::ref(value));
```

### Mistake 2: Capturing a local variable by reference

```cpp
std::thread t([&] {
    use(localVariable);
});
```

If `localVariable` is destroyed before the thread finishes, this is unsafe.

### Mistake 3: Sharing mutable data without synchronization

Multiple threads modifying the same object can cause a data race.

### Mistake 4: Moving an object and then using it as if ownership was unchanged

After:

```cpp
std::move(ptr)
```

the source object is in a valid but moved-from state. Do not assume it still owns the original resource.

---

## 18. Key Takeaways

- `std::thread` normally stores copies/moved values of its arguments.
- Use `std::ref()` for reference semantics.
- Use `std::cref()` for const-reference semantics.
- `std::move()` can transfer ownership of move-only objects.
- Lambda captures follow their own value/reference capture rules.
- Always consider object lifetime when passing references or pointers.
- Shared mutable data requires synchronization.
- Passing by value can often simplify thread safety because the thread gets independent data.
