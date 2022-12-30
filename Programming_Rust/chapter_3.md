# Fundamental Types

## Rust's type inference and its powerful macro system

Type inference allows the Rust compiler to automatically deduce the types of variables and expressions based on the context in which they appear. This means that you don't always have to explicitly specify the types of your variables and expressions, as the compiler can infer them for you. This can make writing Rust code more concise and reduce the amount of boilerplate you have to write.

Rust's macro system allows you to define and use custom code generation macros, which can be used to generate repetitive or boilerplate code for you. This can help you avoid having to manually write out the same code multiple times, and can make it easier to write code that is more abstract and modular.

Overall, Rust's focus on types and its support for type inference and macros can help make it easier for you to write efficient and reliable code, while still providing the flexibility and expressiveness you need to solve complex problems.

Here are some examples of type inference in Rust:

````rust
let x = 5; // x is inferred to be an i32

let y = "hello"; // y is inferred to be a &str

let z = [1, 2, 3]; // z is inferred to be a [i32; 3]

fn add(a: i32, b: i32) -> i32 {
    a + b
}

let sum = add(5, 6); // sum is inferred to be an i32`

And here is an example of a macro in Rust:

```rust

````

`macro_rules! create_function {
($func_name:ident) => {
        fn $func_name() {
            println!("You called the {} function", stringify!($func_name));
}
}
}

create_function!(foo);
create_function!(bar);

foo(); // prints "You called the foo function"
bar(); // prints "You called the bar function"

````

In this example, the `create_function!` macro generates functions with the specified name. The `$func_name:ident` syntax specifies a macro variable that will be replaced with the name of the function being generated. The `stringify!` macro expands to the string representation of its argument, allowing us to print the name of the function being called.

## Rust Types

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

Here are some examples of the types:
Sure! Here is a more detailed explanation of each of the examples I provided:

