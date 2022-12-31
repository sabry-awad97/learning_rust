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

- Languages in the **"Safety First"** camp, such as Python, Javascript, Ruby, and Java, use garbage collection to _**automatically manage memory**_ and _**avoid dangling pointers**_. This can make it easier to write correct programs, but it also means that you have less control over when memory is freed and may need to rely on the garbage collector to determine when objects are no longer needed.

- Languages in the **"Control First"** camp, such as C and C++, give the programmer _**more control**_ over memory management but also _**require more care to avoid dangling pointers**_. This can be more challenging, but it also gives the programmer more control over the program's memory usage and can lead to more efficient programs.

Rust is a unique language in that it tries to combine the best of both worlds by offering both _**safe memory management**_ and _**control over memory usage**_. It does this through its ownership and borrowing system, which provides automatic memory management while also giving the programmer control over the lifetime of values and the ability to optimize memory usage.

Here is a summary of the key differences between the "Safety First" and "Control First" approaches to memory management, as well as how Rust handles these issues:

|                                | Safety First | Control First | Rust                               |
| ------------------------------ | ------------ | ------------- | ---------------------------------- |
| Automatic memory management    | Yes          | No            | **Yes** (through ownership system) |
| Control over memory usage      | No           | Yes           | **Yes** (through borrowing system) |
| Avoidance of dangling pointers | Yes          | No            | **Yes** (through borrowing system) |
| Predictability of memory usage | Yes          | No            | **Yes** (through ownership system) |

## Examples of ownership system

```rust
fn print_padovan() {
    let mut padovan = vec![1, 1, 1]; // allocated here
    for i in 3..10 {
        let next = padovan[i - 3] + padovan[i - 2];
        padovan.push(next);
    }
    println!("P(1..10) = {:?}", padovan);
} // dropped her
```

In this example, the variable `padovan` is a vector that is allocated on the heap. It is owned by the `print_padovan` function, so when the function finishes executing and control leaves the block in which padovan is declared, `padovan` is dropped and its memory is freed.

The ownership system ensures that values are not used after they are no longer needed, which can help prevent memory leaks and make it easier to reason about the lifetime of values in a Rust program.

```rust
fn main () {
    let point = Box::new((0.625, 0.5)); // point allocated here
    let label = format!("{:?}", point); // label allocated here
    assert_eq!(label, "(0.625, 0.5)");
} // both dropped here
```

In this example, the `Box` type is used to allocate a tuple of two `f64` values on the heap. The `Box::new` function takes a tuple as an argument and returns a `Box` pointing to the heap space where the tuple is stored.

Since the `Box` owns the heap space it points to, when the `Box` is dropped, it frees the space as well. In this example, the `point` variable is a `Box` pointing to the heap-allocated tuple, and the `label` variable is a string containing a formatted version of the tuple.

When the program finishes executing and control reaches the closing curly brace, both the `point` and `label` variables are dropped. This frees the heap-allocated tuple and the memory used for the `label` string.

Overall, the `Box` type is a useful tool for allocating values on the heap in Rust and managing their lifetimes. It allows you to store values on the heap and automatically free the memory when the `Box` is no longer needed.

```rust
fn main() {
    struct Person {
        name: String,
        birth: i32,
    }
    let mut composers = Vec::new();
    composers.push(Person {
        name: "Palestrina".to_string(),
        birth: 1525,
    });
    composers.push(Person {
        name: "Dowland".to_string(),
        birth: 1563,
    });
    composers.push(Person {
        name: "Lully".to_string(),
        birth: 1632,
    });
    for composer in &composers {
        println!("{}, born {}", composer.name, composer.birth);
    }
}
```

In this example, a struct called `Person` is defined with two fields: `name` and `birth`. The `name` field is a `String` and the `birth` field is an `i32`.

The `composers` variable is a `Vec` of `Person` structs, which is created using the `Vec::new` function. Three `Person` structs are then pushed onto the `composers` vector using the `push` method.

The `for` loop then iterates over the `composers` vector using a reference `&composer`. The `println!` macro is used to print the `name` and `birth` fields of each `composer`.

This code demonstrates how structs own their fields and how vectors own their elements. When the `composers` vector is dropped at the end of the program, the `Person` structs it contains are also dropped, along with their `name` and `birth` fields. This frees the memory used by the `Person` structs and their fields.

This cascading effect of dropping values is an important aspect of the ownership system in Rust. It helps ensure that all values are properly cleaned up and that memory is freed when it is no longer needed which can help prevent memory leaks and make it easier to reason about the lifetime of values in a Rust program.

It is also important to keep in mind that when a value is moved to a new owner, the original value is no longer valid and cannot be used. This is why, the `push` method is used to add new elements to the `composers` vector, rather than directly assigning them to elements of the vector. If a value is moved, any attempts to use it will result in a compile-time error.

