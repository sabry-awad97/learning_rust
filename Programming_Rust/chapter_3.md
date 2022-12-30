# Fundamental Types

Here's a list of some types in rust

| Type                                | Description                                                                                     | Values                                                                       |
| ----------------------------------- | ----------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| `i8`, `i16`, `i32`, `i64`, `i128`   | Signed integers with 8, 16, 32, 64, and 128 bits of precision, respectively                     | 42, -5i8, 0x400u16, 0o100i16, 20_922_789_888_000u64, b'\*' (u8 byte literal) |
| `u8`, `u16`, `u32`, `u64`, `u128`   | Unsigned integers with 8, 16, 32, 64, and 128 bits of precision, respectively                   | 42, 0x400u16, 20_922_789_888_000u64, b'\*' (u8 byte literal)                 |
| `isize` and `usize`                 | Signed and unsigned integers with the same size as an address on the machine (32 or 64 bits)    | 137, -0b0101_0010isize, 0xffff_fc00usize                                     |
| `f32` and `f64`                     | Single and double precision floating-point numbers, respectively, using the IEEE 754 standard   | 1.61803, 3.14f32, 6.0221e23f64                                               |
| `bool`                              | Boolean                                                                                         | true, false                                                                  |
| `char`                              | Unicode character, 32 bits wide                                                                 | '\*', '\\n', '字', '\\x7f', '\\u{CA0}'                                       |
| `(char, u8, i32)`                   | Tuple: mixed types allowed                                                                      | ('%', 0x7f, -1)                                                              |
| `()`                                | "Unit" (empty tuple)                                                                            | ()                                                                           |
| `struct S { x: f32, y: f32 }`       | Named-field struct                                                                              | S { x: 120.0, y: 209.0 }                                                     |
| `struct T (i32, char)`              | Tuple-like struct                                                                               | T(120, 'X')                                                                  |
| `struct E`                          | Unit-like struct; has no fields                                                                 | E                                                                            |
| `enum Attend { OnTime, Late(u32) }` | Enumeration, algebraic data type                                                                | Attend::Late(5), Attend::OnTime                                              |
| `Box<Attend>`                       | Box: owning pointer to value in heap                                                            | Box::new(Late(15))                                                           |
| `&i32`, `&mut i32`                  | Shared and mutable references: non-owning pointers that must not outlive their referent         | &s.y, &mut v                                                                 |
| `String`                            | UTF-8 string, dynamically sized                                                                 | "ラーメン: ramen".to_string()                                                |
| `&str`                              | Reference to str: non-owning pointer to UTF-8 text                                              | "そば: soba", &s\[0..12\]                                                    |
| `[f64; 4]`, `[u8; 256]`             | Array, fixed length; elements all of same type                                                  | \[1.0, 0.0, 0.0, 1.0\], \[b' '; 256\]                                        |
| `Vec<f64>`                          | Vector, varying length; elements all of same type                                               | vec!\[0.367, 2.718, 7.389\]                                                  |
| `&[u8]`, `&mut [u8]`                | Reference to slice: reference to a portion of an array or vector, comprising pointer and length | &v\[10..20\], &mut a\[..\]                                                   |
| `Option<&str>`                      | Optional value: either `None` (absent) or `Some(v)` (present, with value `v`)                   | `Some("Dr.")`, `None`                                                        |
| `Result<u64, Error>`                | Result of operation that may fail: either a success value `Ok(v)`, or an error `Err(e)`         | `Ok(4096)`, `Err(Error::last_os_error())`                                    |
| `&dyn Any`, `&mut dyn Read`         | Trait object: reference to any value that implements a given set of methods                     | value as `&dyn Any`, `&mut file as &mut dyn Read`                            |
| `fn(&str) -> bool`                  | Pointer to function                                                                             | `str::is_empty`                                                              |

this table lists many of the different types in the Rust programming language

- The `i8`, `i16`, `i32`, `i64`, and `i128` types are signed integers of 8, 16, 32, 64, and 128 bits in size, respectively.
- The `u8`, `u16`, `u32`, `u64`, and `u128` types are unsigned integers of 8, 16, 32, 64, and 128 bits in size, respectively.
- The `isize` and `usize` types are signed and unsigned integers, respectively, that are the same size as an address on the machine. On a 32-bit machine, these types are 32 bits wide, while on a 64-bit machine, they are 64 bits wide.
- The `f32` and `f64` types are IEEE floating-point numbers, representing single and double precision, respectively.
- The `bool` type represents a boolean value, which can be either `true` or `false`.
- The `char` type represents a Unicode character, which is 32 bits wide.
- The `()` type, also known as the unit type, represents an empty tuple. It is often used to represent a value that does not have any meaningful data associated with it.

In addition to these primitive types, Rust also has a number of compound types, including:

- Structs, which are user-defined types that can have multiple fields of different types.
- Tuples, which are fixed-length collections of values of different types.
- Enums, which are types that can have a fixed set of values, each of which can have an optional payload of data.
- Boxes, which are smart pointers that own a value in the heap.
- References, which are non-owning pointers to a value.
- Arrays, which are fixed-length collections of values of the same type.
- Vectors, which are dynamically-sized arrays.
- Slices, which are references to a portion of an array or vector.
- Option&lt;T&gt;, which is a type that can either be `Some(T)` or `None`, representing a value that may or may not be present.
- Result&lt;T, E&gt;, which is a type that represents the result of an operation that may fail, with a success value of type `T` or an error value of type `E`.
- Trait objects, which are references to any value that implements a given set of methods.
- Functions, which are pointers to a block of code that can be called.
