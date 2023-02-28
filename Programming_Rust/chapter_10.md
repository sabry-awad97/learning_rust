# Enums and Patterns

Enums and patterns are powerful concepts in Rust programming that allow you to define a set of possible values for a variable and handle those values in a flexible and efficient way.

## Enums

Enums, short for "enumerations", are types that allow you to define a set of named values, known as "variants". Each variant can have an associated value or be empty. Enums are useful when you need to represent a set of related values that are known at compile time.

Enums are declared using the `enum` keyword followed by the name of the enum and its variants. Each variant can have zero or more associated values.

```rs
enum TrafficLight {
    Red,
    Yellow,
    Green,
}
```

This enum has three variants, each representing a different traffic light color.

Enums are often used to represent a finite set of options for a given value, such as days of the week or menu options. They can also be used to represent states or conditions in a program, such as the status of a network connection or the mode of an application.

In memory, values of C-style enums are stored as integers. Occasionally it’s useful to tell Rust which integers to use:

```rs
enum HttpStatus {
    Ok = 200,
    NotModified = 304,
    NotFound = 404,
    ...
}
```

By default, Rust stores C-style enums using the smallest built-in integer type that can accommodate them. Most fit in a single byte:

```rs
use std::mem::size_of;
assert_eq!(size_of::<TrafficLight>(), 1);
assert_eq!(size_of::<HttpStatus>(), 2);
```

Casting a C-style enum to an integer is allowed:

```rs
assert_eq!(HttpStatus::Ok as i32, 200);
// casting in the other direction, from the integer to the enum, is not.
```

Enums can also be used in combination with other Rust features such as pattern matching and traits.
Here's an example of a match expression that uses an enum:

```rs
let light = TrafficLight::Green;

match light {
    TrafficLight::Red => println!("Stop!"),
    TrafficLight::Yellow => println!("Slow down."),
    TrafficLight::Green => println!("Go!"),
}
```

Enums can have methods just like structs:

```rs
#[derive(Copy, Clone, Debug, PartialEq, Eq)]
enum TimeUnit {
    Seconds, Minutes, Hours, Days, Months, Years,
}

impl TimeUnit {
    /// Return the plural noun for this time unit.
    fn plural(self) -> &'static str {
        match self {
            TimeUnit::Seconds => "seconds",
            TimeUnit::Minutes => "minutes",
            TimeUnit::Hours => "hours",
            TimeUnit::Days => "days",
            TimeUnit::Months => "months",
            TimeUnit::Years => "years",
        }
    }

    /// Return the singular noun for this time unit.
    fn singular(self) -> &'static str {
        self.plural().trim_end_matches('s')
    }
}

let unit = TimeUnit::Hours;
println!("{} is the singular form of {}", unit.singular(), unit.plural());
```

### Enums with Data

Enums can also have associated values:

```rs
enum TrafficLight {
    Red(u8),
    Yellow(u8),
    Green(u8),
}

let light = TrafficLight::Green(30);

match light {
    TrafficLight::Red(time) => println!("Stop for {} seconds.", time),
    TrafficLight::Yellow(time) => println!("Slow down for {} seconds.", time),
    TrafficLight::Green(time) => println!("Go for {} seconds.", time),
}
```

### Enums in memory

Enums in Rust are represented in memory using a tag or discriminant value, which is an integer that identifies the variant of the enum. The size of the tag depends on the number of variants in the enum. For example, if an enum has only two variants, the tag size will be one bit, because a single bit can represent two possible values (0 or 1). If an enum has more than two variants, the tag size will be a larger integer that can represent all possible variants.

When an enum value is created, its tag is stored in memory along with any associated data for the variant. If the variant has no associated data, the tag is the only value stored in memory.

Here's an example of an enum with two variants, `Red` and `Blue`, each with no associated data:

```rs
enum Color {
    Red,
    Blue,
}
```

In memory, an instance of this enum would be represented by a single byte, with the value `0` representing `Red` and the value `1` representing `Blue`.

If we add associated data to the `Color` enum, such as a tuple of RGB values for each color, the memory representation becomes more complex. Here's an example:

```rs
enum Color {
    Red(u8, u8, u8),
    Blue(u8, u8, u8),
}
```

In this case, an instance of the `Color` enum would be represented by two bytes. The first byte would be the tag value, with `0` representing `Red` and `1` representing `Blue`. The second byte would be the RGB tuple associated with the variant. Each tuple element is one byte, so the tuple takes up three bytes in total.

Enums with associated data can become more memory-intensive as the amount of associated data increases. For example, if the `Color` enum had an associated image data field, it could take up many kilobytes or even megabytes of memory, depending on the size of the image. It's important to keep this in mind when designing enums and choosing appropriate representations for associated data.

