# Expressions

In Rust, expressions are the building blocks that make up the body of Rust functions and the majority of Rust code. Expressions are used for control flow and are the foundation of Rust's operators, which can be used in isolation or in combination.

Rust is considered an expression-based language, which is a tradition that dates back to Lisp. In expression-based languages like Rust, expressions are used to perform all the work, including control flow and computation. This is in contrast to statement-based languages, where statements are used to perform specific actions, and expressions are used to evaluate values.

The use of expressions in Rust provides a more concise and expressive way of expressing logic and computation. This can make the code more readable and maintainable, as well as more powerful, as expressions can be used to perform complex operations. Additionally, the use of expressions in Rust also makes the language more type-safe, as the type of an expression is known at compile-time, which can help prevent runtime errors.

However, the use of expressions in Rust also has some limitations. For example, the use of expressions can make code more difficult to read and understand, especially when dealing with complex logic. Additionally, since expressions in Rust are type-safe, it can make it more difficult to perform certain operations, such as working with different types of data.

The expressions in Rust include:

| Expression Type          | Example                                 | Related Traits          |
| ------------------------ | --------------------------------------- | ----------------------- |
| Array literal            | \[1, 2, 3\]                             |                         |
| Repeat array literal     | \[0; 50\]                               |                         |
| Tuple                    | (6, "crullers")                         |                         |
| Grouping                 | (2 + 2)                                 |                         |
| Block                    | { f(); g() }                            |                         |
| Control flow expressions | if ok { f() }                           |                         |
|                          | if ok { 1 } else { 0 }                  |                         |
|                          | if let Some(x) = f() { x } else { 0 }   |                         |
|                          | match x { None => 0, \_ => 1 }          |                         |
|                          | for v in e { f(v); }                    | std::iter::IntoIterator |
|                          | while ok { ok = f(); }                  |                         |
|                          | while let Some(x) = it.next() { f(x); } |                         |
|                          | loop { next_event(); }                  |                         |
|                          | break                                   |                         |
|                          | continue                                |                         |
|                          | return 0                                |                         |
| Macro invocation         | println!("ok")                          |                         |
| Path                     | std::f64::consts::PI                    |                         |
| Struct literal           | Point {x: 0, y: 0}                      |                         |
| Tuple field access       | pair.0                                  | Deref, DerefMut         |
| Struct field access      | point.x                                 | Deref, DerefMut         |
| Method call              | point.translate(50, 50)                 | Deref, DerefMut         |
| Function call            | stdin()                                 |                         |
| Index                    | arr\[0\]                                |                         |
| Error check              | create_dir("tmp")?                      |                         |
| Logical/bitwise NOT      | !ok                                     | Not                     |
| Negation                 | -num                                    | Neg                     |
| Dereference              | \*ptr                                   | Deref, DerefMut         |
| Borrow                   | &val                                    |                         |
| Type cast                | x as u32                                |                         |
| Multiplication           | n \* 2                                  | Mul                     |
| Division                 | n / 2                                   | Div                     |
| Remainder                | n % 2                                   | Rem                     |
| Addition                 | n + 1                                   | Add                     |
| Subtraction              | n - 1                                   | Sub                     |
| Left shift               | n << 1                                  | Shl                     |
| Right shift              | n >> 1                                  | Shr                     |
| Bitwise AND              | n & 1                                   | BitAnd                  |
| Bitwise exclusive OR     | n ^ 1                                   | BitXor                  |
| Bitwise OR               | n                                       | 1                       |
| Less than                | n < 1                                   | std::cmp::PartialOrd    |
| Less than or equal       | n <= 1                                  | std::cmp::PartialOrd    |
| Greater than             | n > 1                                   | std::cmp::PartialOrd    |
| Greater than or equal    | n >= 1                                  | std::cmp::PartialOrd    |
| Equal                    | n == 1                                  | std::cmp::PartialEq     |
| Not equal                | n != 1                                  | std::cmp::PartialEq     |
| Logical AND              | x.ok && y.ok                            |                         |
| Logical OR               | x.ok                                    |                         |
| End-exclusive range      | start .. stop                           |                         |
| End-inclusive range      | start ..= stop                          |                         |
| Assignment               | x = val                                 |                         |
| Compound assignment      | x \*= 1                                 | MulAssign               |
|                          | x /= 1                                  | DivAssign               |
|                          | x %= 1                                  | RemAssign               |
|                          | x += 1                                  | AddAssign               |
|                          | x -= 1                                  | SubAssign               |
| Closure                  | \|x, y\| x + y                          |                         |

These expressions represent most of the operations that can be performed in Rust, and are used to construct complex logic and computation in the language.

## Precedence and Associativity

Precedence and associativity are the rules that determine the order in which operations are executed in an expression.

Precedence refers to the order in which operations are performed when multiple operations are combined in a single expression. For example, in the expression "2 + 3 - 4", the multiplication operation (\*) has a higher precedence than the addition operation (+), so the multiplication is performed first, resulting in the value 12.

Associativity refers to the order in which operations are performed when multiple operations of the same precedence are combined in a single expression. For example, in the expression "2 - 3 - 4", subtraction is left-associative, so the operations are performed from left to right, resulting in the value -5.

## Blocks and Semicolons

In Rust, blocks are used to group multiple expressions together and can be used as expressions themselves. Blocks are enclosed in curly braces { } and can contain any number of statements or expressions. They are commonly used in control flow statements like if, while, and for, as well as in function and method definitions.

A semicolon (;) is used to separate statements in Rust. Each statement must end with a semicolon, except for when a block is used as the last element in a statement. In this case, the semicolon is optional. For example:

```rust
let x = {
    let y = 1;
    y + 1
}; //This is a valid statement, semicolon is optional here

let msg = {
    // let-declaration: semicolon is always required
    let dandelion_control = puffball.open();
    // expression + semicolon: method is called, return value dropped
    dandelion_control.release_all_seeds(launch_codes);
    // expression with no semicolon: method is called,
    // return value stored in `msg`
    dandelion_control.get_status()
};
```

However, in most cases, it's recommended to use a semicolon to separate statements, as it makes the code more clear and consistent.

It's also important to note that semicolons can also be used to separate expressions in Rust, but it is not recommended to use them to do so, as it can make the code more difficult to read and understand, especially when dealing with complex logic.

In summary, blocks are used in Rust to group multiple statements together, and semicolons are used to separate statements. While semicolons can be used to separate expressions as well, it's not recommended as it can make the code more difficult to read.
