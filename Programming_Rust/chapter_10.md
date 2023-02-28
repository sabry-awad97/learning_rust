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