## Rich Data Structures Using Enums

Enums in Rust are a powerful tool for creating rich and expressive data structures. By combining enums with associated data and methods, you can create data structures that are both flexible and type-safe.

For example, consider a simple data structure to represent a rectangle. We could use a struct with four fields for the x and y coordinates of the top-left corner, and the width and height of the rectangle:

```rs
struct Rectangle {
    x: i32,
    y: i32,
    width: i32,
    height: i32,
}
```

While this struct is straightforward and easy to use, it has a limitation: it can only represent rectangles that are aligned with the x and y axes. If we wanted to represent rectangles that are rotated or skewed, we would need a more complex data structure.

We can use an enum to create a more flexible data structure for rectangles. We'll define two enum variants: `AxisAligned`, which represents a rectangle that is aligned with the x and y axes, and `Rotated`, which represents a rectangle that is rotated by a specified angle. Each variant will have associated data to represent the rectangle's position, size, and rotation:

```rs
enum Rectangle {
    AxisAligned {
        x: i32,
        y: i32,
        width: i32,
        height: i32,
    },
    Rotated {
        x: i32,
        y: i32,
        width: i32,
        height: i32,
        angle: f32,
    },
}
```

We can now create rectangles that are aligned with the axes or rotated at an arbitrary angle. We can also define methods on the `Rectangle` enum to perform operations such as computing the area or checking for intersection with another rectangle.

For example, here's a method to compute the area of a rectangle:

```rs
impl Rectangle {
    fn area(&self) -> i32 {
        match self {
            Rectangle::AxisAligned { width, height, .. } => width * height,
            Rectangle::Rotated { width, height, .. } => ((width as f32).abs() * (height as f32).abs() * self.angle.cos()) as i32,
        }
    }
}
```

This method uses a match expression to compute the area of the rectangle, depending on whether it's axis-aligned or rotated. If it's axis-aligned, the area is simply the width times the height. If it's rotated, the area is computed using the formula for the area of a rotated rectangle.

By using enums and associated data, we've created a more flexible and expressive data structure for rectangles that can represent a wider variety of shapes. We've also used Rust's type system to ensure that our data structure is type-safe and easy to use.

## Generic Enums

Enums can be generic, meaning that they can have type parameters just like structs and functions. This allows you to create enums that can represent values of any type, and to write generic code that can work with any type of value that an enum can represent.

To define a generic enum, you add one or more type parameters to the enum definition, just as you would for a generic struct or function. For example, here's a simple generic enum that can represent values of any type:

```rs
enum Value<T> {
    Null,
    Number(T),
    String(String),
    Boolean(bool),
}
```

This enum has a single type parameter `T`, which can be any type. The `Null` variant represents a null value, the `Number` variant represents a numeric value of type `T`, the `String` variant represents a string value, and the `Boolean` variant represents a boolean value.

You can use this enum to create values of any type that it can represent. For example, here's how you could create a value of type `Value<f64>`:

```rs
let value = Value::Number(3.14);
```

You can also write generic code that works with any type of value that this enum can represent. For example, here's a function that takes a value of type `Value<T>` and returns its string representation:

```rs
fn to_string<T: std::fmt::Display>(value: Value<T>) -> String {
    match value {
        Value::Null => "null".to_string(),
        Value::Number(num) => num.to_string(),
        Value::String(string) => string,
        Value::Boolean(true) => "true".to_string(),
        Value::Boolean(false) => "false".to_string(),
    }
}
```

This function uses a type parameter `T` to represent the type of the value, and a trait bound `std::fmt::Display` to ensure that the value can be formatted as a string. It then matches on the value to determine its type, and returns the appropriate string representation.

By using generic enums and generic code, you can create flexible and reusable data structures and algorithms that work with any type of value that they can represent. This can make your code more concise and easier to maintain,

## Patterns

Patterns are used extensively in destructuring, matching, and binding values in Rust.
They can be used in `match` expressions, `if let` expressions, and function parameters.

```rs
let x = 42;

match x {
    0 => println!("zero"),
    1..=10 => println!("between one and ten"),
    n if n % 2 == 0 => println!("even number"),
    _ => println!("something else"),
}
```

Destructuring refers to breaking down a complex data structure into smaller components, such as extracting individual fields from a struct or elements from an array.

```rs
fn sum_tuple((x, y): (i32, i32)) -> i32 {
    x + y
}

let my_tuple = (3, 4);
let my_sum = sum_tuple(my_tuple);
println!("The sum is {}", my_sum);
```

Matching refers to testing a value against a pattern to determine if it matches the pattern, and if so, performing some action based on the matched value.
Patterns can match on different types of values, including enums, structs, tuples, and more.

