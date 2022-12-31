# References

References in Rust are denoted with an `&` symbol and are used to borrow data from a source, rather than taking ownership of the data. This means that when a reference goes out of scope, it does not cause the data it refers to to be deallocated.

References are a way to allow multiple parts of your code to read or write to a single piece of data without requiring ownership of that data. This can be useful in many situations, such as when you want to pass a large piece of data to a function without copying it, or when you want to allow multiple parts of your code to access a single resource.

Here is an example of using a reference:

```rust
fn main() {
    let s = String::from("hello");
    let len = calculate_length(&s);
    println!("The length of '{}' is {}.", s, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```

In this example, the `calculate_length` function takes a reference to a `String` as its argument. This reference is denoted with the `&` symbol. The function does not take ownership of the `String`, so it is still available to be used after the function call.

It is also possible to use references to mutate the data they refer to. This is done by using a mutable reference, which is denoted with the `&mut` symbol. Here is an example of using a mutable reference:

```rust
fn main() {
    let mut s = String::from("hello");
    change(&mut s);
}

fn change(s: &mut String) {
    s.push_str(", world!");
}
```

In this example, the `change` function takes a mutable reference to a `String` as its argument. This mutable reference is denoted with the `&mut` symbol. The function is able to modify the value of the `String` through the reference.

The borrow checker in Rust enforces rules around the use of references to ensure the safety and correctness of code.

**These rules include**:

1. You cannot have a mutable reference and a non-mutable reference to the same data in the same scope:

```rust
let mut x = 5;
let y = &x; // Non-mutable reference to x
let z = &mut x; // Mutable reference to x
```

This code will not compile, because it tries to create both a non-mutable reference (`y`) and a mutable reference (`z`) to the same data (`x`) in the same scope.

1. You cannot have multiple mutable references to the same data in the same scope:

```rust
let mut x = 5;
let y = &mut x; // Mutable reference to x
let z = &mut x; // Mutable reference to x
```

This code will not compile, because it tries to create multiple mutable references (`y` and `z`) to the same data (`x`) in the same scope.

1. When you have a mutable reference to some data, you are the only owner of that data, and you have exclusive access to it:

```rust
let mut x = 5;
let y = &mut x; // Mutable reference to x
x = 6; // Valid, because y is the only reference to x
*y = 7; // Valid, because y is a mutable reference to x
```

In this example, the mutable reference `y` has exclusive access to `x`, so it is able to modify `x` directly or through the reference.

1. When you have a non-mutable reference to some data, you do not have exclusive access to it, and other parts of your code may also have non-mutable references to the same data:

```rust
let x = 5;
let y = &x; // Non-mutable reference to x
let z = &x; // Non-mutable reference to x
```

``

In this example, both `y` and `z` are non-mutable references to `x`, so they do not have exclusive access to `x`. Other parts of the code may also have non-mutable references to `x`.

1. You cannot create a reference to data that does not have a clear owner:

```rust
fn foo() -> &i32 {
    let x = 5;
    &x
} // x goes out of scope here
```

``

This code will not compile, because `x` goes out of scope at the end of the `foo` function, but a reference to `x` is returned. This creates a dangling reference, which can cause undefined behavior.

1. You cannot create a reference to data that is not valid:

```rust
let x = vec![1, 2, 3];
let y = &x[3]; // Index out of bounds`

