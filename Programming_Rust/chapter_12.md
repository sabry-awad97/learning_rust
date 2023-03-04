# Operator overloading

## What is Operator Overloading?

Operator overloading in Rust refers to the ability to redefine the behavior of operators such as addition, subtraction, and multiplication for custom types or structs. Rust provides a mechanism to overload operators by implementing traits that define the behavior of these operators. This can make the code more concise and expressive, as well as easier to understand and maintain.

To define operator overloading in Rust, you need to implement certain traits for your types. Traits are similar to interfaces in other programming languages, in that they define a set of methods that a type must implement in order to satisfy the trait. In the case of operator overloading, the traits you need to implement are typically the `Add`, `Sub`, `Mul`, `Div`, and `Rem` traits, which correspond to the `+`, `-`, `*`, `/`, and `%` operators, respectively.

```rs
// Define a Vector2D struct
struct Vector2D {
    x: i32,
    y: i32,
}

// Implement the Add trait for Vector2D
impl std::ops::Add for Vector2D {
    type Output = Vector2D;

    fn add(self, other: Vector2D) -> Vector2D {
        Vector2D {
            x: self.x + other.x,
            y: self.y + other.y,
        }
    }
}

let v1 = Vector2D { x: 1, y: 2 };
let v2 = Vector2D { x: 3, y: 4 };
let result = v1 + v2; // result is a new Vector2D instance with x=4 and y=6
```

## Advantages of Operator Overloading

- Expressiveness: By overloading operators, you can make your code more expressive and readable, by allowing it to closely mirror the mathematical or logical operations being performed.

- Reusability: By defining how your custom types behave with certain operators, you can make your code more reusable and generic, by enabling it to work with a wider range of data types.

- Consistency: By defining how your custom types behave with certain operators, you can ensure that they behave consistently with other types that implement the same traits, making it easier to reason about your code.

## Disadvantages of Operator Overloading

While operator overloading can be a powerful feature, it's important to use it judiciously and in a way that maintains clarity and readability in your code. Overloading too many operators or using non-intuitive behavior can lead to confusion and make it more difficult for other developers to understand your code.

One potential challenge with operator overloading is that it can lead to name collisions if multiple libraries define operator overloads for the same operators. To avoid this, it's important to choose appropriate names for custom types and to use namespacing and/or modules to prevent conflicts.

Another potential challenge is that operator overloading can make it more difficult to reason about the behavior of your code, since the behavior of an operator may not be immediately apparent from its use in code. To mitigate this, it's important to provide clear documentation and to use operator overloading only in cases where it makes code more readable and expressive.

## Traits for operator overloading

Operator overloading is a powerful feature in Rust that allows you to define custom behavior for Rust's built-in operators, such as `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `<`, `<=`, `>`, `>=`, `[]`, and `[]=`.

There are several traits that you can implement to overload operators in Rust. Some of the most common traits include:

- `std::ops::Add`: This trait defines the `+` operator. You can implement it to define what happens when you add two instances of your type together.
- `std::ops::Sub`: This trait defines the `-` operator. You can implement it to define what happens when you subtract one instance of your type from another.
- `std::ops::Mul`: This trait defines the `*` operator. You can implement it to define what happens when you multiply two instances of your type together.
- `std::ops::Div`: This trait defines the `/` operator. You can implement it to define what happens when you divide one instance of your type by another.
- `std::ops::Rem`: This trait defines the `%` operator. You can implement it to define what happens when you take the remainder of dividing one instance of your type by another.
- `std::ops::Not`: This trait defines the `!` operator. You can implement it to define what happens when you apply the logical NOT operator to an instance of your type.
- `std::ops::BitAnd`: This trait defines the `&` operator. You can implement it to define what happens when you perform a bitwise AND operation between two instances of your type.
- `std::ops::BitOr`: This trait defines the `|` operator. You can implement it to define what happens when you perform a bitwise OR operation between two instances of your type.
- `std::ops::BitXor`: This trait defines the `^` operator. You can implement it to define what happens when you perform a bitwise XOR operation between two instances of your type.
- `std::ops::Shl`: This trait defines the `<<` operator. You can implement it to define what happens when you left-shift an instance of your type by a certain amount.
- `std::ops::Shr`: This trait defines the `>>` operator. You can implement it to define what happens when you right-shift an instance of your type by a certain amount.
- `std::cmp::PartialEq`: This trait defines the `==` and `!=` operators. You can implement it to define how instances of your type should be compared for equality.
- `std::cmp::PartialOrd`: This trait defines the `<`, `<=`, `>`, and `>=` operators. You can implement it to define how instances of your type should be ordered relative to each other.
- `std::ops::Index`: This trait defines the `[]` operator. You can implement it to define how instances of your type should be indexed.
- `std::ops::IndexMut`: This trait defines the `[]=` operator. You can implement it to define how instances of your type should be indexed and assigned to.

