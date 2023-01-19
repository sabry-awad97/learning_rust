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

In this example, the comparisons between `x` and `y`, and between `x` and `x`, are allowed, because both references have the same lifetime (the scope in which the reference is valid). However, the following code is not allowed:

```rust
fn compare_references(x: &i32, y: &i32) -> bool {
    x == y
}

let x = 10;

assert!(compare_references(&x, &10)); // not allowed
```

In this example, the comparison between `&x` and `&10` is not allowed, because the reference to `10` has a different lifetime than the reference to `x`. Specifically, the lifetime of the reference to `10` is the entire function `compare_references`, while the lifetime of the reference to `x` is the block in which x is defined.

In general, you can only compare references if they have the same lifetime. This is a safety feature of Rust that ensures that you cannot compare references to values that have already gone out of scope.

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

## References Are Never Null

A null pointer is a special value that is used to indicate that a pointer does not currently refer to a valid memory address. In some programming languages, such as C and C++, a null pointer is represented by a special value called `NULL` or `nullptr`, which is typically defined as a constant with a value of zero.

Null pointers are often used as a sentinel value to indicate the absence of a value, or as a placeholder for a pointer that has not yet been initialized. However, null pointers can also be the source of serious bugs and security vulnerabilities, as they can be dereferenced (accessed through the pointer) by mistake, leading to undefined behavior and potentially causing a crash or allowing attackers to exploit the program.

Instead of using null pointers to indicate the absence of a value, Rust has the `Option<T>` type, which can either be `Some(T)` if a value is present, or `None` if the value is absent and and it uses explicit lifetime annotations to specify the scope in which references are valid. These features help prevent null reference errors and enable Rust to provide strong guarantees about memory safety and data ownership.

For example, consider the following code:

```rust
fn find_first_even(numbers: &[i32]) -> Option<&i32> {
    for number in numbers {
        if number % 2 == 0 {
            return Some(number);
        }
    }
    None
}

let numbers = [1, 3, 5, 7, 9];
let result = find_first_even(&numbers);

match result {
    Some(n) => println!("The first even number is {}", n),
    None => println!("There are no even numbers in the list"),
}
```

In this example, the function `find_first_even` takes a slice of integers and returns an `Option<&i32>`, which is either `Some(&i32)` if an even number is found, or `None` if no even numbers are found.

Using the `Option<T>` type instead of null pointers allows Rust to ensure that you always check for the presence of a value before using it, which helps prevent null reference errors.

It is also worth noting that Rust has a number of other features that help prevent null reference errors, such as the `NonNull<T>` type, which is a reference-like type that is guaranteed to be non-null, and the `MaybeUninit<T>` type, which allows you to temporarily store uninitialized values. These types can be used in conjunction with the `unsafe` keyword to provide more fine-grained control over null references in Rust. However, it is generally recommended to use Rust's built-in borrowing and ownership system and explicit lifetime annotations to avoid null reference errors, as these features are safer and easier to use in most cases.

## Borrowing References to Arbitrary Expressions

In Rust, you can borrow a reference to any expression, not just variables. The reference will be valid for the lifetime of the expression. For example, you can borrow a reference to a constant value, like this:

```rust
let x = 10;
let y = 20;

let r1 = &(x + y); // r1: &i32
assert_eq!(*r1, 30);
```

Here, `r1` is a shared reference to the result of the `x + y` expression, which is a constant value of `30`.

You can also borrow a reference to a more complex expression, like this:

```rust
let v = vec![1, 2, 3];

let r2 = &v[1..]; // r2: &[i32]
assert_eq!(*r2, [2, 3]);
```

Here, `r2` is a shared reference to a slice of the `v` vector, which includes the elements at indices 1 and 2.

```rust

fn factorial(n: usize) -> usize {
    (1..n+1).product()
}

let r = &factorial(6);
// Arithmetic operators can see through one level of references.
assert_eq!(r + &1009, 1729);
```

This code defines a function called `factorial` that takes in a `usize` parameter called `n`, and returns a `usize` value. It calculates the factorial of `n` by using the `product` method of an iterator over the range `1..n+1`.

