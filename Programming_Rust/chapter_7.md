# Error Handling

Error handling is an essential aspect of Rust programming that ensures code reliability and robustness. Rust has a unique approach to error handling compared to other languages, called "Result types." In this approach, the code returns either an "Ok" value, indicating that the function has completed successfully, or an "Err" value, indicating that an error has occurred.

Let's dive deeper into the various aspects of error handling in Rust programming.

## Panic

The use of `panic!` can make code more difficult to reason about and debug, since it can be difficult to determine where the error occurred and why the program halted. Therefore, it is generally best to reserve the use of `panic!` for situations where it is truly necessary, and to handle recoverable errors using other mechanisms. `panic!` is for the kind of error that should never happen.

In Rust, a program panics when it encounters an error that is so severe that there must be a bug in the program itself. These errors include things like:

- Out-of-bounds array access:

  ```rs
  let arr = [1, 2, 3];
  let index = 5;
  let value = arr[index];
  // This will panic at runtime with an "index out of bounds" error.
  ```

- Integer division by zero:

  ```rs
  let numerator = 10;
  let denominator = 0;
  let result = numerator / denominator;
  // This will panic at runtime with a "attempt to divide by zero" error.
  ```

- Calling `.expect()` on a Result that happens to be Err:

  ```rs
  use std::fs::File;
  let file = File::open("nonexistent_file.txt").expect("Failed to open file");
  // This will panic at runtime with a custom error message.
  ```

- Assertion failure:

  ```rs
  let x = 5;
  let y = 10;
  assert!(x > y, "x is not greater than y");
  // This will panic at runtime with a custom error message.
  ```

- Triggering a panic directly with `panic!()`:

  ```rs
  let age = -10;
  if age < 0 {
      panic!("Age cannot be negative!");
  }
  // This will panic at runtime with a custom error message.
  ```

When these errors occur, Rust provides two options: _unwinding the stack_ or _aborting the process_. By default, Rust will unwind the stack. This means that Rust will attempt to unwind the stack and clean up any resources that were allocated before the panic occurred. This process involves calling destructors for any objects on the stack and freeing any resources that were allocated on the heap.

However, in some cases, it may be more appropriate to abort the process when a panic occurs. Aborting the process is a more drastic measure, but it can be useful in situations where the program has become so unstable that it is unsafe to continue executing. To abort the process, you can set the panic runtime configuration option to abort using the `panic = "abort"` configuration in your `Cargo.toml` file.

### Unwinding

In Rust, when a panic occurs, the default behavior is for the program to unwind the stack. Unwinding the stack means that Rust will walk back up the stack and call the destructors of any objects that were created along the way, in the reverse order in which they were created. This process ensures that any resources allocated on the heap are properly cleaned up, and any temporary objects are destroyed.

```rs
fn main() {
    let v = vec![1, 2, 3];
    panic!("oh no!");
}
```

When the `panic!` macro is executed, the program will begin unwinding the stack. This means that Rust will call the destructors of any objects that were created along the way, in the reverse order in which they were created. In this case, Rust will destroy the vector `v` before the program exits. This ensures that any memory allocated on the heap for the vector is properly cleaned up before the program terminates.

There is also a way to catch stack unwinding, allowing the thread to survive and continue running. The standard library function `std::panic::catch_unwind()` does this.

This function takes a closure that contains the code you want to run, and returns a `Result` indicating whether the closure panicked or not.

```rs
use std::panic;

fn main() {
    let result = panic::catch_unwind(|| {
        println!("this code is running inside a catch_unwind closure");
        panic!("oh no, something went wrong!");
    });

    match result {
        Ok(()) => println!("the closure ran successfully"),
        Err(_) => println!("the closure panicked"),
    }

    println!("the program continues to run after the catch_unwind closure");
}
```

### Aborting

Unwinding the stack can be expensive in terms of performance, especially if the program has to unwind a large number of objects. For this reason, Rust provides an alternative method for handling panics: aborting the program. Aborting the program simply terminates the process immediately without unwinding the stack. While this can be a faster method for handling panics, it also means that any resources that were allocated during the program's execution will not be properly cleaned up.

You can choose to abort the program instead of unwinding the stack by setting the panic runtime configuration option to abort. This can be done in your Cargo.toml file, like this:

```toml
[profile.release]
panic = "abort"
```

There are three circumstances in which Rust does not try to unwind the stack when a panic occurs:

- When the panic is caused by a call to the `std::process::abort` function. The `std::process::abort` function immediately terminates the process without any attempt to unwind the stack.

- When the program is built with the "panic=abort" configuration option. This configuration option causes the program to immediately terminate the process without any attempt to unwind the stack when a panic occurs.

- If a `drop()` method itself panics, it can trigger a second panic while Rust is still trying to unwind the stack and clean up resources. This is known as a "double panic" and is considered fatal. When a double panic occurs, Rust will immediately stop unwinding and abort the whole process, which can lead to some resources not being properly cleaned up.

To avoid double panics, it's important to ensure that `drop()` methods are implemented in a way that does not panic. One common approach is to use the `std::mem::take()` function to replace the value being dropped with a default value before attempting to drop it. This can prevent panics caused by trying to drop a value that has already been dropped or is in an invalid state.