Here's the table for the operators and their corresponding traits in Rust:

| Category             | Trait                    | Operator                             |
| -------------------- | ------------------------ | ------------------------------------ |
| Unary operators      | `std::ops::Neg`          | `-x`                                 |
|                      | `std::ops::Not`          | `!x`                                 |
| Arithmetic operators | `std::ops::Add`          | `x + y`                              |
|                      | `std::ops::Sub`          | `x - y`                              |
|                      | `std::ops::Mul`          | `x * y`                              |
|                      | `std::ops::Div`          | `x / y`                              |
|                      | `std::ops::Rem`          | `x % y`                              |
| Bitwise operators    | `std::ops::BitAnd`       | `x & y`                              |
|                      | `std::ops::BitOr`        | `x`                                  |
|                      | `std::ops::BitXor`       | `x ^ y`                              |
|                      | `std::ops::Shl`          | `x << y`                             |
|                      | `std::ops::Shr`          | `x >> y`                             |
| Compound assignment  | `std::ops::AddAssign`    | `x += y`                             |
| arithmetic operators | `std::ops::SubAssign`    | `x -= y`                             |
|                      | `std::ops::MulAssign`    | `x *= y`                             |
|                      | `std::ops::DivAssign`    | `x /= y`                             |
|                      | `std::ops::RemAssign`    | `x %= y`                             |
| Compound assignment  | `std::ops::BitAndAssign` | `x &= y`                             |
| bitwise operators    | `std::ops::BitOrAssign`  | `x \|= y`                            |
|                      | `std::ops::BitXorAssign` | `x ^= y`                             |
|                      | `std::ops::ShlAssign`    | `x <<= y`                            |
|                      | `std::ops::ShrAssign`    | `x >>= y`                            |
| Comparison           | `std::cmp::PartialEq`    | `x == y`, `x != y`                   |
|                      | `std::cmp::PartialOrd`   | `x < y`, `x <= y`, `x > y`, `x >= y` |
| Indexing             | `std::ops::Index`        | `x[y]`, `&x[y]`                      |
|                      | `std::ops::IndexMut`     | `x[y] = z`, `&mut x[y]`              |

## Arithmetic and Bitwise Operators

Arithmetic and bitwise operators are commonly used in programming to perform mathematical operations and manipulate binary values, respectively. In Rust, these operators can be overloaded using traits, which allows custom types to define their own behavior for these operators.

In Rust, the expression `a + b` is actually shorthand for `a.add(b)`, a call to the `add` method of the standard library's `std::ops::Add` trait.

Here's the definition of `std::ops::Add`:

```rs
trait Add<Rhs = Self> {
type Output;
    fn add(self, rhs: Rhs) -> Self::Output;
}
```

In other words, the trait `Add<T>` is the ability to add a `T` value to itself.

For example, let's say we have a Vector2D struct that represents a two-dimensional vector.

```rs
#[derive(Debug, PartialEq)]
struct Vector2D {
    x: i32,
    y: i32,
}

impl std::ops::Add for Vector2D {
    type Output = Vector2D;

    fn add(self, other: Vector2D) -> Vector2D {
        Vector2D {
            x: self.x + other.x,
            y: self.y + other.y,
        }
    }
}

fn main() {
    let a = Vector2D { x: 1, y: 2 };
    let b = Vector2D { x: 3, y: 4 };
    let c = a + b;

    assert_eq!(c, Vector2D { x: 4, y: 6 });
}
```

### Unary Operators