Then, it creates a reference `r` to the value returned by calling the `factorial` function with the argument `6`.

Finally, it uses the `assert_eq!` macro to assert that the value of `r` plus the value of `1009` is equal to `1729`. The `+` operator is able to see through one level of references, so the addition is performed on the values pointed to by `r` and `&1009`. If this assertion is true, the code will continue to execute. If it is false, the program will panic.

Borrowing a reference to an expression can be useful when you want to pass the result of the expression to a function or method that expects a reference, without creating a new variable to store the result. However, it is important to keep in mind that the reference will only be valid for the lifetime of the expression, and you cannot use the reference after the expression goes out of scope.

## References to Slices and Trait Objects

Slices are a view into a contiguous sequence of elements in memory, and they are represented by the type `&[T]`. For example, a slice of integers is written as `&[i32]` and a slice of strings is written as `&[String]`. Slices allow you to borrow a portion of an array or vector, rather than the entire thing.

```rust
let arr = [1, 2, 3, 4, 5];
let slice = &arr[1..3]; // creates a slice of the array from index 1 to 3 (excluding)

fn print_slice(slice: &[i32]) {
    for item in slice {
        println!("{}", item);
    }
}

print_slice(slice); // prints "2 3"
```

Trait objects are references to values that implement a particular trait, and they are represented by the type `&Trait`. For example, a trait object that allows you to call the draw method on any value that implements the `Draw` trait would be written as `&Draw`. Trait objects are useful when you need to **store a value of unknown type** in a struct or when you want to **call a method on a value without knowing its exact type** (dynamic dispatch).

```rust
trait Shape {
    fn area(&self) -> f64;
}

struct Circle {
    radius: f64,
}

impl Shape for Circle {
    fn area(&self) -> f64 {
        3.14 * self.radius * self.radius
    }
}

struct Rectangle {
    width: f64,
    height: f64,
}

impl Shape for Rectangle {
    fn area(&self) -> f64 {
        self.width * self.height
    }
}

fn print_area(shape: &dyn Shape) {
    println!("Area: {}", shape.area());
}

let circle = Circle { radius: 2.0 };
let rectangle = Rectangle { width: 3.0, height: 4.0 };

print_area(&circle); // prints "Area: 12.56"
print_area(&rectangle); // prints "Area: 12.0"
```

Both slices and trait objects are implemented using **fat pointers**, which are references that include both a pointer to the **data** and a pointer to **metadata**. The metadata includes information about the length of the slice or the type of the value being pointed to. This additional information allows slices and trait objects to be used safely and efficiently.

## Fat Pointers

Fat pointers are a type of data structure in Rust that contain two pieces of information: a pointer to some data, and the length of the data. In Rust, slices are a kind of fat pointer, represented by the type `&[T]`, where `T` is the type of the elements in the slice. Fat pointers are often used when working with slices, as they allow the slice to be passed around by reference, while also maintaining information about the length of the slice.

Fat pointers are also used for trait objects, which are a way to represent a value of any type that implements a particular trait. Trait objects are implemented using fat pointers, with the first element of the fat pointer being a pointer to the value, and the second element being a pointer to a virtual table of functions that can be called on the value. This allows trait objects to be passed around by reference and for the correct function to be called on the value, even if the type of the value is not known at compile time.
They allow for more information to be stored about a reference without requiring the use of unsafe code.

Here is an example of using a slice fat pointer:

```rust
fn print_slice(slice: &[i32]) {
    for element in slice {
        println!("{}", element);
    }
}

fn main() {
    let array = [1, 2, 3, 4, 5];
    let slice = &array[1..4];
    print_slice(slice);  // Output: 2 3 4
}
```

And here is an example of using a trait object fat pointer:

```rust
trait Printable {
    fn print(&self);
}

impl Printable for i32 {
    fn print(&self) {
        println!("{}", self);
    }
}

fn print_printable(p: &dyn Printable) {
    p.print();
}

fn main() {
    let x = 5;
    print_printable(&x);  // Output: 5
}
```

## Reference Safety