```rust
// Signed and unsigned integers
let i: i32 = -42;
let u: u32 = 42;
````

In this example, `i` is a signed 32-bit integer with the value `-42`, and `u` is an unsigned 32-bit integer with the value `42`. In Rust, signed integers are denoted by a `i` followed by a number indicating the number of bits they use (e.g., `i8`, `i16`, `i32`, `i64`, `i128`). Unsigned integers are denoted by a `u` followed by a number indicating the number of bits they use (e.g., `u8`, `u16`, `u32`, `u64`, `u128`).

```rust
// Floating-point numbers
let f1: f32 = 3.14;
let f2: f64 = 6.022e23;
```

In this example, `f1` is a single-precision floating-point number with the value `3.14`, and `f2` is a double-precision floating-point number with the value `6.022e23`. In Rust, floating-point numbers are denoted by the type `f32` for single-precision numbers and `f64` for double-precision numbers.

```rust
// Boolean
let b: bool = true;
```

In this example, `b` is a boolean variable with the value `true`. The boolean type in Rust is denoted by `bool` and can have the values `true` or `false`.

```rust
// Unicode character
let c: char = 'X';
```

In this example, `c` is a Unicode character with the value `'X'`. The `char` type in Rust represents a single Unicode character and is 32 bits wide.

```rust
// Empty tuple
let unit: () = ();
```

In this example, `unit` is an empty tuple, also known as the unit type. The unit type represents a value that does not have any meaningful data associated with it. It is denoted by the empty tuple `()`.

```rust
// Struct
struct Point {
    x: f32,
    y: f32,
}
let point = Point { x: 0.0, y: 0.0 };
```

In this example, `Point` is a struct with two fields, `x` and `y`, both of which are floating-point numbers. The `point` variable is an instance of this struct, with the values `0.0` and `0.0` for the `x` and `y` fields, respectively.

```rust
// Tuple
let tuple: (i32, char) = (42, 'X');
```

In this example, `tuple` is a tuple with two elements, an integer and a character. The type of the tuple is written as a comma-separated list of the types of its elements, in this case `(i32, char)`.

```rust
// Enum
enum Color {
    Red,
    Green,
    Blue,
}
let color = Color::Red;
```

In this example, `Color` is an enumeration (enum) with three values: `Red`, `Green`, and `Blue`. Enums are used to define a fixed set of values, each of which can optionally have an associated payload of data. In this case, the `Color` enum has three values, each of which has no associated data. The `color` variable is set to the value `Color::Red`, which is one of the values of the `Color` enum.

```rust
// Box
let boxed: Box<i32> = Box::new(42);
```

In this example, `boxed` is a `Box` that owns an integer value in the heap. Boxes are smart pointers that manage the memory for their values and ensure that the values are properly deallocated when they are no longer needed. The `Box::new()` function is used to create a new box containing the given value.

```rust
// Reference
let x = 5;
let y = &x;
```

In this example, `x` is an integer with the value `5`, and `y` is a reference to `x`. References are non-owning pointers to a value and are denoted by the `&` operator. In this case, `y` is a reference to `x`, so it points to the same value as `x`, but it does not own it.

```rust
// Array
let array: [i32; 3] = [1, 2, 3];
```

In this example, `array` is an array with three elements, all of which are 32-bit integers. Arrays in Rust are fixed-length collections of values of the same type. The type of an array is written as `[T; N]`, where `T` is the type of the elements and `N` is the length of the array.

```rust
// Vector
let mut vec = vec![1, 2, 3];
vec.push(4);
```

In this example, `vec` is a vector with three elements, all of which are integers. Vectors are dynamically-sized arrays in Rust, and they can grow or shrink as needed. The `vec!` macro is used to create a new vector with the given elements. The `push()` method is used to add an element to the end of the vector.

```rust
// Slice
let slice = &vec[1..3];
```

In this example, `slice` is a slice that refers to a portion of the `vec` vector. Slices are references to a portion of an array or vector, comprising a pointer and a length. They are written as `&[T]`, where `T` is the type of the elements. In this case, `slice` refers to the elements of `vec` at indices 1 and 2 (the slice is half-open, so it includes the start index but not the end index).

```rust
// Option
let opt: Option<i32> = Some(5);
```

In this example, `opt` is an `Option` type that represents a value that may or may not be present. The `Option` type is written as `Option<T>`, where `T` is the type of the value that may be present. The `Option` type can have two values: `Some(T)`, which represents a value that is present, and `None`, which represents a value that is absent. In this case, `opt` is set to `Some(5)`, which means it has the value `5`.

```rust
// Result
let res: Result<i32, &str> = Ok(5);
```

In this example, `res` is a `Result` type that represents the result of an operation that may fail. The `Result` type is written as `Result<T, E>`, where `T` is the type of the success value and `E` is the type of the error value. The `Result` type can have two values: `Ok(T)`, which represents a successful result with a value of type `T`, and `Err(E)`, which represents a failed result with an error value of type `E`. In this case, `res` is set to `Ok(5)`, which means the operation was successful and the result is the value `5`.

## Fixed-Width Numeric Types

### Integer Types

In Rust, the `u8`, `i8`, `u16`, `i16`, `u32`, `i32`, `u64`, `i64`, `u128`, and `i128` types are fixed-width numeric types. These types represent integers with a fixed number of bits, which means that they can represent a range of values that is determined by the number of bits they use.

For example, an `i8` is a signed 8-bit integer, which means it can represent any integer value between -128 and 127, inclusive. An `u16` is an unsigned 16-bit integer, which means it can represent any integer value between 0 and 65535, inclusive.

Fixed-width numeric types are useful when you need to ensure that a value will always be stored in a specific number of bits. This can be important in situations where the size of the value is critical, such as when working with hardware devices or when communicating with other systems.

It's also worth noting that Rust has type aliases for the fixed-width integer types that correspond to the native word size of the target architecture. These types are `isize` and `usize`, and they are either 32 or 64 bits wide depending on the architecture. These types are typically used when you need an integer type that is the same size as a pointer on the machine.

Certainly! Here is a summary of the fixed-width numeric types in Rust in the form of a table:
Certainly! Here is a summary of the integer types in Rust, along with some example uses of each type:

| Type     | Width (bits) | Range                                                                               | Example Uses                                          |
| -------- | ------------ | ----------------------------------------------------------------------------------- | ----------------------------------------------------- |
| `i8`     | 8            | -128 to 127                                                                         | Small integers, counts, flags, enumerations           |
| **`u8`** | 8            | 0 to 255                                                                            | Small counts, flags, enumerations                     |
| `i16`    | 16           | -32768 to 32767                                                                     | Medium-sized integers, counts, flags, enumerations    |
| `u16`    | 16           | 0 to 65535                                                                          | Medium-sized counts, flags, enumerations              |
| `i32`    | 32           | -2147483648 to 2147483647                                                           | Large integers, counts, flags, enumerations           |
| `u32`    | 32           | 0 to 4294967295                                                                     | Large counts, flags, enumerations                     |
| `i64`    | 64           | -9223372036854775808 to 9223372036854775807                                         | Very large integers, counts, flags, enumerations      |
| `u64`    | 64           | 0 to 18446744073709551615                                                           | Very large counts, flags, enumerations                |
| `i128`   | 128          | -170141183460469231731687303715884105728 to 170141183460469231731687303715884105727 | Extremely large integers, counts, flags, enumerations |
| `u128`   | 128          | 0 to 340282366920938463463374607431768211455                                        | Extremely large counts, flags, enumerations           |
| `isize`  | 32 or 64     | depends on architecture                                                             | Pointer-sized integers                                |
| `usize`  | 32 or 64     | depends on architecture                                                             | Pointer-sized counts                                  |

Here are some examples of using the fixed-width numeric types in Rust:

```rust
// Declare and initialize variables with fixed-width integer types
let x: i8 = 42;
let y: u8 = 255;
let z: i16 = -32768;

