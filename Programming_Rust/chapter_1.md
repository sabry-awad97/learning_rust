# Systems programmers can have nice things

"Systems programmers can have nice things" is a phrase often used to describe the benefits of using the Rust programming language for systems programming tasks. Rust is a programming language that was designed to be safe, concurrent, and fast, making it well-suited for use in systems programming contexts.

One of the key features of Rust that makes it well-suited for systems programming is its emphasis on safety. Rust uses a type system that is statically checked at compile time, which helps to prevent common programming errors such as null or dangling pointer references. This can help to reduce the number of bugs in systems-level code, which can be especially important when dealing with low-level hardware interactions or resource-constrained environments.

Another key feature of Rust is its support for concurrent programming. Rust has a built-in concurrency model that is based on the idea of "ownership" and "borrowing," which allows for safe, concurrent execution of code. This can be especially useful for systems programming tasks, where concurrency is often a key concern.

Finally, Rust is designed to be fast, with a focus on performance and efficiency. Rust code is often as fast as equivalent code written in languages like C or C++, making it a good choice for high-performance systems programming tasks.

Overall, Rust is a powerful and flexible language that is well-suited for a wide range of systems programming tasks. Its emphasis on safety, concurrency, and performance make it an attractive choice for many systems programmers.

Here is some code in C:

```c
int main(int argc, char **argv) {
    unsigned long a[1];
    a[3] = 0x7ffff7b36cebUL;
    return 0;
}
```

- This code defines an array `a` of size `1` and attempts to assign a value to the fourth element of the array, which is _out of bounds_. Accessing array elements outside of the bounds of the array can lead to **undefined behavior** and may result in a segmentation fault or other runtime error.

Here is the equivalent code in Rust:

```rust
fn main() {
    let mut a: [u64; 1] = [0; 1];
    a[3] = 0x7ffff7b36cebu64;
}
```

- Like in C, this code defines an array `a` of size `1` and attempts to assign a value to the fourth element of the array, which is out of bounds. In Rust, this will result in a runtime panic and the program will **terminate**.

## Comparison between C and Rust:

- One major difference between Rust and C is the level of type safety and memory safety provided by each language. C is a relatively low-level language that does not have a strong type system and does not perform any runtime checking of memory accesses. This can make it prone to certain types of bugs, such as null or dangling pointer references, which can be difficult to detect and debug. In contrast, Rust has a statically-typed, borrow-checked type system that is designed to prevent these types of errors. As a result, Rust code is often considered to be safer and more robust than equivalent C code.

- Another difference between the two languages is their support for concurrency. C does not have any built-in support for concurrency, so developers must use external libraries or manually implement concurrency mechanisms such as mutexes and semaphores. Rust, on the other hand, has a built-in concurrency model based on the idea of "ownership" and "borrowing," which allows for safe, concurrent execution of code.

- Finally, Rust and C differ in terms of their performance and efficiency. C is generally considered to be a very fast language, with code that is often as fast as or faster than equivalent code written in other languages. Rust is also designed to be fast, with a focus on performance and efficiency, and Rust code is often as fast as or faster than equivalent C code.

Here is the summary between C and Rust:

| Feature       | Rust     | C                                  |
| ------------- | -------- | ---------------------------------- |
| Type safety   | Strong   | Weak                               |
| Memory safety | Strong   | Weak                               |
| Concurrency   | Built-in | None (external libraries required) |
| Performance   | Fast     | Fast                               |