Unary operators are operators that work on a single operand. Rust provides two traits for unary operator overloading: `Neg` for the negation operation and `Not` for the logical NOT operation. Here are the methods defined by these traits:

```rs
pub trait Neg {
    type Output;
    fn neg(self) -> Self::Output;
}

pub trait Not {
    type Output;
    fn not(self) -> Self::Output;
}
```

The `Neg` trait overloads the `-` operator, which performs the negation operation on the operand. The `Not` trait overloads the `!` operator, which performs the logical `NOT` operation on the operand.

```rs
use std::ops::{Neg, Not};

struct Number {
    value: i32,
}

impl Neg for Number {
    type Output = Number;
    fn neg(self) -> Number {
        Number { value: -self.value }
    }
}

impl Not for Number {
    type Output = bool;
    fn not(self) -> bool {
        self.value == 0
    }
}

fn main() {
    let x = Number { value: 5 };
    let y = -x;  // calls the neg method
    let z = !x;  // calls the not method
    println!("x = {}, y = {}, z = {}", x.value, y.value, z);
}
```

### Binary Operators

### Arithmetic Operators

```rs
pub trait Add<RHS=Self> {
    type Output;
    fn add(self, rhs: RHS) -> Self::Output;
}

pub trait Sub<RHS=Self> {
    type Output;
    fn sub(self, rhs: RHS) -> Self::Output;
}

pub trait Mul<RHS=Self> {
    type Output;
    fn mul(self, rhs: RHS) -> Self::Output;
}

pub trait Div<RHS=Self> {
    type Output;
    fn div(self, rhs: RHS) -> Self::Output;
}
```

```rs
use std::ops::{Add, Sub, Mul, Div};

struct Vector {
    x: f64,
    y: f64,
}

impl Add for Vector {
    type Output = Vector;
    fn add(self, other: Vector) -> Vector {
        Vector { x: self.x + other.x, y: self.y + other.y }
    }
}

impl Sub for Vector {
    type Output = Vector;
    fn sub(self, other: Vector) -> Vector {
        Vector { x: self.x - other.x, y: self.y - other.y }
    }
}

impl Mul<f64> for Vector {
    type Output = Vector;
    fn mul(self, scalar: f64) -> Vector {
        Vector { x: self.x * scalar, y: self.y * scalar }
    }
}

impl Div<f64> for Vector {
    type Output = Vector;
    fn div(self, scalar: f64) -> Vector {
        Vector { x: self.x / scalar, y: self.y / scalar }
    }
}

fn main() {
    let v1 = Vector { x: 1.0, y: 2.0 };
    let v2 = Vector { x: 3.0, y: 4.0 };
    let v3 = v1 + v2;  // calls the add method
    let v4 = v2 - v1;  // calls the sub method
    let v5 = v1 * 2.0; // calls the mul method
    let v6 = v2 / 2.0; // calls the div method
    println!("v1 = ({}, {})", v1.x, v1.y);
    println!("v2 = ({}, {})", v2.x, v2.y);
    println!("v3 = ({}, {})", v3.x, v3.y);
    println!("v4 = ({}, {})", v4.x, v4.y);
    println!("v5 = ({}, {})", v5.x, v5.y);
    println!("v6 = ({}, {})", v6.x, v6.y);
}
```

### Bitwise Operators

In Rust, bitwise operators can be overloaded using the following traits from the std::ops module:

- `BitAnd`: The `BitAnd` trait overloads the `&` operator for bitwise AND operations.
- `BitOr`: The `BitOr` trait overloads the `|` operator for bitwise OR operations.
- `BitXor`: The `BitXor` trait overloads the `^` operator for bitwise XOR (exclusive OR) operations.
- `Shl`: The `Shl` trait overloads the `<<` operator for left shift operations.
- `Shr`: The `Shr` trait overloads the `>>` operator for right shift operations.

These traits can be used to define custom implementations for the bitwise operators on user-defined types.

