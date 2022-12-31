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