```rs
let x: Option<i32> = Some(5);

match x {
    Some(n) => println!("The value is {}", n),
    None => println!("There is no value."),
}
```

Binding refers to creating a new variable and assigning a value to it based on a pattern.

### Pattern types

Patterns are used to match against values and extract their contents.

They come in different types, including:

1. Literal patterns: match against an exact value, such as a number or a string.

```rs
fn main() {
    let x = 5;
    match x {
        1 => println!("One"),
        2 => println!("Two"),
        3 => println!("Three"),
        4 => println!("Four"),
        5 => println!("Five"),
        _ => println!("Other")
    }
}
```

1. Range patterns: match against a range of values, including the end value.

```rs
fn main() {
    let x = 42;
    match x {
        0..=10 => println!("Low"),
        11..=50 => println!("Medium"),
        51..=100 => println!("High"),
        _ => println!("Other")
    }
}
```

1. Wildcard patterns: match against any value and ignore it.

```rs
fn main() {
    let x = Some(5);
    match x {
        Some(_) => println!("Some value"),
        None => println!("None")
    }
}
```

1. Variable name patterns: match against any value and move or copy it into a new local variable.

```rs
fn main() {
    let x = Some(5);
    match x {
        Some(value) => println!("The value is {}", value),
        None => println!("None")
    }
}
```

1. Ref patterns: borrow a reference to the matched value instead of moving or copying it.

```rs
fn main() {
    let x = &5;
    match x {
        &val => println!("The value is {}", val)
    }
}
```

1. Binding with subpattern patterns: match a subpattern and give it a name that can be used in the rest of the match arm.

```rs
fn main() {
    let x = (10, 20);
    match x {
        (val @ 1..=10, _) => println!("The first value is {} in range 1..=10", val),
        (_, val @ 1..=10) => println!("The second value is {} in range 1..=10", val),
        (val @ 11..=20, _) => println!("The first value is {} in range 11..=20", val),
        (_, val @ 11..=20) => println!("The second value is {} in range 11..=20", val),
        _ => println!("Other")
    }
}
```

1. Enum patterns: match against enum variants.

```rs
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState),
}

#[derive(Debug)]
enum UsState {
    Alabama,
    Alaska,
    // --snip--
}

fn main() {
    let coin = Coin::Quarter(UsState::Alaska);
    match coin {
        Coin::Penny => println!("A penny!"),
        Coin::Nickel => println!("A nickel!"),
        Coin::Dime => println!("A dime!"),
        Coin::Quarter(state) => println!("A quarter from {:?}!", state),
    }
}
```

1. Tuple patterns: match against tuples of a specific size.

```rs
fn main() {
    let point = (3, 5);
    match point {
        (0, 0) => println!("Origin"),
        (0, _) => println!("X-axis"),
        (_, 0) => println!("Y-axis"),
        (x, y) => println!("({},{})", x, y)
    }
}
```

1. Array patterns: match against arrays of a specific size.

```rs
fn main() {
    let arr = [1, 2, 3, 4, 5];
    match arr {
        [a, b, c, d, e] => println!("{}{}{}{}{}", a, b, c, d, e),
        [a, b, c, ..] => println!("{}{}{}...", a, b, c),
        _ => println!("Other")
    }
}
```

1. Slice patterns: match against slices of any size.

```rs
fn first_and_third(slice: &[i32]) -> Option<(&i32, &i32)> {
    match slice {
        [first, _, third, ..] => Some((first, third)),
        _ => None,
    }
}
```

1. Struct patterns: match against struct instances.

```rs
struct Point {
    x: i32,
    y: i32,
}

fn origin(point: &Point) -> bool {
    match point {
        Point { x: 0, y: 0 } => true,
        _ => false,
    }
}
```

1. Reference patterns: match against reference values.

```rs
fn first_element(slice: &[i32]) -> Option<&i32> {
    match slice {
        [first, ..] => Some(first),
        _ => None,
    }
}
```

1. Multiple patterns: match against multiple patterns, separated by the | symbol.

```rs
fn is_vowel(c: char) -> bool {
    match c {
        'a' | 'e' | 'i' | 'o' | 'u' => true,
        _ => false,
    }
}
```

1. Guard patterns: match against a value if a certain condition is met.

```rs
fn describe_number(x: i32) -> &'static str {
    match x {
        x if x < 0 => "negative",
        x if x == 0 => "zero",
        x if x % 2 == 0 => "even",
        _ => "odd",
    }
}
```

Each pattern has its own syntax and usage, and can be combined in complex patterns to match against complex data structures.

Here's a table summarizing the different types of patterns in Rust:

| Pattern Type            | Example                             | Notes                                                                                          |
| ----------------------- | ----------------------------------- | ---------------------------------------------------------------------------------------------- |
| Literal                 | `100`                               | Matches an exact value                                                                         |
|                         | `"name"`                            | the name of a const is also allowed                                                            |
| Range                   | `0..=100` , `'a'..='k'`             | Matches any value in the specified range, including the end value                              |
|                         | `'a'..='k'`                         |                                                                                                |
| Wildcard                | `_`                                 | Matches any value and ignores it                                                               |
| Variable Name           | `name`                              | Like `_` but moves or copies the value into a new local variable                               |
|                         | `mut count`                         |                                                                                                |
| Ref                     | `ref variable`, `ref mut field`     | Borrows a reference to the matched value instead of moving or copying it                       |
| Binding with Subpattern | `val @ 0..=99`                      | Matches the pattern to the right of `@`, using the variable name to the left                   |
|                         | `ref circle @ Shape::Circle { .. }` |                                                                                                |
| Enum                    | `Some(value)`                       | Matches against enum variants                                                                  |
|                         | `None`                              |                                                                                                |
|                         | `Pet::Orca`                         |                                                                                                |
| Tuple                   | `(key, value)`                      | Matches against tuples of a specific size                                                      |
|                         | `(r, g, b)`                         |                                                                                                |
| Array                   | `[a, b, c, d, e, f, g]`             | Matches against arrays of a specific size                                                      |
|                         | `[heading, carom, correction]`      |                                                                                                |
| Slice                   | `[first, second]`                   | Matches against slices of any size                                                             |
|                         | `[first, _, third]`                 |                                                                                                |
|                         | `[first, .., nth]`                  |                                                                                                |
| Struct                  | `Color(r, g, b)`                    | Matches against struct instances                                                               |
|                         | `Point { x, y }`                    |                                                                                                |
|                         | `Card { suit: Clubs, rank: n }`     |                                                                                                |
|                         | `Account { id, name, .. }`          |                                                                                                |
| Reference               | `&value`                            | Matches only reference values                                                                  |
|                         | `&(k, v)`                           |                                                                                                |
| Multiple Patterns       | `'a' \| 'A'`                        | In refutable patterns only (match, if let while let)                                           |
| Guard                   | `x if x * x <= r2`                  | Matches against a value if a certain condition is met - In match only (not valid in let, etc.) |

Note that not all patterns can be used in all contexts - some patterns are only valid in certain constructs, such as match expressions or function arguments.

### Refutable Patterns vs Irrefutable patterns

A refutable pattern is a pattern that may or may not match the value being matched against. If the pattern does not match, the program will simply move on to the next pattern (if there is one) or exit the match statement. If the pattern does match, the program will execute the code associated with that pattern.

```rs
let tup = (1, 2, 3);
match tup {
    (1, 2) => println!("Found (1, 2)"),
    _ => println!("Found something else"),
}

enum Color {
    Red,
    Green,
    Blue,
}
let color = Color::Red;
match color {
    Color::Green => println!("Found green"),
    Color::Blue => println!("Found blue"),
}

let x = 10;
match x {
    x if x < 0 => println!("Found a negative number"),
    x if x > 100 => println!("Found a number greater than 100"),
    _ => println!("Found something else"),
}
```

An irrefutable pattern is a pattern that will always match the value being matched.

```rs
let x = 10;
let y = match x {
    value => value + 1,
};

let x = 10;
match x {
    _ => println!("Found something"),
}
```

### Expressions vs Patterns

- Expressions are constructs that produce values, while patterns are constructs that consume values. An expression can be thought of as a value or computation that can be evaluated to produce a result, while a pattern can be thought of as a way of matching and extracting values from a larger data structure.

- The two use a lot of the same syntax.

```rs
let x = 42;

match x {
    0 => println!("zero"),
    1..=10 => println!("between one and ten"),
    n if n % 2 == 0 => println!("even number"),
    _ => println!("something else"),
}

let x = 42;
let y = if x > 0 { x } else { -x };
```

## Advantages and Disadvantages

### The advantages of enums and patterns in Rust are:

- Expressive: Enums and patterns can help make your code more expressive and easier to read by providing a concise way to represent complex values and conditions.

- Safe: Rust's type system ensures that enums and patterns are used in a safe and predictable way, preventing many common programming errors such as null pointer dereferences and invalid type conversions.

- Flexible: Enums and patterns can be used in many different ways to create powerful and reusable abstractions that can be used across your entire codebase.

### The disadvantages of enums and patterns in Rust are:

- Complexity: Enums and patterns can be complex to understand and use correctly, especially for beginners. It can take time and practice to become proficient with these features.

- Overhead: Enums and patterns can introduce some overhead in terms of code size and performance, although this is usually negligible in most cases.