```rs
use std::ops::BitAnd;

#[derive(Debug)]
struct Bitwise {
    a: u32,
    b: u32,
}

impl BitAnd for Bitwise {
    type Output = Bitwise;

    fn bitand(self, rhs: Bitwise) -> Bitwise {
        Bitwise {
            a: self.a & rhs.a,
            b: self.b & rhs.b,
        }
    }
}

fn main() {
    let x = Bitwise { a: 0b1100, b: 0b1010 };
    let y = Bitwise { a: 0b1010, b: 0b0101 };
    let z = x & y;
    println!("{:?}", z); // Output: Bitwise { a: 0b1000, b: 0b0000 }
}
```

### Compound assignment operators

Compound assignment operators are a shorthand notation to perform an operation on a variable and then assign the result back to the same variable. Rust provides several traits for compound assignment operators, including arithmetic and bitwise operations. These traits are defined in the `std::ops` module.

The compound assignment traits have the form [Operation]Assign, where [Operation] is the name of the operation being performed. For example, `AddAssign` is the trait for the `+=` operator.

Here's an overview of the compound assignment traits for arithmetic and bitwise operations:

Arithmetic operators:

- `AddAssign`: +=
- `SubAssign`: -=
- `MulAssign`: \*=
- `DivAssign`: /=
- `RemAssign`: %=

Bitwise operators:

- `BitAndAssign`: &=
- `BitOrAssign`: |=
- `BitXorAssign`: ^=
- `ShlAssign`: <<=
- `ShrAssign`: >>=

To use these traits, you must implement them for your custom types. Here's an example of how to implement the `AddAssign` trait for a custom type:

```rs
use std::ops::AddAssign;

struct MyType(i32);

impl AddAssign for MyType {
    fn add_assign(&mut self, other: MyType) {
        self.0 += other.0;
    }
}

fn main() {
    let mut x = MyType(5);
    x += MyType(10);
    println!("{}", x.0); // prints 15
}
```

### Equivalence Comparisons

In Rust, the equivalence comparison operators are the `==` and `!=` operators. These operators are used to compare two values and return a boolean result indicating whether they are equal or not.

By default, Rust provides the `PartialEq` trait for implementing the equivalence comparison operators. This trait requires implementing the `eq` and `ne` methods.

```rs
struct MyType {
    value: i32,
}

impl PartialEq for MyType {
    fn eq(&self, other: &Self) -> bool {
        self.value == other.value
    }

    fn ne(&self, other: &Self) -> bool {
        self.value != other.value
    }
}

fn main() {
    let x = MyType { value: 5 };
    let y = MyType { value: 5 };
    let z = MyType { value: 10 };

    assert_eq!(x, y);
    assert_ne!(x, z);
}
```

In Rust, floating-point values such as `f32` and `f64` follow the IEEE 754 standard for floating-point arithmetic. According to this standard, certain expressions, such as division by zero or the square root of a negative number, are undefined or produce special values such as NaN (Not-a-Number).

In Rust, a NaN value is represented by a special bit pattern, and it is treated as unequal to every other value, including itself. This behavior is mandated by the IEEE standard, and it is important for ensuring the correctness of numerical algorithms. When working with floating-point values, it is important to keep in mind that NaN values can arise in unexpected ways, and that they require special handling in order to avoid propagating errors or producing incorrect results.

```rs
fn main() {
    let x = 0.0 / 0.0;
    let y = 0.0 / 0.0;

    println!("x = {}, y = {}", x, y);
    println!("x == y: {}", x == y);  // prints "x == y: false"
    println!("x != y: {}", x != y);  // prints "x != y: true"
}
```

### Ordered Comparisons

In Rust, the ordered comparisons (`<`, `<=`, `>`, `>=`) are part of the `PartialOrd` trait, which provides a way to compare values that have a partial ordering, meaning that they can be ordered relative to each other but not necessarily compared for exact equality.

To overload the ordered comparison operators in Rust, a type must implement the `PartialOrd` trait. The `PartialOrd` trait requires a single method, `partial_cmp()`, which returns an `Option<Ordering>`:

```rs
pub trait PartialOrd<Rhs: ?Sized = Self> {
    fn partial_cmp(&self, other: &Rhs) -> Option<Ordering>;
}
```

The `partial_cmp()` method compares the `self` value to the `other` value and returns an `Option<Ordering>` that represents the result of the comparison. The `Option` type is used to handle cases where the comparison cannot be performed, such as when one of the values is NaN.