The ownership relationships between values in a Rust program can be thought of as a tree, with each value having a single owner and the values it owns being its children. The root of the tree is a variable, and when that variable goes out of scope, the entire tree is dropped.

This tree structure helps ensure that values are properly cleaned up and that memory is freed when it is no longer needed. It also helps prevent common memory safety issues, such as dangling pointers and use of uninitialized memory, by enforcing the single-owner rule and preventing values from being accessed after they are dropped.

It is important to keep in mind that this tree structure is formed from a mixture of types and that the ownership relationships in Rust are strictly enforced. This helps ensure that the ownership relationships in a Rust program are easy to understand and predictable.

In Rust, values are automatically dropped when they are no longer needed, rather than being explicitly freed like in C and C++. This is because Rust's ownership system tracks the lifetime of values and automatically cleans up memory when it is no longer needed.

To remove a value from the ownership tree in Rust, you can either move it to a new owner or delete it from a data structure. For example, you can move a value to a new owner by assigning it to a new variable or passing it as an argument to a function. You can delete a value from a data structure by using methods like `pop` or `remove` or by using the `Drop` trait to define custom behavior for dropping values.

When a value is removed from the ownership tree in this way, Rust ensures that it is properly dropped, along with everything it owns.

While this may seem limiting compared to other programming languages that allow for more flexible relationships between values, Rust's ownership system allows the compiler to perform more powerful analyses on your code and provide stronger safety guarantees. In practice, Rust's ownership system is often sufficient for solving a wide range of problems, and the restrictions it imposes can help make your code more predictable and easier to reason about.

Yes, that is correct. While the basic concept of ownership in Rust is simple, the language extends it in several ways to make it more flexible and practical to use.

Some of the ways that Rust extends the concept of ownership include:

- Moving values: In Rust, you can move a value from one owner to another, which allows you to build, rearrange, and tear down the ownership tree as needed. This is often done using the `std::mem::swap` or `std::mem::replace` functions, or by simply assigning the value to a new variable.
- Copy types: Some simple types in Rust, such as integers, floating-point numbers, and characters, are marked as `Copy`. This means that when a value of one of these types is moved, the original value is not dropped and is still usable. This allows you to make copies of these types without having to worry about ownership issues.
- Reference-counted pointers: The `std::rc::Rc` and `std::sync::Arc` types in the standard library allow values to have multiple owners by keeping track of the number of references to the value. This can be useful in certain situations, but it comes with some restrictions to ensure that the ownership rules are not violated.
- References: In Rust, you can create a reference to a value, which is a non-owning pointer to the value with a limited lifetime. References allow you to access a value without taking ownership of it, and they are often used to pass values to functions or to work with data structures that do not have a single owner.

Overall, these extensions to the concept of ownership in Rust allow you to use values in more flexible and powerful ways while still maintaining the safety guarantees provided by the ownership system.

## Moves

In Rust, a move is the act of transferring ownership of a value from one owner to another. When a value is moved, the original value is no longer valid and cannot be used.

- Moves are often used to pass values to functions or to rearrange the ownership tree

  ```rust
  fn main() {
      let x = vec![1, 2, 3];
      let y = x;

      println!("{:?}", x);
  }
  ```

- Moves are also used when returning values from functions

  ```rust
  fn create_vec() -> Vec<i32> {
      let x = vec![1, 2, 3];
      x
  }

  fn main() {
      let y = create_vec();
      println!("{:?}", y);
  }
  ```

In Rust, most types are moved rather than copied when they are assigned to a new variable, passed to a function, or returned from a function. This means that the source relinquishes ownership of the value to the destination, and the value's lifetime is now controlled by the destination

Yes, it is true that the use of moves in Rust may be surprising to some people, as assignment is a fundamental operation in programming languages that is typically well-defined. However, different programming languages handle assignment in different ways, and Rust's decision to use moves is a result of its design goals and the safety guarantees it aims to provide.

In many languages, assignment simply copies the value from the source to the destination, leaving the original value unchanged. This is known as "copy semantics." However, Rust uses "move semantics," which means that assignment transfers ownership of the value from the source to the destination, rendering the original value invalid.

One reason for this is that Rust's design goals include memory safety and the prevention of common memory safety issues such as dangling pointers and use of uninitialized memory. Using move semantics helps ensure that values are not used after they are no longer needed and that memory is properly managed.

Additionally, Rust's move semantics can make it easier to reason about the lifetime of values in your code. Since a value can only have one owner, it is clear when a value will be dropped and when its memory will be freed. This can make it easier to understand and predict the behavior of your code.

Overall, Rust's use of move semantics is a key part of its ownership system and helps ensure memory safety and make the lifetime of values in your code more predictable.