Reference safety in Rust is ensured through a combination of compile-time checks and runtime checks. At compile time, Rust checks that references are always valid and will not allow you to create a reference to an invalid location in memory. This ensures that the program is safe to run, as there is no risk of a null or dangling reference causing a segmentation fault or other runtime error.

At runtime, Rust also checks that references are valid. For example, if you try to dereference a shared reference to a value that has already been dropped, the program will panic. This helps to prevent data races and other concurrency issues that can arise when working with shared references.

Overall, the combination of compile-time and runtime checks helps to ensure that Rust programs are safe to run and free from the kinds of bugs that can be caused by invalid or improperly used references.

## Borrowing a Local Variable

```rust
fn main() {
    let x = 10;
    {
        let r = &x;
        assert_eq!(*r, 10);
    }
    assert_eq!(x, 10);
}
```

This code works just fine: the variable `x` is in scope for the entire body of the outer block, and `r` is a reference to `x`, so everything is in order.

However, if we try to modify the reference in a local block, Rust will complain:

```rust
fn main() {
    let mut x = 10;
    {
        let r = &x;
        *r = 10
    }
    assert_eq!(x, 10);
}
```

This error message is a bit cryptic, but it’s trying to tell you that the borrow of `x` is still in effect after the inner block has completed. In other words, you’re trying to modify a borrowed value.

We're not allowed to modify `x` through `r` because `r` has been borrowed (not owned).

- We can only modify `x` in the **same block** where it’s declared.

This rule is in place to prevent you from inadvertently modifying a borrowed value through a shared reference.

If the reference were to last longer than the block, the value might have changed out from under you, which could be confusing.

If you really want to modify a value through a borrowed reference, you have to use a mutable reference:

```rust
let mut x = 10;
{
    let mut r = &mut x;
    *r += 20;
}
assert_eq!(x, 30);
```

Now the code works as expected, because you have exclusive access to `x` through `r`.
Thus, the borrowing rules ensure that Rust prevents data races and other threading errors at compile time.

When you create a reference to a local variable, Rust checks that the reference will be valid for the entire duration that the reference will be used.
This is called the reference's lifetime.

- If the lifetime of the reference is shorter than the lifetime of the variable, Rust reports an error.

For example, consider the following code:

```rust
fn main() {
    let x = 10;
    let r = &x; // r has lifetime 'a
    drop(x); // x has lifetime 'b
    //
}
```

In this code, the variable `x` has a lifetime of `'b` and the reference `r` has a lifetime of `'a`. Since the lifetime of `r` is longer than the lifetime of `x`, Rust will report an error because it is not safe to use `r` after `x` has been dropped.

To fix this error, we need to ensure that the lifetime of `r` is shorter than the lifetime of `x`. We can do this by creating the reference before we declare `x`:

```rust
fn main() {
    let r; // r has lifetime 'a
    {
        let x = 10;
        r = &x; // r's lifetime is now tied to x's lifetime
    } // x goes out of scope, so r is no longer valid
}
```

In this code, the lifetime of `r` is now tied to the lifetime of `x`, so it is safe to use `r` within the curly braces where `x` is defined. Once `x` goes out of scope at the end of the curly braces, `r` is no longer valid.

- You can't borrow a reference to a local variable and take it out of the variable’s scope.

```rust
{
    let r;
    {
        let x = 1;
        r = &x;
    }
    assert_eq!(*r, 1); // bad: reads memory `x` used to occupy
}
```

This code tries to create a reference to the local variable `x` and then store it in the variable `r`.

But when `x` goes out of scope, Rust invalidates all references to it, so the final line of the code tries to access memory that has already been deallocated.

This is what the `assert_eq!` statement does, and Rust will not let you do it: it won't even compile. If you remove the `assert_eq!` statement and try to run the code, Rust will complain that `r` is not initialized.

This protection is a key feature of Rust. It prevents you from creating a reference to something that’s not supposed to be accessed anymore.

## Lifetimes

Lifetimes are a way to specify the lifetime of a reference in Rust. A lifetime is a period of time during which a reference is valid. Every reference has a lifetime, which is the scope within which the reference is guaranteed to be valid.