```

This code will not compile, because it tries to create a reference to an element at index 3 in a vector with only 3 elements. This is not a valid operation, and trying to create a reference to the invalid data can cause undefined behavior.

Here is a summary of the rules for using references in Rust:

| Rule | Description                                                                                                                                                                     | Example                                          |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| 1    | You cannot have a mutable reference and a non-mutable reference to the same data in the same scope.                                                                             | `let mut x = 5; let y = &x; let z = &mut x;`     |
| 2    | You cannot have multiple mutable references to the same data in the same scope.                                                                                                 | `let mut x = 5; let y = &mut x; let z = &mut x;` |
| 3    | When you have a mutable reference to some data, you are the only owner of that data, and you have exclusive access to it.                                                       | `let mut x = 5; let y = &mut x; x = 6; *y = 7;`  |
| 4    | When you have a non-mutable reference to some data, you do not have exclusive access to it, and other parts of your code may also have non-mutable references to the same data. | `let x = 5; let y = &x; let z = &x;`             |
| 5    | You cannot create a reference to data that does not have a clear owner.                                                                                                         | `fn foo() -> &i32 { let x = 5; &x }`             |
| 6    | You cannot create a reference to data that is not valid.                                                                                                                        | `let x = vec![1, 2, 3]; let y = &x[3];`          |

Here is a list of terms that are commonly used when discussing references in Rust:

- Reference: A non-owning pointer type that allows you to borrow data from a source without taking ownership of it. Denoted with the `&` symbol.
- Mutable reference: A reference that allows you to modify the data it refers to. Denoted with the `&mut` symbol.
- Referent: The data that is being referred to by a reference.
- Borrow: The act of using a reference to borrow data from a source without taking ownership of it.
- Borrow checker: A system in Rust that enforces rules around the use of references in order to ensure the safety and correctness of code.
- Dangling reference: A reference to data that no longer has a clear owner. This can cause undefined behavior.

```rust
fn main() {
    let s = String::from("hello"); // s is a referent
    let r = &s; // Borrow s with a reference
    let t = &r; // t is also a reference to s
    println!("{}", t);
}
```

In this example, we borrow the data in `s` with a reference `r`, and then create another reference `t` to `r`. Both `r` and `t` are references to the same referent, `s`.

```rust
fn main() {
    let s = String::from("hello");
    let r = &s; // Borrow s with a reference
    let t = r; // t is also a reference to s
    println!("{}", t);
}
```

In this example, we borrow the data in `s` with a reference `r`, and then create another reference `t` to `r`. Both `r` and `t` are references to the same referent, `s`. However, this code will not compile, because Rust does not allow references to be directly assigned to each other. This is to prevent dangling references, which can cause undefined behavior.

In Rust, references must never outlive their referents. This means that you must ensure that a reference cannot possibly outlive the value it points to. This is important because if a reference outlives its referent, it becomes a dangling reference, which can cause undefined behavior.

To emphasize the idea that a reference is borrowing data from a source, Rust refers to creating a reference as borrowing the value. This helps to emphasize the fact that the reference does not own the data, and that the data must eventually be returned to its owner.

When we say that "references must never outlive their referents," we mean that the lifetime of a reference must be strictly shorter than the lifetime of the data it refers to. This means that a reference cannot be used after the data it refers to has been deallocated or goes out of scope.

In Rust, the borrow checker enforces this rule to ensure the safety and correctness of code. If the borrow checker determines that a reference could possibly outlive its referent, it will generate an error to prevent the code from being compiled.

Here is an example of a situation where a reference could potentially outlive its referent:

```rust
fn main() {
    let s = String::from("hello");
    let r = &s; // Borrow s with a reference
    let t = r; // t is also a reference to s
    drop(s); // s goes out of scope and is deallocated
    println!("{}", t); // Error: t is a dangling reference
}
```

In this example, we create a `String` called `s` and borrow it with a reference `r`. We then create another reference `t` to `r`. However, when we call `drop` on `s`, it goes out of scope and is deallocated. This means that `t` becomes a dangling reference, because it is still pointing to data that no longer exists. If we tried to use `t` after `s` has been deallocated, it would cause undefined behavior.

Because references are just addresses under the hood, it is important to have these strict rules in place to ensure that they are used correctly and safely.

The borrow checker is a system in Rust that enforces these rules and ensures that references are used correctly. It checks that references are not used after their referents have been deallocated or go out of scope, and it prevents the creation of references to data that does not have a clear owner.

## References to Values

```rust
use std::collections::HashMap;
type Table = HashMap<String, Vec<String>>;

fn show(table: Table) {
    for (artist, works) in table {
        println!("works by {}:", artist);
        for work in works {
            println!(" {}", work);
        }
    }
}

fn main() {
    let mut table = Table::new();
    table.insert(
        "Gesualdo".to_string(),
        vec![
            "many madrigals".to_string(),
            "Tenebrae Responsoria".to_string(),
        ],
    );
    table.insert(
        "Caravaggio".to_string(),
        vec![
            "The Musicians".to_string(),
            "The Calling of St. Matthew".to_string(),
        ],
    );
    table.insert(
        "Cellini".to_string(),
        vec![
            "Perseus with the head of Medusa".to_string(),
            "a salt cellar".to_string(),
        ],
    );
    show(table);
}
```

In this code, `show` is defined to take a `Table` as an argument. When `show` is called with `table`, the `table` is moved into the function and the ownership of the `table` is transferred to `show`. This means that `main` can no longer access or modify the `table` after the call to `show`.

The `for` loop in the `show` function takes ownership of the `table` and consumes it entirely. This means that the `table` is no longer accessible after the `for` loop finishes executing. Thanks Rust!

If you want to retain ownership of the `table` in the `main` function and still be able to pass it to `show`, you can use a reference to the `table` instead. You can modify the code like this:

```rust
use std::collections::HashMap;
type Table = HashMap<String, Vec<String>>;

