# A Tour of Rust

Rust is a programming language that is designed to be fast, safe, and concurrent.
It has a strong emphasis on ownership and borrowing, which helps prevent null or dangling pointer references and segmentation faults, and makes it easier to write concurrent code.

## Key Features

Here is a high-level overview of some of the key features of Rust:

| Feature                 | Description                                                                                                                                                                                                                                         |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Ownership and Borrowing | Rust has a borrow checker that helps ensure that references are valid and that data is not used after it goes out of scope. This helps prevent many common programming errors, such as null or dangling pointer references and segmentation faults. |
| Concurrency             | Rust has strong support for concurrent programming, with a lightweight threading model and the ability to send messages between threads using channels.                                                                                             |
| Type System             | Rust has a statically-typed, expressive type system with a number of advanced features, such as generic types and trait-based programming.                                                                                                          |
| Macro System            | Rust has a powerful macro system that allows you to define custom syntax and code generation.                                                                                                                                                       |
| FFI                     | Rust has a Foreign Function Interface (FFI) that allows you to call C code from Rust and vice versa. This makes it easy to use existing libraries and frameworks written in other languages.                                                        |

## Rustup and Cargo

`rustup` is a tool that manages Rust installations and enables you to switch between stable, beta, and nightly versions of the Rust compiler. It also enables you to install the Rust toolchain, including cargo, the Rust package manager.

`cargo` is a command-line utility that helps you manage Rust projects and their dependencies. It can build and run your projects, download libraries (called "crates" in Rust), and handle other tasks like testing and creating documentation.

To install rustup, you can follow the instructions on the Rust website: **<https://www.rust-lang.org/tools/install>**

Once rustup is installed, you can use it to install the latest stable version of the Rust compiler and the cargo tool by running the following command:

```rust
rustup install stable
```

You can also use `rustup` to switch between different versions of the Rust compiler and to install additional tools, such as Rustfmt (a tool for formatting Rust code) and Clippy (a linting tool for Rust). For example, to install Rustfmt, you can run the following command:

```rust
rustup component add rustfmt
```

To create a new Rust project with `cargo`, you can use the `cargo new` command. This command generates the basic files and directories needed for a new Rust project, including a `Cargo.toml` file that specifies the project's dependencies and build settings.

For example, to create a new Rust project called "my-project", you can run the following command:

```rust
cargo new my-project
```

The `Cargo.toml` file is the main configuration file for the project. It specifies the name, version, and dependencies of the project. The `src` directory contains the Rust source code for the project. The `main.rs` file is the entry point for the project, and it contains the main function that is run when the project is built and executed.

You can then navigate to the project directory and build the project with cargo by running the following command:

```rust
cargo build
```

This will compile the project and create an executable file in the target directory. To run the project, you can use the following command:

```rust
cargo run
```

Here is a summary of the `rustup` and `cargo` commands in a table format:

| Command                        | Description                                                                              |
| ------------------------------ | ---------------------------------------------------------------------------------------- |
| `rustup install stable`        | Install the latest stable version of the Rust compiler and the `cargo` tool              |
| `rustup component add rustfmt` | Install the Rustfmt tool for formatting Rust code                                        |
| `rustup component add clippy`  | Install the Clippy linting tool for Rust                                                 |
| `cargo new my-project`         | Create a new Rust project with the name "my-project"                                     |
| `cargo build`                  | Build the current project                                                                |
| `cargo run`                    | Build and run the current project                                                        |
| `cargo test`                   | Run the tests for the current project                                                    |
| `cargo doc`                    | Generate documentation for the current project                                           |
| `cargo publish`                | Publish the current project to [crates.io](https://crates.io), the Rust package registry |

### Cargo.toml

The `Cargo.toml` file is the main configuration file for a Rust project managed by `cargo`. It specifies the `name`, `version`, and `dependencies` of the project, as well as other build and development settings.

Here is an example of a simple Cargo.toml file:

```rust
[package]
name = "my-project"
version = "0.1.0"
authors = ["Your Name <your@email.com>"]

# See more keys and their definitions at
# https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
somecrate = "1.2.3"

[dev-dependencies]
mockito = "0.23"
```

The `[package]` section specifies metadata about the project, such as the name and version. The `[dependencies]` section lists the crates that the project depends on. These crates will be automatically downloaded and compiled when the project is built. The `[dev-dependencies]` section lists crates that are only needed for development and testing, and they will not be included in the final binary produced by the project.

## Hello World Program

```rust
fn main() {
    println!("Hello, world!");
}
```

- The `fn main()` is the entry point of every Rust program. It is the first function that is executed when the program runs.

- The `println!` macro is used to print a message to the console. It takes a string as an argument and prints it to the console, followed by a newline.

- The `!` after println indicates that it is a macro, rather than a function. Macros are a way to generate code at compile-time, rather than running code at runtime. They are often used for tasks such as printing messages or creating repetitive code.

Overall, this program is a simple example of how to use Rust to print a message to the console.

Here is an explanation of the syntax used in this Rust program:

| Syntax Element    | Purpose                                                                                                                                       |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| `fn`              | Defines a function in Rust.                                                                                                                   |
| `main`            | The name of the function. The `main` function is the entry point of every Rust program.                                                       |
| `()`              | Enclose the parameters of the function. In this case, the `main` function does not take any arguments.                                        |
| `{` and `}`       | Enclose the body of the function. The body of a function is the code that is executed when the function is called.                            |
| `println!`        | A macro that is used to print a message to the console. It takes a string as an argument and prints it to the console, followed by a newline. |
| `"Hello, world!"` | A string literal that represents the message to be printed. It is enclosed in double quotes.                                                  |

## GCD Euclidean Algorithm

```rust
fn gcd(mut n: u64, mut m: u64) -> u64 {
    assert!(n != 0 && m != 0);
    while m != 0 {
        if m < n {
            let t = m;
            m = n;
            n = t;
        }
        m = m % n;
    }
    n
}
```

This code defines a function called greatest common divisor (`GCD`) that takes two arguments: `n` and `m`, both of which are unsigned 64-bit integers (`u64`). The function returns a value of type `u64`, which is the `GCD` of `n` and `m`.

Here is a breakdown of the function into smaller parts, with a description of what each part does:

### Part 1: Check for zero values

```rust
assert!(n != 0 && m != 0);
```

This part checks if either `n` or `m` is zero, and if either is zero, it causes the program to panic. The `assert!` macro is used to perform this check. It will only execute if the condition (`n != 0 && m != 0)` is not met. This check is important because the GCD of two numbers is not defined if either number is zero.

### Part 2: Initialize loop

```rust
while m != 0 {
    // Loop body goes here
}
```

This part initializes a while loop that continues until `m` becomes zero. The loop condition is `m != 0`, which means the loop will continue as long as `m` is not equal to zero. The loop body, which contains the remaining steps of the function, goes between the curly braces (`{}`).

### Part 3: Swap values of n and m if necessary

```rust
if m < n {
    let t = m;
    m = n;
    n = t;
}
```

This part checks if `m` is less than `n`, and if it is, the values of `n` and `m` are swapped. This is done using a `let` statement to declare a temporary variable `t`, which is set to the value of `m`. The values of `m` and `n` are then swapped using the temporary variable. This step ensures that n is always the smaller of the two numbers.

### Part 4: Update value of m

```rust
m = m % n;
```

This part updates the value of m to be the remainder of `m` divided by `n`. It uses the modulo operator (`%`) to calculate the remainder. For example, if m is 7 and n is 3, then `m` would be updated to be 1 (the remainder of 7 divided by 3).

### Part 5: Return result

```rust
n
```

This is the final part of the function, and it simply returns `n` as the result. The GCD of `n` and `m` is the value of `n` when the loop terminates, because this is the largest number that can divide both `n` and m without leaving a remainder.

Here is an ultra-summary of the steps in the function
| Name | Description |
|---------|--------------------------------------------------------------------------------------------------|
| Check | Check if either `n` or `m` is zero and return zero if either is zero. |
| Loop | Initialize a loop that continues until `m` becomes zero. |
| Swap | Inside the loop, check if `m` is less than `n`. If it is, swap the values of `n` and `m`. |
| Update | Update the value of `m` to be the remainder of `m` divided by `n`. |
| Return | The loop ends when `m` becomes zero. At this point, `n` is returned as the result. |

This function has a time complexity of O(log(n)), which means that the number of steps required to compute the GCD grows at most logarithmically with the size of the input numbers. This makes the function efficient for calculating the GCD of large numbers.

## Writing and Running Unit Tests

To write and run unit tests in Rust, you'll need to use the #[test] attribute, which marks a function as a unit test. You can then run your tests using the cargo test command.

To test this function, you can define some test cases and then use assertions to check that the function returns the expected results for those test cases. Here is an example of how you could do this in Rust:

```rust
#[test]
fn test_gcd() {
    assert_eq!(gcd(14, 21), 7);
    assert_eq!(gcd(49, 14), 7);
    assert_eq!(gcd(1, 100), 1);
    assert_eq!(gcd(6, 9), 3);
    assert_eq!(gcd(0, 5), 5);
}
```

| Input values | Expected output |
| ------------ | --------------- |
| (14, 21)     | 7               |
| (49, 14)     | 7               |
| (1, 100)     | 1               |
| (6, 9)       | 3               |
| (0, 5)       | 5               |

This defines a test function test_gcd that contains five test cases. The assert_eq! macro checks that the value returned by the gcd function is equal to the expected result. If any of the assertions fails, the test function will produce an error.

The `#[test]` marker is an example of an attribute.
In Rust, attributes are a way to attach additional information to your code.
They are used for a wide range of purposes, **including setting compiler options**, **defining test functions**, and **controlling how functions are called**.

Attributes are written in square brackets and are placed before the item they apply to. For example, the `#[test]` attribute is used to mark a function as a unit test, and the `#[should_panic]` attribute is used to indicate that a test function should panic.

Here's an example of how to use the `#[cfg]` attribute to enable or disable a block of code based on the current configuration:

```rust
#[cfg(target_os = "linux")]
fn test_function() {
    // Code that is only compiled on Linux goes here
}
```

Attributes are a powerful feature in Rust and are used extensively in the Rust ecosystem.

## Handling Command-Line Arguments

To handle command-line arguments in Rust, you can use the `std::env::args` function, which returns an iterator over the arguments passed to the program.

> Basic Structure

```rust
use std::env;

fn main() {
    let args: Vec<String> = env::args().collect();

    if args.len() != 2 {
        println!("Usage: cargo run <filename>");
        return;
    }

    let filename = &args[1];
    println!("Reading file {}", filename);
}
```

Here is a summary of the code, with each step explained in more detail:

### Step 1: Bring the env module into scope

```rust
use std::env;
```

- This line brings the `env` module from the Rust standard library into scope. This module provides functions for interacting with the environment in which the program is running, including the ability to access command line arguments.

### Step 2: Define the main function

```rust
fn main() {
    // code goes here
}
```

- The `main` function is the entry point of every Rust program. It is executed when the program is run.

### Step 3: Initialize a vector of command line arguments

```rust
let args: Vec<String> = env::args().collect();
```

- This code initializes a variable called `args` to a vector of strings that contains the command line arguments passed to the program. It does this by calling the `args` function from the `env` module, which returns an iterator over the arguments, and then calling the `collect` method on the iterator to turn it into a vector.

### Step 4: Check the number of command line arguments

```rust
if args.len() != 2 {
    println!("Usage: cargo run <filename>");
    return;
}
```

- The code checks the length of the `args` vector. If it is not equal to 2, the program prints a usage message and returns, because it expects to receive exactly one command line argument (the name of the file).

### Step 5: Read the specified file

```rust
let filename = &args[1];
println!("Reading file {}", filename);
```

### Step 6: End the program

The `main` function ends and the program execution is complete.

- If the `args` vector does have a length of 2, the code assigns the value of the second element (the name of the file) to a variable called `filename` and prints a message indicating that it is reading the file.

Here is the the ultra-summarized version without the step labels:
| Code | Explanation |
|------|-------------|
| `use std::env;` | This line allows the program to access command line arguments. |
| `fn main() {  // code goes here }` | This is the entry point of the program and is executed when the program is run. |
| `let args: Vec<String> = env::args().collect();` | This vector contains the command line arguments passed to the program. |
| `if args.len() != 2 {  println!("Usage: cargo run <filename>");  return; }` | If the number of arguments is not equal to 2, the program prints a usage message and returns. |
| `let filename = &args[1]; println!("Reading file {}", filename);` | If the number of arguments is equal to 2, the program reads the specified file and prints a message indicating this. |
| (end of `main` function) | The program execution is complete. |

> GCD command line program

```rust
use std::str::FromStr;
use std::env;

fn main() {
    // Parse command line arguments
    let mut numbers = Vec::new();
    for arg in env::args().skip(1) {
        // Attempt to parse each argument as a u64
        numbers.push(u64::from_str(&arg)
            .expect("error parsing argument"));
    }

    // Handle empty input
    if numbers.len() == 0 {
        eprintln!("Usage: gcd NUMBER ...");
        std::process::exit(1);
    }

    // Calculate the GCD
    let mut d = numbers[0];
    for m in &numbers[1..] {
        d = gcd(d, *m);
    }

    // Print the result
    println!("The greatest common divisor of {:?} is {}", numbers, d);
}
```

Here is a breakdown of the code with explanations for each part:

### Parsing command line arguments

```rust
let mut numbers = Vec::new();
for arg in env::args().skip(1) {
    numbers.push(u64::from_str(&arg)
    .expect("error parsing argument"));
}
```

This code creates a new empty vector called `numbers`, and then iterates over the command line arguments (skipping the first one, which is the name of the program itself). For each argument, it attempts to parse it as a `u64` using the `FromStr` trait's `from_str` method. If the parsing succeeds, the number is added to the `numbers` vector. If the parsing fails, the program calls the `expect` method, which will cause the program to panic and print the provided message (in this case, "error parsing argument").

### Handling empty input

```rust
if numbers.len() == 0 {
    eprintln!("Usage: gcd NUMBER ...");
    std::process::exit(1);
}
```

This code checks if the `numbers` vector is empty. If it is, the program prints a usage message using the `eprintln!` macro (which is similar to `println!`, but prints to the standard error stream instead of the standard output stream) and then exits the program with a non-zero exit code using the `exit` function from the `std::process` module. The exit code of 1 is a convention that indicates that the program encountered an error.

### Calculating the GCD

```rust
let mut d = numbers[0];
for m in &numbers[1..] {
    d = gcd(d, *m);
}
```

This code initializes a variable `d` with the first element of the `numbers` vector, and then iterates over the rest of the elements. At each iteration, it updates `d` to the GCD of `d` and the current element using the `gcd` function (which is not defined in this code snippet).

### Printing the result

```rust
println!("The greatest common divisor of {:?} is {}", numbers, d);
```

This code prints the result of the GCD calculation using the `println!` macro. The `{:?}` syntax is a placeholder for the `numbers` vector, which will be printed using the `Debug` trait. The `{}` syntax is a placeholder for the value of `d`.

Here is a table explaining the syntax elements in the code in more detail:

| Syntax element                                                        | Description                                                                           |
| --------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| `use std::str::FromStr;`                                              | Imports `FromStr` trait for parsing strings.                                          |
| `use std::env;`                                                       | Imports `env` module for interacting with environment.                                |
| `for arg in env::args().skip(1)`                                      | Iterates over command line arguments, starting from second.                           |
| `numbers.push(u64::from_str(&arg).expect("error parsing argument"));` | Parses argument as `u64` and adds to `numbers` vector.                                |
| `if numbers.len() == 0`                                               | Checks if `numbers` vector is empty.                                                  |
| `eprintln!("Usage: gcd NUMBER ...");`                                 | Prints usage message to standard error stream.                                        |
| `std::process::exit(1);`                                              | Exits with non-zero exit code.                                                        |
| `let mut d = numbers[0];`                                             | Declares mutable variable `d` and initializes with first element of `numbers` vector. |
| `for m in &numbers[1..]`                                              | Iterates over elements of `numbers` vector, starting from second element.             |
| `d = gcd(d, *m);`                                                     | Updates `d` to GCD of `d` and current element `m`.                                    |
| `println!("The greatest common divisor of {:?} is {}", numbers, d);`  | Prints result of GCD calculation.                                                     |

### FromStr Trait

A trait is a collection of methods that types can implement.
The `FromStr` trait is a Rust trait that provides a method for parsing a string as a value of a specific type.
It is defined in the `std::str` module, and it can be implemented for any type that can be constructed from a string.
Any type that implements the `FromStr` trait has a `from_str` method that tries to parse a value of that type from a
string.

Here is the definition of the `FromStr` trait:

```rust
pub trait FromStr: Sized {
    type Err;

    fn from_str(s: &str) -> Result<Self, Self::Err>;
}
```

The `from_str` method takes a `&str` as input and returns a `Result` with the parsed value or an error value of type `Self::Err`. The `Sized` trait indicates that the type implementing `FromStr` must have a fixed size.

Here is the example of implementing the `FromStr` trait for the `Point` type:

```rust
use std::str::FromStr;

#[derive(Debug)]
struct Point {
    x: i32,
    y: i32,
}

impl FromStr for Point {
    type Err = String;

    fn from_str(s: &str) -> Result<Self, Self::Err> {
        let coords: Vec<&str> = s.split(',').collect();
        if coords.len() != 2 {
            return Err(format!("Error parsing point: {}", s));
        }
        let x = coords[0].parse::<i32>().map_err(|e| e.to_string())?;
        let y = coords[1].parse::<i32>().map_err(|e| e.to_string())?;
        Ok(Point { x, y })
    }
}

fn main() {
    let p: Point = "1,2".parse().unwrap();
    println!("{:?}", p); // prints "Point { x: 1, y: 2 }"

    let q: Result<Point, String> = "3,4,5".parse();
    println!("{:?}", q); // prints "Err("Error parsing point: 3,4,5")"
}
```

In this example, the `Point` type has two fields, `x` and `y`, which represent the coordinates of a point on a 2D plane. The `FromStr` trait is implemented for `Point` by defining a `from_str` method that splits the input string on the `','` character and tries to parse the resulting strings as `i32` values. If the input string doesn't have exactly two parts, the method returns an error. Otherwise, it constructs a new `Point` value with the parsed coordinates and returns it as an `Ok` variant of the `Result`.

You can then use the `parse` method on a string to parse it as a value of the type that implements `FromStr`. The `parse` method returns a `Result` with the parsed value or an error value, so you can use it in a `match` expression or the `?` operator to handle the possible error cases.

A trait must be in scope in order to use its methods.

### `std::env`

The `std::env` module in Rust provides functions for interacting with the environment in which a program is run. It allows you to access various properties of the environment, such as the command line arguments, the current working directory, and the environment variables.

Here is a table with a summary of the most commonly functions in the `std::env` module:

| Function          | Description                                                                |
| ----------------- | -------------------------------------------------------------------------- |
| `args`            | Returns an iterator over the command line arguments passed to the program. |
| `current_dir`     | Returns the current working directory as a `PathBuf`.                      |
| `set_current_dir` | Sets the current working directory to the provided path.                   |
| `var`             | Gets the value of an environment variable as a string.                     |
| `var_os`          | Gets the value of an environment variable as an `OsString`.                |
| `set_var`         | Sets the value of an environment variable.                                 |
| `remove_var`      | Removes an environment variable.                                           |

Here is an example of using some of these functions to print the command line arguments and the value of an environment variable:

```rust
use std::env;

fn main() {
    // Print the command line arguments
    for arg in env::args() {
        println!("{}", arg);
    }

    // Get the value of the "PATH" environment variable
    let path = env::var("PATH").unwrap_or_else(|e| {
        eprintln!("Error getting PATH: {}", e);
        std::process::exit(1);
    });
    println!("PATH: {}", path);
}
```

In this example, the `args` function is used to iterate over the command line arguments, and the `var` function is used to get the value of the `PATH` environment variable. If the `var` function returns an error, it is printed to the standard error stream and the program exits with a non-zero exit code.