For example, consider the following code:

```rust
fn main() {
    let x = 10;
    let r = &x;

    println!("The value of x is {}", x);
    println!("The value of r is {}", r);
}
```

In this code, the reference `r` has the same lifetime as the variable `x`. This means that the reference `r` is valid for as long as the variable `x` is in scope. In this case, `r` is a valid reference until the end of the `main` function, at which point the variable `x` goes out of scope and the reference `r` becomes invalid.

Lifetimes are particularly important when working with references to references, as the lifetime of the inner reference must be a subset of the lifetime of the outer reference.

For example, consider the following code:

```rust
fn main() {
    let x = 10;
    let r1 = &x;
    let r2 = &r1;

    println!("The value of r1 is {}", r1);
    println!("The value of r2 is {}", r2);
}
```

In this code, the reference `r1` has the same lifetime as the variable `x`, and the reference `r2` has the same lifetime as the reference `r1`. This means that the reference `r2` is valid for as long as the reference `r1` is valid, which in turn is valid for as long as the variable `x` is in scope.

It is important to note that the lifetime of a reference cannot be longer than the lifetime of the value it refers to. This ensures that references are always valid and cannot refer to values that have already gone out of scope.

Lifetimes are often inferred by the Rust compiler, but in some cases you may need to annotate your code with explicit lifetime annotations to provide additional information to the compiler.

For example, consider the following code:

```rust
fn main() {
    let x = 10;
    let r1 = &x;

    {
        let y = 20;
        let r2 = &y;

        println!("The value of r1 is {}", r1);
        println!("The value of r2 is {}", r2);
    }
}
```

In this code, the reference `r1` has the same lifetime as the variable `x`, and the reference `r2` has the same lifetime as the variable `y`. However, the lifetime of `y` is only within the inner block of code, and so the reference `r2` is only valid within that block.

If we try to use the reference `r2` outside of that block, the Rust compiler will give us an error because the value `y` has already gone out of scope and the reference `r2` is no longer valid.

To fix this error, we can use explicit lifetime annotations to specify that the lifetime of the reference `r2` should be the same as the lifetime of the reference `r1`, which has a longer lifetime:

```rust
fn main() {
    let x = 10;
    let r1 = &x;

    {
        let y = 20;
        let r2: &'a i32 = &y;

        println!("The value of r1 is {}", r1);
        println!("The value of r2 is {}", r2);
    }
}
```

In this code, we have annotated the reference `r2` with the lifetime `'a`. This specifies that the lifetime of `r2` is the same as the lifetime `'a`, which we have chosen to be the same as the lifetime of `r1`.

This allows us to use the reference `r2` within the inner block of code, as it now has the same lifetime as `r1`, which has a longer lifetime that encompasses the inner block of code.

Lifetime annotations are often used when working with references to references or when working with structs that contain references. They provide a way to specify the lifetime of a reference and ensure that references are always valid and cannot refer to values that have already gone out of scope.

## Receiving References as Function Arguments

Here are some of the key points:

1. In Rust, when you pass a reference to a function as an argument, the function signature must specify the lifetime of the reference.

```rust
fn foo(x: &i32) {
    // x is a reference to an i32 with an unknown lifetime
}

fn bar<'a>(x: &'a i32) {
    // x is a reference to an i32 with the lifetime 'a
}
```

1. This is to ensure that the reference is used safely and does not result in a dangling pointer.

```rust
fn foo(x: &i32) {
    println!("{}", x);
}

fn main() {
    let y = 5;
    foo(&y); // okay, y is valid for the entire duration of the program
}
```

1. If the function stores the reference in a global variable (static), the lifetime of the reference must be at least as long as the lifetime of the global variable.

```rust
static mut STASH: &i32 = &10;

fn foo(x: &i32) {
    unsafe {
        STASH = x;
    }
}

fn main() {
    let y = 5;
    foo(&y); // error: y has a shorter lifetime than STASH
}
```

1. In Rust, the lifetime of a global variable is 'static, which means it lasts for the entire duration of the program.