// Perform arithmetic operations with fixed-width integer types
let a: i32 = x + y as i32; // Convert y to i32 before adding
let b: u16 = z as u16 + 1; // Convert z to u16 before adding

// Compare fixed-width integer types
let c: bool = x < y as i8; // Convert y to i8 before comparing
let d: bool = z == -32768;

// Convert between fixed-width integer types
let e: u8 = x as u8; // Convert x to u8
let f: i16 = y as i16; // Convert y to i16

// Use type aliases for the native word size
let g: isize = -9223372036854775808;
let h: usize = 18446744073709551615;
```

> Byte literals

- In Rust, a byte literal is a way to represent a single byte value in a string or character literal.

- Byte literals can only represent ASCII and Unicode characters that can be encoded as a single byte in UTF-8. This means that they can't be used to represent characters that require multiple bytes to encode, such as most non-ASCII Unicode characters.

- Byte literals are used to represent the raw byte value of a character in a string or character literal. This can be useful when working with non-UTF-8 encodings, or when you need to manipulate individual bytes in a string.

- Byte literals are written as a single ASCII character preceded by a b and enclosed in single quotes. For example, the byte literal for the ASCII character A is b'A'.

  Here is an example of using byte literals in Rust:

  ```rust
  let b1: u8 = b'A'; // A byte literal representing the ASCII character 'A'
  let b2: u8 = b'\n'; // A byte literal representing the ASCII character '\n'
  let b3: u8 = b'\x7f'; // A byte literal representing the ASCII character '\x7f'
  let b4: u8 = b'\xff'; // A byte literal representing the ASCII character '\xff'

  let s1: &[u8] = b"hello"; // A byte string literal representing the ASCII string "hello"
  let s2: &[u8] = b"\x00\x01\x02\x03"; // A byte string literal representing the bytes [0x00, 0x01, 0x02, 0x03]
  ```

- Byte literals can also be written using escape sequences, which allow you to represent certain ASCII and Unicode characters using special codes. Some common escape sequences that can be used in byte literals are:

  Here is a summary of the character byte literals and their corresponding numeric equivalents:

  | Character         | Byte Literal  | Numeric Equivalent   |
  | ----------------- | ------------- | -------------------- |
  | Single quote, '   | b'''          | 39u8                 |
  | Backslash, \      | b'\\'         | 92u8                 |
  | Newline           | b'\\n'        | 10u8                 |
  | Carriage return   | b'\\r'        | 13u8                 |
  | Tab               | b'\\t'        | 9u8                  |
  | ASCII character   | b'\\xhh'      | hh (hexadecimal)     |
  | Unicode character | b'\\u{hhhhh}' | hhhhhh (hexadecimal) |

  Note that these byte literals are not the same as ASCII character literals, which are written using regular characters and are not preceded by a `b`. For example, the ASCII character literal for the single quote character is `'\''` (with no `b`), and the numeric equivalent is 39i8.

  Here is a list of some common keyboard keys and the hexadecimal codes that can be used to represent them in Rust using hexadecimal byte literals:

  | Keyboard Key | Hexadecimal Code | Byte Literal |
  | ------------ | ---------------- | ------------ |
  | Backspace    | `08`             | `b'\x08'`    |
  | Tab          | `09`             | `b'\x09'`    |
  | Enter        | `0d`             | `b'\x0d'`    |
  | Escape       | `1b`             | `b'\x1b'`    |
  | Space        | `20`             | `b'\x20'`    |
  | Digit 0      | `30`             | `b'\x30'`    |
  | Digit 1      | `31`             | `b'\x31'`    |
  | Digit 2      | `32`             | `b'\x32'`    |
  | Digit 3      | `33`             | `b'\x33'`    |
  | Digit 4      | `34`             | `b'\x34'`    |
  | Digit 5      | `35`             | `b'\x35'`    |
  | Digit 6      | `36`             | `b'\x36'`    |
  | Digit 7      | `37`             | `b'\x37'`    |
  | Digit 8      | `38`             | `b'\x38'`    |
  | Digit 9      | `39`             | `b'\x39'`    |
  | A            | `41`             | `b'\x41'`    |
  | B            | `42`             | `b'\x42'`    |
  | C            | `43`             | `b'\x43'`    |
  | D            | `44`             | `b'\x44'`    |
  | E            | `45`             | `b'\x45'`    |
  | F            | `46`             | `b'\x46'`    |
  | G            | `47`             | `b'\x47'`    |
  | H            | `48`             | `b'\x48'`    |
  | I            | `49`             | `b'\x49'`    |
  | J            | `4a`             | `b'\x4a'`    |
  | K            | `4b`             | `b'\x4b'`    |
  | L            | `4c`             | `b'\x4c'`    |
  | M            | `4d`             | `b'\x4d'`    |
  | N            | `4e`             | `b'\x4e'`    |
  | O            | `4f`             | `b'\x4f'`    |
  | P            | `50`             | `b'\x50'`    |
  | Q            | `51`             | `b'\x51'`    |
  | R            | `52`             | `b'\x52'`    |
  | S            | `53`             | `b'\x53'`    |
  | T            | `54`             | `b'\x54'`    |
  | U            | `55`             | `b'\x55'`    |
  | V            | `56`             | `b'\x56'`    |
  | W            | `57`             | `b'\x57'`    |
  | X            | `58`             | `b'\x58'`    |
  | Y            | `59`             | `b'\x59'`    |
  | Z            | `5a`             | `b'\x5a'`    |
  | Delete       | `7f`             | `b'\x7f'`    |

  Here is an example of using the b'\x7f' byte literal in Rust:

  ```rust
  let key: u8 = b'\x7f';

  if key == b'\x7f' {
      println!("The delete key was pressed!");
  }
  ```

In Rust, you can use the `as` operator to convert from one integer type to another. This can be useful when you need to perform arithmetic or other operations on integers with different precisions or when you need to match the type of an integer to a particular function or API.

Here is an example of using the `as` operator to convert an `i32` value to an `i64` value:

```rust
let x: i32 = 42;
let y: i64 = x as i64;