## Stack and Heap

```rust
fn main() {
    let s = vec!["udon".to_string(), "ramen".to_string(), "soba".to_string()];
    let t = s;
    let u = s;
}
```

The first line creates a new vector `s` on the heap, containing three strings. Each of these strings is also allocated on the heap and contains the text "udon", "ramen", and "soba" respectively.

In the second line, the value of `s` is moved to `t`. This means that the value of `s` is no longer valid, and attempting to use it will result in a compile-time error. The vector `t` now owns the values that were previously owned by s.

In the third line, the value of s is used agin. This would result in a compile-time error because the value of s is no longer valid after it has been moved.

After these three lines are executed, the ownership tree in the heap looks like this:

```rust
t  ->  ["udon", "ramen", "soba"]
```

The vector t owns the strings "udon", "ramen", and "soba".

In the stack, the variables s and t are all valid and contain references to the vectors on the heap. However, the value of s has been moved to t, so attempting to use s after this point would result in a compile-time error.

Before the move, the stack and heap would be as follows:

```rust
s: +------------+
    |  Pointer   |  ---->  [Heap]
    +------------+
    |  Length    |
    +------------+
    |  Capacity  |
    +------------+

t: +------------+
    |  Pointer   |
    +------------+
    |  Length    |
    +------------+
    |  Capacity  |
    +------------+

Heap:
+-------------------+
|  "udon"           |
+-------------------+
|  "ramen"          |
+-------------------+
|  "soba"           |
+-------------------+
```

The variable `s` contains a pointer to the heap-allocated array that contains the elements of the vector, as well as the length and capacity of the array. The variable `t` is uninitialized and does not contain a valid pointer. The heap-allocated array contains the elements of the vector, each element being a string slice containing the text of the element.

After the move, the stack and heap would be as follows:

```rustStack:
s: +------------+
    |  Pointer   |
    +------------+
    |  Length    |
    +------------+
    |  Capacity  |
    +------------+

t: +------------+
    |  Pointer   |  ---->  [Heap]
    +------------+
    |  Length    |
    +------------+
    |  Capacity  |
    +------------+

Heap:
+-------------------+
|  "udon"           |
+-------------------+
|  "ramen"          |
+-------------------+
|  "soba"           |
+-------------------+
```

The value of `s` has been moved to `t`, so the variable `s` is now uninitialized and does not contain a valid pointer. The variable `t` now contains a pointer to the heap-allocated array that was previously owned by `s`, as well as the length and capacity of the array. The values in the heap are unchanged, as the elements themselves are not moved. Only the ownership of the heap-allocated array is transferred from `s` to `t`.

To make u vector you must ask for a copy

```rust
let s = vec!["udon".to_string(), "ramen".to_string(), "soba".to_string()];
let t = s.clone();
let u = s.clone();
```

In this version, `s` is an immutable variable and its value is cloned into `t` and u using the clone method. This creates new values on the heap that are copies of the original value in `s`.

The ownership tree in the heap after these three lines are executed would look like this:

```rust
       u  ->  ["udon", "ramen", "soba"]
        \
s  ->  ["udon", "ramen", "soba"]
        \
       t  ->  ["udon", "ramen", "soba"]
```

In this version of the code, `s`, `t`, and `u` all own separate values on the heap, and the original value of `s` is still valid and can be used after the clones are created.

```rust
struct Person {
    name: String,
    birth: i32,
}
let mut composers = Vec::new();
composers.push(Person {
    name: "Palestrina".to_string(),
    birth: 1525,
});
```

```rust
Variable       |  Value
---------------+------------------------------------------------
composers      |  +------------+
               |  |  Pointer   |  ---->  [Heap]
               |  +------------+
               |  |  Length    |
               |  +------------+
               |  |  Capacity  |
               |  +------------+
               |  |  Person 1  |  +------------+
               |  |            |  |  Pointer   |  ---->  [Heap]
               |  |            |  +------------+
               |  |            |  |  Length    |
               |  |            |  +------------+
               |  |            |  |  Capacity  |
               |  |            |  +------------+
               |  |            |  |"Palestrina"     |  +------------+
               |  |            |  |                 |  |  Pointer   |  ---->  [Heap]
               |  |            |  |                 |  +------------+
               |  |            |  |                 |  |  Length    |
               |  |            |  |                 |  +------------+
               |  |            |  |                 |  |  Capacity  |
               |  |            |  |                 |  +------------+
               |  |            |  |                 |  |"Palestrina"|
               |  |            |  |                 |  +------------+
               |  |            |  |                 |
               |  |            |  +------------+
               |  +------------+


```

The variable `composers` is a vector that owns a heap-allocated array containing one element, which is a struct `Person`. The struct `Person` owns a heap-allocated string slice containing the text of its `name` field.
