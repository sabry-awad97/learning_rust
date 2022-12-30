# Fundamental Types

In the Rust programming language, there are several built-in "fundamental" types, which are basic types that are available in every Rust program. These types include:

1. `bool`: a boolean type that can represent either `true` or `false`.

   ```rust
   let x: bool = true;
   let y: bool = false;
   ```

1. `char`: a Unicode character, represented as a single 32-bit value.

   ```rust
   let c: char = 'a';
   ```

1. `i8`, `i16`, `i32`, `i64`, `i128`: signed integer types with 8, 16, 32, 64, and 128 bits of size, respectively.

   ```rust
   let x: i32 = -42;
   let y: i64 = 99999999;
   ```

1. `u8`, `u16`, `u32`, `u64`, `u128`: unsigned integer types with 8, 16, 32, 64, and 128 bits of size, respectively.

   ```rust
   let x: u32 = 42;
   let y: u64 = 99999999;
   `

   ```

1. `f32`, `f64`: floating-point types with 32 and 64 bits of size, respectively.

   ```rust
   let x: f32 = 3.14;
   let y: f64 = 2.71828;
   ```

1. `array`: fixed-size arrays, where the size is known at compile time.

   ```rust
   let a: [i32; 5] = [1, 2, 3, 4, 5];
   ```

1. `slice`: dynamic arrays, also known as "slices," which allow you to reference a contiguous portion of an array.

   ```rust
   let a = [1, 2, 3, 4, 5];
   let b = &a[1..3]; // a slice of the array a
   ```

1. `str`: a string slice, which is a slice of bytes that represents a string.

   ```rust
   let s: &str = "hello world";
   ```

    These are the fundamental types in Rust. They are the building blocks of the language and can be used to create more complex types and data structures.

In addition to the fundamental types, Rust also has several other built-in types that are commonly used in Rust programs. These include:

1. `tuple`: a fixed-size collection of values of different types, where the size and types of the values are known at compile time.

    ```rust
    let t: (i32, char, f64) = (42, 'a', 3.14);
    let x = t.0; // x is an i32 with a value of 42
    let y = t.1; // y is a char with a value of 'a'
    let z = t.2; // z is a f64 with a value of 3.14
    ```

1. `struct`: a custom data type that allows you to define a new type with a set of fields.

    ```rust
    struct Point {
        x: i32,
        y: i32,
    }

    let p = Point { x: 0, y: 0 };
    ```

1. `enum`: a type that represents a set of related values, each of which can have a different type.

    ```rust
    enum Color {
        Red,
        Green,
        Blue,
    }

    let c: Color = Color::Red;
    ```

1. `fn`: a type that represents a function.

    ```rust
    fn add(x: i32, y: i32) -> i32 {
        x + y
    }

    let f: fn(i32, i32) -> i32 = add;
    ```

    These are just a few of the built-in types available in Rust. There are many others, including types for working with ownership, borrowing, and concurrency, that are important to understand when programming in Rust.

In Rust, it is also possible to define your own custom types using a combination of the built-in types and the language's powerful type system. For example, you can define a type alias to give a new name to an existing type:

```rust
type Kilometers = i32;

let x: Kilometers = 5;
```

You can also define a newtype, which is a type that is defined in terms of an existing type, but is treated as a distinct type by the compiler:

```rust
struct Kilometers(i32);

let x = Kilometers(5);
```

Finally, you can define a new type using the `struct` keyword, as mentioned earlier, which allows you to define a custom data type with a set of fields:

```rust
struct Point {
    x: i32,
    y: i32,
}

let p = Point { x: 0, y: 0 };
```

By using the built-in types and the ability to define custom types, you can create complex and powerful data structures in Rust that are tailored to your specific needs.

Another important aspect of Rust's type system is the concept of generics, which allow you to define a type or function that can work with a variety of different types. For example, you can define a generic function that takes a parameter of some type `T`:

```rust
fn identity<T>(x: T) -> T {
    x
}
```

You can also define a generic struct or enum by using angle brackets to specify the type parameters:

```rust
struct Option<T> {
    value: T,
}

enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

Generics allow you to write flexible and reusable code that can work with a variety of different types without having to duplicate code or use typecasts. They are an important feature of Rust's type system and are widely used in the standard library and in many Rust programs.

Sure! Here is a summary of the fundamental types in Rust, along with their sizes and ranges:

| Type   | Size (bits) | Range                                                                                                                                                                                                                                                                                                                                |
| ------ | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `bool` | 8           | `true` or `false`                                                                                                                                                                                                                                                                                                                    |
| `char` | 32          | Any Unicode character                                                                                                                                                                                                                                                                                                                |
| `i8`   | 8           | -128 to 127                                                                                                                                                                                                                                                                                                                          |
| `i16`  | 16          | -32768 to 32767                                                                                                                                                                                                                                                                                                                      |
| `i32`  | 32          | -2147483648 to 2147483647                                                                                                                                                                                                                                                                                                            |
| `i64`  | 64          | -9223372036854775808 to 9223372036854775807                                                                                                                                                                                                                                                                                          |
| `i128` | 128         | -170141183460469231731687303715884105728 to 170141183460469231731687303715884105727                                                                                                                                                                                                                                                  |
| `u8`   | 8           | 0 to 255                                                                                                                                                                                                                                                                                                                             |
| `u16`  | 16          | 0 to 65535                                                                                                                                                                                                                                                                                                                           |
| `u32`  | 32          | 0 to 4294967295                                                                                                                                                                                                                                                                                                                      |
| `u64`  | 64          | 0 to 18446744073709551615                                                                                                                                                                                                                                                                                                            |
| `u128` | 128         | 0 to 340282366920938463463374607431768211455                                                                                                                                                                                                                                                                                         |
| `f32`  | 32          | Approximately -3.4028235e38 to 3.4028235e38                                                                                                                                                                                                                                                                                          |
| `f64`  | 64          | Approximately -1.7976931348623157081452742373170435679807056752584499659891747680315726078002853876058955863276687817154045895351438246423432132688946418276846754670353751698604991057655128207624549009038932894407586850845513394230458323690322294816580855933212334827479782620414472316873817718091929988125040402618412485836 |

Here is a summary of the other built-in types in Rust:

| Type     | Description                                                                                              |
| -------- | -------------------------------------------------------------------------------------------------------- |
| `array`  | A fixed-size array, where the size is known at compile time                                              |
| `slice`  | A dynamic array, also known as a "slice," which allows you to reference a contiguous portion of an array |
| `str`    | A string slice, which is a slice of bytes that represents a string                                       |
| `tuple`  | A fixed-size collection of values of different types, where the size and types are known at compile time |
| `struct` | A custom data type that allows you to define a new type with a set of fields                             |
| `enum`   | A type that represents a set of related values, each of which can have a different type                  |
| `fn`     | A type that represents a function                                                                        |