println!("x = {} (i32), y = {} (i64)", x, y);
```

This will print `x = 42 (i32), y = 42 (i64)`.

It's also possible to use the `as` operator to convert from an unsigned integer type to a signed integer type, as long as the value is within the range of the signed type. For example:

```rust
let x: u32 = 42;
let y: i32 = x as i32;

println!("x = {} (u32), y = {} (i32)", x, y);
```

This will also print `x = 42 (u32), y = 42 (i32)`.

## Checked, Wrapping, Saturating, and Overflowing Arithmetic

In computer arithmetic, an "overflow" condition occurs when the result of an arithmetic operation exceeds the maximum value that can be represented by the type being used. For example, in Rust, the maximum value that can be represented by an `u8` type is `255`. If you try to perform an arithmetic operation that would produce a result greater than 255, an overflow condition occurs.

An "underflow" condition is the opposite of an overflow condition: it occurs when the result of an arithmetic operation is less than the minimum value that can be represented by the type being used. For example, in Rust, the minimum value that can be represented by an i8 type is -128. If you try to perform an arithmetic operation that would produce a result less than -128, an underflow condition occurs.

In Rust, you can perform arithmetic operations on integers using the standard arithmetic operators (e.g., `+`, `-`, `*`, `/`, `%`). By default, these operators will perform "unchecked" arithmetic, which means that they will not check for overflow or underflow conditions. This can be useful when you want to maximize performance, but it can also be dangerous if you are not careful, as it can lead to undefined behavior if an overflow or underflow occurs.

```rust
// Overflow example
let x: u8 = 255;
let y: u8 = 1;