```rust
static STASH: i32 = 10;

fn main() {
    let x = &STASH;
    println!("{}", x); // okay, x has the lifetime 'static
}
```

1. Therefore, the signature of the function must be changed to `fn f(p: &'static i32)` to indicate that it only accepts references with the 'static lifetime.

```rust
static mut STASH: &i32 = &10;

fn foo(x: &'static i32) {
    unsafe {
        STASH = x;
    }
}

fn main() {
    let y = 5;
    foo(&y); // error: y has a shorter lifetime than STASH
    let z = 10;
    foo(&z); // okay, z is a static
}
```

1. After the signature of the function is changed, you can only apply the function to references to other statics, because the lifetime of a static is 'static.

```rust
static mut STASH: &i32 = &10;

fn foo(x: &'static i32) {
    unsafe {
        STASH = x;
    }
}

static WORTH_POINTING_AT: i32 = 1000;

fn main() {
    let y = 5;
    foo(&y); // error: y has a shorter lifetime than STASH
    foo(&WORTH_POINTING_AT); // okay, WORTH_POINTING_AT is a static
}
```

## Passing References to Functions

When you pass a reference to a function in Rust, the function is able to use the reference to access the value that the reference points to. This can be useful when you want to allow a function to modify a value without taking ownership of it or when you want to avoid copying large data structures.

Here is an example of how you might pass a reference to a function in Rust:

```rust
fn add_one(x: &mut i32) {
    *x += 1;
}

fn main() {
    let mut x = 10;
    add_one(&mut x);
    println!("{}", x); // prints "11"
}
```

In this example, the function `add_one` takes a mutable reference to an `i32` as an argument. The reference is marked with the `&mut` syntax, which indicates that the function is allowed to modify the value that the reference points to. Inside the body of `add_one`, the `*` operator is used to dereference the reference and access the value it points to. The value is then incremented by 1.

When `add_one` is called with the argument `&mut x`, the function is able to modify the value of `x` through the reference. After the function returns, the value of `x` is 11.

```rust
fn g<'a>(p: &'a i32) { ... }
let x = 10;
g(&x);
```

In this code, the function `g` has a signature `fn g<'a>(p: &'a i32)`, which means it takes a reference to an `i32` with any given lifetime 'a. The function `g` is called with the argument `&x`, which is a reference to the variable `x`.

When the reference `&x` is passed to `g`, the lifetime of `&x` is inferred to be the same as the lifetime of `x`. Since `x` has a local variable lifetime (it is only valid within the scope in which it is declared), the lifetime of `&x` is also a local variable lifetime. Therefore, the type of `&x` is `&'a i32`, where 'a is the lifetime of `x`.

Since the type of `&x` matches the type expected by `g`, the call to `g` is valid. The function `g` can then use the reference `p` safely, without worrying about it becoming a dangling pointer.

When you call a function in Rust, the compiler will infer the lifetimes of any references passed as arguments based on the context in which the function is called.

It's worth noting that although `g` takes a lifetime parameter 'a, you don't need to specify it when calling `g`. Instead, the compiler will infer the lifetime for you based on the context in which `g` is called. You only need to worry about lifetime parameters when defining functions and types.

```rust
fn f(p: &'static i32) { ... }
let x = 10;
f(&x);
```

If you try to pass a reference with a local variable lifetime (such as `&x`) to a function that expects a reference with a 'static lifetime (such as `f`), the code will not compile. This is because a reference with a local variable lifetime may not be used after the variable it references goes out of scope, while a reference with the 'static lifetime is valid for the entire duration of the program.

The function `f` has a signature `fn f(p: &'static i32)`, which means it expects a reference to an `i32` with the 'static lifetime. The variable `x` has a local variable lifetime, so the reference `&x` has a local variable lifetime as well. The lifetime of `&x` is not 'static, so it cannot be passed as an argument to `f`.

If you want to pass a reference with a local variable lifetime to a function that expects a reference with a 'static lifetime, you must first convert the reference to a 'static lifetime. This can be done using the `std::mem::transmute` function from the standard library, which allows you to convert a reference from one type to another as long as the types have the same size. However, using `transmute` is generally not recommended, as it can lead to undefined behavior if the types are not compatible.

