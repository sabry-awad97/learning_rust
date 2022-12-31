# Ownership and Moves

In Rust, ownership is a central concept that governs how the borrowing and mutability of variables are managed in the program. Ownership is based on the idea that each value in Rust has a single owner, and the owner is responsible for the value's lifetime.

One of the key features of ownership is that it ensures that a value is automatically cleaned up when it is no longer needed, a process known as "dropping". This helps prevent memory leaks and makes it easier to reason about the lifetime of values in a Rust program.

One aspect of ownership is "move semantics". In Rust, when a value is assigned to a new variable, the ownership of the value is transferred to the new variable. This is known as a "move". For example:

```rust
let x = String::from("hello");
let y = x;
```

In this code, the value of `x` is moved to `y`, and `x` is no longer valid. This is because Rust assumes that the original owner (in this case, `x`) is no longer interested in the value and it is safe to transfer ownership to the new owner (`y`).

Move semantics are important because they allow Rust to optimize memory usage by avoiding unnecessary copies of data. However, they also mean that once a value has been moved, the original value can no longer be used.

You can use the `clone` method to create a deep copy of a value if you need to use the original value after it has been moved. This can be useful in cases where you need to retain ownership of a value while also using it in multiple places

## Safety First vs Control First

When it comes to **managing memory**, there are two characteristics we’d like from our programing languages:

- We'd like memory to be freed promptly, at a time of our choosing. This gives us **control** over the program's memory consumption.
- We never want to use a pointer to an object after it's been freed (_dangling pointer_). This would be undefined behavior, leading to crashes and security holes.

  - A **dangling pointer** is a pointer that points to memory that has already been deallocated (freed). Using a dangling pointer can lead to undefined behavior and can cause a program to crash or produce incorrect results.

    For example, consider the following code in a language that does not have garbage collection:

    ```c++
    int *p = malloc(sizeof(int));
    *p = 42;
    free(p);
    printf("%d\n", *p);
    ```

    In this code, the pointer `p` is initially pointing to a block of memory that has been allocated with `malloc`. The value `42` is then written to this memory location. However, the memory is then freed with `free`, which invalidates the pointer.

    The final line of code then attempts to dereference the pointer `p`, which is a dangling pointer at this point. This can lead to undefined behavior, such as a program crash or incorrect output.

These two characteristics of memory management can be mutually exclusive, and many programming languages choose to prioritize one over the other.

- Languages in the **"Safety First"** camp, such as Python, Javascript, Ruby, and Java, use garbage collection to _automatically manage memory_ and _avoid dangling pointers_. This can make it easier to write correct programs, but it also means that you have less control over when memory is freed and may need to rely on the garbage collector to determine when objects are no longer needed.

- Languages in the **"Control First"** camp, such as C and C++, give the programmer _more control_ over memory management but also _require more care to avoid dangling pointers_. This can be more challenging, but it also gives the programmer more control over the program's memory usage and can lead to more efficient programs.

Rust is a unique language in that it tries to combine the best of both worlds by offering both **safe memory management** and **control over memory usage**. It does this through its ownership and borrowing system, which provides automatic memory management while also giving the programmer control over the lifetime of values and the ability to optimize memory usage.

Here is a summary of the key differences between the "Safety First" and "Control First" approaches to memory management, as well as how Rust handles these issues:

|                                | Safety First | Control First | Rust                           |
| ------------------------------ | ------------ | ------------- | ------------------------------ |
| Automatic memory management    | Yes          | No            | Yes (through ownership system) |
| Control over memory usage      | No           | Yes           | Yes (through borrowing system) |
| Avoidance of dangling pointers | Yes          | No            | Yes (through borrowing system) |
| Predictability of memory usage | Yes          | No            | Yes (through ownership system) |