let result = x + y;

// The result of this operation is 256, which is greater than the maximum value that can be represented by an u8 (255).
// Therefore, an overflow condition occurs, and the result of the operation is undefined.

// Underflow example
let x: i8 = -128;
let y: i8 = -1;

let result = x - y;

// The result of this operation is 127, which is greater than the maximum value that can be represented by an i8 (-128).
// Therefore, an underflow condition occurs, and the result of the operation is undefined.
```

To avoid this, you can use the `checked_*` variants of the arithmetic operators, which check for overflow and underflow and return `None` if an overflow or underflow occurs. Alternatively, you can use the `Wrapping` type from the `std::num` module to perform wrapping arithmetic, or the `saturating_*` variants of the arithmetic operators to perform saturating arithmetic.

To perform checked arithmetic in Rust, you can use the `checked_*` variants of the arithmetic operators. For example, the `checked_add` function performs addition that checks for overflow, and returns `None` if an overflow occurs:

```rust
use std::num::Wrapping;

let x: u8 = 255;
let y: u8 = 1;

let result = x.checked_add(y);

assert_eq!(result, None);
```

You can also use the `Wrapping` type from the `std::num` module to perform wrapping arithmetic, which wraps around instead of overflowing or underflowing. For example:

```rust
use std::num::Wrapping;

let x: Wrapping<u8> = Wrapping(255);
let y: Wrapping<u8> = Wrapping(1);

let result = x + y;

assert_eq!(result.0, 0);
```

In addition, you can use the `saturating_*` variants of the arithmetic operators to perform saturating arithmetic, which saturates to the maximum or minimum value of the type instead of overflowing or underflowing. For example:

```rust
use std::num::Wrapping;

let x: u8 = 255;
let y: u8 = 1;

let result = x.saturating_add(y);

assert_eq!(result, 255);
```

Finally, you can use the `overflowing_*` variants of the arithmetic operators to perform overflowing arithmetic and get a boolean value indicating whether an overflow occurred. For example:

```rust
let x: u8 = 255;
let y: u8 = 1;

let (result, overflowed) = x.overflowing_add(y);

assert_eq!(result, 0);
assert_eq!(overflowed, true);
```

In Rust, the names of the operations that follow the `checked_`, `wrapping_`, `saturating_`, or `overflowing_` prefix are the same as the names of the corresponding standard arithmetic operations (e.g., `add`, `sub`, `mul`, `div`, `rem`), with the exception of the `shl` and `shr` operations, which are used for bitwise shifting.

Here are some examples of using the `checked_`, `wrapping_`, `saturating_`, and `overflowing_` prefixes with the various arithmetic operations:

```rust
use std::num::Wrapping;

// Addition
let x = 1u8;
let y = 2u8;

let checked_result = x.checked_add(y);
let wrapping_result = Wrapping(x).wrapping_add(Wrapping(y)).0;
let saturating_result = x.saturating_add(y);
let (overflowing_result, overflowed) = x.overflowing_add(y);

// Subtraction
let x = 1u8;
let y = 2u8;

let checked_result = x.checked_sub(y);
let wrapping_result = Wrapping(x).wrapping_sub(Wrapping(y)).0;
let saturating_result = x.saturating_sub(y);
let (overflowing_result, overflowed) = x.overflowing_sub(y);

// Multiplication
let x = 2u8;
let y = 3u8;