Here's an example of how you might use `transmute` to pass a reference with a local variable lifetime to a function that expects a reference with a 'static lifetime:

```rust
fn f(p: &'static i32) {
    println!("{}", p);
}

fn main() {
    let x = 10;
    let y: &'static i32 = unsafe { std::mem::transmute(&x) };
    f(y);
}
```

In this example, the reference `&x` is converted to the 'static lifetime using `transmute`. The `transmute` function allows you to convert a reference from one type to another as long as the types have the same size. In this case, the reference is converted from a reference with a local variable lifetime (`&'a i32`) to a reference with a 'static lifetime (`&'static i32`).

After the reference is converted, it is stored in the variable `y`, which has the type `&'static i32`. The reference `y` can then be passed as an argument to `f`, which expects a reference with the 'static lifetime.

It's worth noting that using `transmute` is generally not recommended, as it can lead to undefined behavior if the types are not compatible. In this case, it's safe to use `transmute` because both `&'a i32` and `&'static i32` are pointers to the same type (`i32`), and therefore have the same size. However, if you try to use `transmute` to convert between types that are not compatible, it could result in a runtime error or other unexpected behavior.

## Returning References

In Rust it is common to pass references to data structures as function arguments and return references to parts of that structure. This is done to avoid unnecessary copying of data and to allow for more efficient memory usage. The reference types `&T` and `&mut T` are used to indicate that a function is borrowing a reference to a value rather than taking ownership of it.

```rust
fn smallest(v: &[i32]) -> &i32 {
    let mut s = &v[0];
    for r in &v[1..] {
        if *r < *s { s = r; }
    }
    s
}
```

The function `smallest` takes a reference to a slice of integers `v` and returns a reference to the smallest element in the slice. The function starts by initializing a variable `s` to the first element of the slice, and then iterates over the rest of the slice using a for loop. For each element `r` in the slice, the function checks if `r` is less than the current smallest element `s`. If it is, the function updates `s` to be the new smallest element. At the end of the loop, the function returns the reference to the smallest element.

When a function takes a single reference as an argument and returns a single reference, Rust assumes that the two must have the same lifetime.

```rust
fn smallest<'a>(v: &'a [i32]) -> &'a i32 { ... }
```

The code is adding a lifetime parameter `'a` to the `smallest` function and to the argument `v`. This tells the rust compiler that the returned reference `s` is tied to the lifetime of the slice `v`. This means that the returned reference is only valid as long as the slice `v` is still in scope.

Adding lifetimes in this way allows the Rust compiler to ensure that the returned reference is not used after the original data it references is no longer valid. This can prevent bugs that can occur in other languages when a reference to freed memory is used.

```rust
{
    let parabola = [9, 4, 1, 0, 1, 4, 9];
    let s = smallest(&parabola);
    assert_eq!(*s, 0); // fine: parabola still alive
}
```

It's good practice to include lifetimes when returning references to ensure that the returned references are valid and safe to use.

## Structs Containing References

Structs in Rust can contain references to other values, including other structs. When a struct contains a reference, it is called a "borrowing" the value that the reference points to. The lifetime of the reference is usually tied to the lifetime of the struct.

Whenever a reference type appears inside another type’s definition, you must write out its lifetime.

```rust
struct Data {
    value: i32,
}

struct Container<'a> {
    data: &'a Data,
}
```

The struct `Container` has a lifetime parameter `'a`, which is used to indicate that the reference to the `Data` struct is tied to the lifetime of the `Container` struct. This means that the reference to `Data` is only valid as long as the `Container` struct is still in scope.

It's important to note that the `Container` struct does not own the data it references, it only borrows it. This means that the data will not be deallocated when the `Container` struct goes out of scope.

It's also possible to use `Rc` or `Arc` smart pointers to wrap the references, which allows multiple structs to reference the same data and control the data ownership.

## Distinct Lifetime Parameters