fn show(table: &Table) {
    for (artist, works) in table {
        println!("works by {}:", artist);
        for work in works {
            println!(" {}", work);
        }
    }
}

fn main() {
    let mut table = Table::new();
    table.insert(
        "Gesualdo".to_string(),
        vec![
            "many madrigals".to_string(),
            "Tenebrae Responsoria".to_string(),
        ],
    );
    table.insert(
        "Caravaggio".to_string(),
        vec![
            "The Musicians".to_string(),
            "The Calling of St. Matthew".to_string(),
        ],
    );
    table.insert(
        "Cellini".to_string(),
        vec![
            "Perseus with the head of Medusa".to_string(),
            "a salt cellar".to_string(),
        ],
    );
    show(&table);
}
```

In this version of the code, `show` is defined to take a reference to a `Table` rather than a `Table` itself. This allows the `main` function to retain ownership of the `table` and continue to modify it after the call to `show`. When calling `show`, the `&` operator is used to pass a reference to the `table` rather than the `table` itself.

In Rust, references allow you to refer to a value without taking ownership of it. This can be useful in situations where you want to pass a value to a function but still retain ownership of it.

There are two types of references in Rust: shared references and mutable references.

Shared references allow you to read the value of the referent, but not modify it. They are written with the & operator and have the type &T, where T is the type of the referent. Shared references are Copy, which means that they can be copied freely without affecting the referent.

Mutable references, on the other hand, allow you to both read and modify the value of the referent. They are written with the &mut operator and have the type &mut T, where T is the type of the referent. Mutable references are not Copy because they allow modification of the referent. You may only have one mutable reference to a particular value at a time, to prevent conflicts between multiple modifications.

Using references can be a good way to avoid having to move values around and maintain ownership, especially when working with large or complex data structures.

Here is a comparison of shared references and mutable references in Rust:

|                              | Shared reference | Mutable reference |
| ---------------------------- | ---------------- | ----------------- |
| Syntax                       | `&T`             | `&mut T`          |
| Can read value of referent   | Yes              | Yes               |
| Can modify value of referent | No               | Yes               |
| Can have multiple references | Yes              | No                |
| Is `Copy`                    | Yes              | No                |

The distinction between shared and mutable references is a way to enforce a multiple readers or single writer rule at compile time.

A multiple readers or single writer rule is a way to control concurrent access to a shared resource. It specifies that either multiple readers can access the resource concurrently, or a single writer can access the resource exclusively.

By enforcing the multiple readers or single writer rule at compile time, Rust helps to ensure that concurrent access to shared resources is safe and correct.

When you pass a shared reference to a value as an argument to a function, the function receives a non-owning pointer to the value. This means that the function can access the value, but it does not take ownership of the value and the original owner retains ownership.

In the case of the `show` function, the parameter `table` has the type `&Table`, which means it is a shared reference to a `Table`. When `show` is called, it receives a shared reference to the `table` and can access the keys and values of the `table`, but it does not take ownership of the `table` and the original owner (`main`) retains ownership.

Iterating over a shared reference to a `HashMap` is defined to produce shared references to each entry's key and value. This means that the type of `artist` in the `for` loop is `&String` and the type of `works` is `&Vec<String>`. Similarly, iterating over a shared reference to a vector is defined to produce shared references to its elements, so the type of `work` in the inner `for` loop is `&String`.

Since `show` is not taking ownership of the `table` and is only passing around shared references, no ownership changes hands anywhere in this function. It's just passing around non-owning pointers.

That's correct! When you want to modify a value that is being borrowed, you need to use a mutable reference. A mutable reference, written with the `&mut` operator and having the type `&mut T`, allows you to both read and modify the value of the referent. However, you may not have any other references (shared or mutable) to that value active at the same time. This is to prevent conflicts between multiple modifications.

In the case of the `sort_works` function, the parameter `table` has the type `&mut Table`, which means it is a mutable reference to a `Table`. When `sort_works` is called, it receives a mutable reference to the `table` and can read and modify the values of the `table`.

To pass a mutable reference to a value as an argument to a function, you need to use the `&mut` operator. In the example you provided, the `sort_works` function is called with `&mut table`, which passes a mutable reference to the `table` to the function. This allows the function to modify the values of the `table`.

Passing a value to a function in a way that moves ownership of the value to the function is called passing the value by value. Passing a reference to the value, on the other hand, is called passing the value by reference. In Rust, it is important to distinguish between passing a value by value and passing it by reference because it affects how ownership is affected.

When you pass a value by value, the value is moved into the function and the original owner no longer has access to it. This means that the function takes ownership of the value and is responsible for releasing any resources that the value holds when it is finished with the value.

On the other hand, when you pass a value by reference, the function receives a non-owning pointer to the value. This means that the function can access the value, but it does not take ownership of the value and the original owner retains ownership.

In Rust, it is important to carefully consider whether to pass a value by value or by reference, depending on whether you want the function to take ownership of the value or just access it.

## Rust References Versus C++ References

Here is a comparison of Rust references and C++ references:

|                          | Rust references | C++ references |
| ------------------------ | --------------- | -------------- |
| Syntax                   | `&T`            | `&T`           |
| Creating references      | `&T`            | Implicit       |
| Dereferencing references | `*T`            | Implicit       |
| Lifetime                 | Yes             | No             |
| Borrowing rules          | Yes             | No             |
| Nullability              | No              | Yes            |
| Implicit dereference     | `.` operator    | Yes            |

In Rust, references are created explicitly with the `&` operator and dereferenced explicitly with the `*` operator.

In C++, references are created and dereferenced implicitly and can appear anywhere they are needed.

```c++
int x = 10;
int &r = x; // initialization creates reference implicitly
```

Rust references have a lifetime, which is a compile-time concept that specifies the scope in which a reference is valid. This means that a Rust reference can only be used within the lifetime of the value it refers to. C++ references, on the other hand, do not have a lifetime and can be used for the lifetime of the program.

```rust
let x = 10;
let r = &x; // &x is a shared reference to x
let z = *r
```

Rust has a borrowing system that enforces rules around when a value can be borrowed and how it can be borrowed. For example, you can only have one mutable reference to a value at a time, and you cannot have any other references (shared or mutable) to the value while a mutable reference is active. C++ does not have these borrowing rules and allows you to have multiple references to a value at the same time.

Rust references cannot be null, which means that they always refer to a valid value. C++ references can be null, which means that they may not always refer to a valid value.

In Rust, the `.` operator can also implicitly borrow a reference to its left operand if needed for a method call. For example, you can call the `sort` method on a vector like this:

```rust
let mut v = vec![1973, 1968];
v.sort(); // implicitly borrows a mutable reference to v
```

This is equivalent to calling `sort` with a more explicit, verbose syntax:

```rust
let mut v = vec![1973, 1968];
(&mut v).sort(); // equivalent, but more verbose
```

Overall, Rust references and C++ references are similar in that they allow you to access a value without taking ownership of it, but they have some key differences in terms of lifetime, borrowing rules, nullability, and syntax.

## Assigning References

In Rust, assigning a reference to a variable makes the variable point to a new location in memory. This is different from C++ references, which do not allow you to change where they point after they have been initialized.

For example, in Rust you can do the following:

```rust
let x = 10;
let y = 20;
let mut r = &x;
if b { r = &y; }
assert!(*r == 10 || *r == 20);
```

Here, the reference `r` initially points to `x`. If `b` is true, the code points it at `y` instead. This means that `r` can now point to either `x` or `y`, depending on the value of `b`.

In C++, assigning a value to a reference stores the value in its referent. Once a C++ reference has been initialized, there is no way to make it point to anything else. For example:

```rust
int x = 10;
int y = 20;
int &r = x;
if (b) { r = y; } // stores y in x, r still points to x
```

In this C++ code, the reference `r` is initialized to point to `x`. If `b` is true, the value of `y` is stored in `x`, but `r` still points to `x`. There is no way to make `r` point to `y`.

## References to References

It is possible to create references to references in Rust, which are known as "double references" or "reference of reference" types. These types can be useful in certain scenarios, such as when working with raw pointers or when writing generic code that needs to accept a variety of different reference types.

Here is an example of creating a double reference in Rust:

```rust
let x = 10;
let r1 = &x; // r1 is a shared reference to x
let r2 = &r1; // r2 is a shared reference to r1, which is a reference to x
```

In this example, `r1` is a shared reference to `x`, and `r2` is a shared reference to `r1`. This means that `r2` is effectively a reference to `x`, but it is represented as a reference to a reference.

You can dereference a double reference with the `**` operator:

```rust
let x = 10;
let r1 = &x;
let r2 = &r1;
let y = **r2; // y is now 10
```

In this example, `y` is assigned the value of `x` by dereferencing `r2` twice: once to get the value of `r1`, and a second time to get the value of `x`.

It is also possible to create mutable double references in Rust by using the `&mut` operator:

```rust
let mut x = 10;
let r1 = &mut x; // r1 is a mutable reference to x
let r2 = &r1; // r2 is a shared reference to r1, which is a mutable reference to x
```

In this example, `r1` is a mutable reference to `x`, and `r2` is a shared reference to `r1`. This means that `r2` is effectively a reference to a mutable reference to `x`.

You can dereference a mutable double reference with the `*` operator:

```rust
let mut x = 10;
let r1 = &mut x;
let r2 = &r1;
*r2 = 20; // x is now 20
```

In this example, the value of `x` is changed.

It is important to note that creating double references can be dangerous, because it can be easy to lose track of how many levels of indirection are involved. This can lead to bugs, especially when working with raw pointers, where the type system does not provide any safety guarantees.

For example, consider the following code:

```rust
let x = 10;
let r1 = &x;
let r2 = &r1;
let r3 = &r2;
**r3 = 20; // x is now 20
```

In this code, `x` is changed to `20` by dereferencing `r3` three times: once to get the value of `r2`, once to get the value of `r1`, and a third time to get the value of `x`. This can be confusing, especially if you are working with long chains of references.

It is generally recommended to avoid creating double references unless there is a specific need for them. If you do need to use double references, it is important to pay close attention to the number of levels of indirection involved, and to be careful when dereferencing them.

## Comparing References

In Rust, you can compare references with the `==` and `!=` operators, just like you would with other types. For example:

```rust
let x = 10;
let y = 20;
let r1 = &x;
let r2 = &y;