let checked_result = x.checked_mul(y);
let wrapping_result = Wrapping(x).wrapping_mul(Wrapping(y)).0;
let saturating_result = x.saturating_mul(y);
let (overflowing_result, overflowed) = x.overflowing_mul(y);

assert_eq!(checked_result, Some(6));
assert_eq!(wrapping_result, 6);
assert_eq!(saturating_result, 6);
assert_eq!(overflowing_result, 6);
assert_eq!(overflowed, false);

// Division
let x = 10u8;
let y = 3u8;

let checked_result = x.checked_div(y);
let wrapping_result = Wrapping(x).wrapping_div(Wrapping(y)).0;

// Remainder
let x = 10u8;
let y = 3u8;

let checked_result = x.checked_rem(y);
let wrapping_result = Wrapping(x).wrapping_rem(Wrapping(y)).0;

// Left shift
let x = 1u8;
let y = 2u8;

let checked_result = x.checked_shl(y);
let wrapping_result = Wrapping(x).wrapping_shl(y as u32).0;
let (overflowing_result, overflowed) = x.overflowing_shl(y);

// Right shift
let x = 1u8;
let y = 2u8;

let checked_result = x.checked_shr(y);
let wrapping_result = Wrapping(x).wrapping_shr(y as u32).0;
let (overflowing_result, overflowed) = x.overflowing_shr(y);
```

Certainly! Here is a summary of the operation names that follow the `checked_`, `wrapping_`, `saturating_`, or `overflowing_` prefix:

| Operation      | Prefix            | Description                                                                                                          |
| -------------- | ----------------- | -------------------------------------------------------------------------------------------------------------------- |
| Addition       | `checked_add`     | Performs checked addition, returning `None` if an overflow or underflow occurs.                                      |
|                | `wrapping_add`    | Performs wrapping addition, wrapping around on overflow or underflow.                                                |
|                | `saturating_add`  | Performs saturating addition, returning the maximum or minimum value if an overflow or underflow occurs.             |
|                | `overflowing_add` | Performs overflowing addition, returning a boolean value indicating whether an overflow or underflow occurred.       |
| Subtraction    | `checked_sub`     | Performs checked subtraction, returning `None` if an overflow or underflow occurs.                                   |
|                | `wrapping_sub`    | Performs wrapping subtraction, wrapping around on overflow or underflow.                                             |
|                | `saturating_sub`  | Performs saturating subtraction, returning the maximum or minimum value if an overflow or underflow occurs.          |
|                | `overflowing_sub` | Performs overflowing subtraction, returning a boolean value indicating whether an overflow or underflow occurred.    |
| Multiplication | `checked_mul`     | Performs checked multiplication, returning `None` if an overflow or underflow occurs.                                |
|                | `wrapping_mul`    | Performs wrapping multiplication, wrapping around on overflow or underflow.                                          |
|                | `saturating_mul`  | Performs saturating multiplication, returning the maximum or minimum value if an overflow or underflow occurs.       |
|                | `overflowing_mul` | Performs overflowing multiplication, returning a boolean value indicating whether an overflow or underflow occurred. |
| Division       | `checked_div`     | Performs checked division, returning `None` if an overflow or underflow occurs or if the divisor is zero.            |
|                | `wrapping_div`    | Performs wrapping division, wrapping around on overflow or underflow or if the divisor is zero.                      |
| Remainder      | `checked_rem`     | Performs checked remainder, returning `None` if an overflow or underflow occurs or if the divisor is zero.           |
|                | `wrapping_rem`    | Performs wrapping remainder, wrapping around on overflow or underflow or if the divisor is zero.                     |
| Left shift     | `checked_shl`     | Performs checked left shift, returning `None` if an overflow or underflow occurs.                                    |
|                | `wrapping_shl`    | Performs wrapping left shift, wrapping around on overflow or underflow.                                              |
|                | `overflowing_shl` | Performs overflowing left shift, returning a boolean value indicating whether an overflow or underflow occurred.     |
| Right shift    | `checked_shr`     | Performs checked right shift, returning `None` if an overflow or underflow occurs.                                   |
|                | `wrapping_shr`    | Performs wrapping right shift, wrapping around on overflow or underflow.                                             |
|                | `overflowing_shr` | Performs overflowing right shift, returning a boolean value indicating whether an overflow or underflow occurred.    |
