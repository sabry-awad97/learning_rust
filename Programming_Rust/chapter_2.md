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