The `Ordering` type is an enum that has three variants: `Less`, `Equal`, and `Greater`. These variants are used to represent the result of a comparison and can be used to implement the comparison operators:

```rs
pub enum Ordering {
    Less,
    Equal,
    Greater,
}
```

Here is an example that demonstrates how to overload the ordered comparison operators in Rust:

```rs
use std::cmp::Ordering;

#[derive(Debug)]
struct Rational {
    numerator: i32,
    denominator: i32,
}

impl PartialOrd for Rational {
    fn partial_cmp(&self, other: &Rational) -> Option<Ordering> {
        let self_float = (self.numerator as f64) / (self.denominator as f64);
        let other_float = (other.numerator as f64) / (other.denominator as f64);
        self_float.partial_cmp(&other_float)
    }
}

fn main() {
    let r1 = Rational { numerator: 1, denominator: 2 };
    let r2 = Rational { numerator: 2, denominator: 3 };
    let r3 = Rational { numerator: 3, denominator: 4 };

    println!("r1 < r2: {}", r1 < r2);  // prints "r1 < r2: true"
    println!("r2 < r3: {}", r2 < r3);  // prints "r2 < r3: true"
    println!("r3 < r1: {}", r3 < r1);  // prints "r3 < r1: false"
}
```

In this example, we define a `Rational` struct that represents a rational number as a numerator and a denominator. We then implement the `PartialOrd` trait for the `Rational` type by defining the `partial_cmp()` method. In this implementation, we first convert the `self` and `other` values to `f64` floating-point values by dividing the numerator by the denominator. We then use the `partial_cmp()` method of the `f64` type to compare the resulting values.

Finally, we create three `Rational` values and use the overloaded `<` operator to compare them. Because the `Rational` type now implements the `PartialOrd` trait, we can use the `<` operator directly. The output shows that the comparisons produce the expected results, with `r1 < r2` and `r2 < r3` both being `true`, and `r3 < r1` being `false`.

### Index and IndexMut

The `Index` and `IndexMut` traits are used to overload the square bracket indexing syntax (`[]`) to access elements of a custom data structure.

The `Index` trait allows read-only access to elements using the square bracket syntax. It requires implementing the `index` method, which takes a reference to the data structure and an index value, and returns a reference to the element at that index.

Here's an example implementation of `Index` for a custom `Matrix` struct:

```rs
use std::ops::Index;

struct Matrix<T> {
    data: Vec<T>,
    rows: usize,
    cols: usize,
}

impl<T> Index<(usize, usize)> for Matrix<T> {
    type Output = T;

    fn index(&self, (row, col): (usize, usize)) -> &Self::Output {
        &self.data[row * self.cols + col]
    }
}
```

In this example, we implement `Index` for a 2D matrix with `rows` and `cols` fields. The `index` method calculates the index of the element in the underlying `Vec` using row-major order and returns a reference to it.

The `IndexMut` trait extends `Index` to allow mutable access to elements using the same square bracket syntax. It requires implementing the `index_mut` method, which takes a mutable reference to the data structure and an index value, and returns a mutable reference to the element at that index.

Here's an example implementation of `IndexMut` for our `Matrix` struct:

```rs
use std::ops::IndexMut;

impl<T> IndexMut<(usize, usize)> for Matrix<T> {
    fn index_mut(&mut self, (row, col): (usize, usize)) -> &mut Self::Output {
        &mut self.data[row * self.cols + col]
    }
}
```

In this example, we implement `IndexMut` by returning a mutable reference to the element at the calculated index. This allows us to use the square bracket syntax to both read and modify elements of the matrix.

Here's an example usage of `Index` and `IndexMut` on our `Matrix` struct:

```rs
fn main() {
    let mut m = Matrix {
        data: vec![1, 2, 3, 4, 5, 6],
        rows: 2,
        cols: 3,
    };

    assert_eq!(m[(0, 1)], 2);
    m[(1, 2)] = 7;
    assert_eq!(m[(1, 2)], 7);
}
```

In this example, we create a `Matrix` with 2 rows and 3 columns and use the square bracket syntax to read and modify elements. The first assertion checks that the element at row 0, column 1 is equal to 2. The second line modifies the element at row 1, column 2 to be 7. The third assertion checks that the modification was successful.