```rust
struct S<'a> {
    x: &'a i32,
    y: &'a i32
}

let x = 10;
let r;
{
    let y = 20;
    {
        let s = S { x: &x, y: &y };
        r = s.x;
    }
}
println!("{}", r);
```

The code would not compile.

When initializing the struct `S` with `&x` and `&y`, Rust must ensure that the lifetime of `'a` is valid for both references, but in this case, the lifetime of `'a` cannot be shorter than the lifetime of `y` and also longer than the lifetime of `r` which is defined in the outermost scope.

`y` is defined in an inner scope and goes out of scope before the outermost scope, and the lifetime of `r` is defined in the outermost scope, so the lifetime of `'a` must enclose the lifetime of `r`, but it cannot enclose the lifetime of `y` because `y` goes out of scope before the outermost scope.

Rust would prevent you from creating such struct with such lifetime parameters, since the constraints are impossible to satisfy. This is to prevent creating references that might point to invalid memory, and to ensure the safety of the program.

```rust
struct S<'a, 'b> {
    x: &'a i32,
    y: &'b i32
}
```

The struct `S` has two lifetime parameters `'a` and `'b` which are used to indicate that the references stored in `x` and `y` fields have different lifetimes.

## Lifetime elision

Lifetime elision is a feature in Rust that allows the compiler to infer the lifetime of references in some cases, without the need to explicitly declare the lifetime parameters.

The Rust compiler uses a set of rules, called the elision rules, to infer the lifetime of references in structs, functions, and other types. The elision rules apply when the references have the same lifetime and can be inferred from the context.

Here are the three elision rules:

1. The first rule, called the "input lifetime rule," states that if a function or method has exactly one parameter that is a reference, the lifetime of that reference is assigned to all references in the return value.

```rust
fn foo<'a>(x: &'a i32) -> &i32 {
    x
}

fn foo(x: &i32) -> &i32 {
    x
}
```

1. The second rule, called the "output lifetime rule," states that if a function or method has exactly one return value that is a reference, the lifetime of that reference is assigned to all references in the parameters.

```rust
fn bar<'a>(x: &i32, y: &i32) -> &'a i32 {
    x
}

fn bar(x: &i32, y: &i32) -> &i32 {
    x
}
```

1. The third rule, called the "lifetime elision in structs," states that if a struct has exactly one field that is a reference, the lifetime of that reference is assigned to all references in the struct.

```rust
struct S<'a> {
    x: &'a i32,
    y: &'a i32,
}

struct S {
    x: &i32,
    y: &i32,
}
```

When the elision rules are not sufficient, you have to explicitly declare the lifetime parameters to indicate the lifetime of the references.

In general, the elision rules are designed to make the code more readable by reducing the amount of explicit lifetime annotations required. But, if you are unsure about the lifetimes of the references, it's a good practice to explicitly declare the lifetime parameters, to make it clear to the reader and to the compiler what the intended lifetimes are.

```rust
struct StringTable {
    elements: Vec<String>,
}

impl StringTable {
    fn find_by_prefix(&self, prefix: &str) -> Option<&String> {
        for i in 0 .. self.elements.len() {
            if self.elements[i].starts_with(prefix) {
                return Some(&self.elements[i]);
            }
        }
        None
    }
}
```

The `StringTable` struct has a field called `elements` which is a vector of `String`s. The `find_by_prefix` method takes a reference to a string as an argument, and it iterates over the vector of `String`s in the `elements` field. It checks if each `String` in the vector starts with the prefix passed as a parameter. If it does, it returns a reference to that `String` wrapped in a `Some` variant. If the prefix is not found in any of the `String`s in the vector, it returns `None`.

It's worth noting that, the lifetime of the returned reference is the same as the lifetime of the `self` reference. This is because the returned reference is pointing to an element of the `elements` vector, which is owned by the `StringTable` struct.

This method does not use lifetime elision since the return value is a reference, and the lifetime of the reference is explicitly specified by the lifetime of the self parameter.

Also, as the method is not modifying the struct it is marked with `&` making it a immutable borrow. If the method needed to modify the struct it would be marked with `&mut` making it a mutable borrow.