assert!(r1 != r2); // r1 and r2 point to different values
```

When you compare references, Rust compares the values that the references point to, rather than the references themselves. This means that two references are equal if and only if they point to the same value.

It is important to note that you can only compare references of the same type. For example, you cannot compare a shared reference to an integer with a mutable reference to a string:

```rust
let x = 10;
let y = "hello";
let r1 = &x;
let r2 = &mut y;

assert!(r1 != r2); // cannot compare references of different types
```

In this example, the comparison between `r1` and `r2` is not allowed, because `r1` is a shared reference to an integer, while `r2` is a mutable reference to a string.

It is also important to note that you cannot compare references to different lifetimes. For example:

```rust
fn compare_references(x: &i32, y: &i32) -> bool {
    x == y
}

let x = 10;
let y = 20;

assert!(compare_references(&x, &y)); // okay
assert!(compare_references(&x, &x)); // okay
```

In this example, the comparisons between `x` and `y`, and between `x` and `x`, are allowed, because both references have the same lifetime. However, the following code is not allowed:

```rust
fn compare_references(x: &i32, y: &i32) -> bool {
    x == y
}

let x = 10;

assert!(compare_references(&x, &10)); // not allowed
```

In this example, the comparison between `x` and `10` is not allowed, because the reference to `10` has a different lifetime than the reference to `x`. This is a safety feature of Rust that ensures that you cannot compare references to values that have already gone out of scope.

If you want to compare the memory addresses of two references, you can use the `std::ptr::eq` function. This function takes two references as arguments and returns `true` if they point to the same memory address, and `false` otherwise.

Here is an example:

```rust
use std::ptr;

fn compare_memory_addresses(x: &i32, y: &i32) -> bool {
    ptr::eq(x, y)
}

let x = 10;
let y = 20;

assert!(!compare_memory_addresses(&x, &y)); // x and y are stored at different memory addresses
let r = &x;
assert!(compare_memory_addresses(r, &x)); // r and &x point to the same memory address
```

In this example, the comparison between `&x` and `&y` returns `false`, because `x` and `y` are stored at different memory addresses. The comparison between `r` and `&x` returns `true`, because `r` and `&x` point to the same memory address.

It is important to note that the `std::ptr::eq` function is not intended for general use, and should only be used in cases where you need to compare the memory addresses of references. In most cases, you should use the `==` operator to compare the values that the references point to, rather than the references themselves.