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

## Declarations

In Rust, a declaration is used to introduce a new binding, which is a name associated with a value. There are several types of declarations in Rust, including:

- Variable declaration: A variable declaration is used to create a new binding with a specific name, and assigns a value to it. The value can be changed later on. The syntax for variable declaration is:

  ```rust
  let var_name = value;
  ```

- Constant declaration: A constant declaration is similar to a variable declaration, but the value cannot be changed after the initial assignment. Constants are declared using the `const` keyword instead of `let`. The syntax for constant declaration is:

  ```rust
  const CONST_NAME: type = value;
  ```

- Function declaration: A function declaration is used to create a new binding that refers to a function. The syntax for function declaration is:

  ```rust
  fn function_name(parameters) -> return_type {
      // function body
  }
  ```

- Struct declaration: A struct declaration is used to define a new struct type. Structs are used to group related data together in a single structure. The syntax for struct declaration is:

  ```rust
  struct StructName {
      field1: type1,
      field2: type2,
      ...
  }
  ```

- Enum declaration: An enum declaration is used to define a new enumerated type. Enums are used to define a set of named constants. The syntax for enum declaration is:

  ```rust
  enum EnumName {
      Variant1,
      Variant2,
      ...
  }
  ```

- Trait declaration: A trait declaration is used to define a new trait, which is a collection of method signatures. Traits can be implemented by structs and enums. The syntax for trait declaration is:

  ```rust
  trait TraitName {
      fn method1(&self);
      fn method2(&self) -> ReturnType;
      ...
  }
  ```

- Type alias: A type alias creates a new name for an existing type. The syntax for type alias is:

  ```rust
  type TypeName = ExistingType;
  ```

- Static variable: A static variable is a variable that is stored in the global memory, and its value is shared by all instances of the program. The syntax for static variable is:

  ```rust
  static VAR_NAME: type = value;
  ```

- Unsafe code block : An unsafe block is a block of code that is marked as unsafe and therefore can access memory in ways that are not guaranteed to be safe. The syntax for unsafe code block is:

  ```rust
  unsafe {
      // unsafe code
  }
  ```

- Macros : Macros are a way to extend the syntax of Rust, they are implemented as functions that are expanded at compile-time. There are two types of macros: macro_rules and built-in macros. The syntax for macro is:

  ```rust
  macro_rules! macro_name {
  // macro code
  }
  ```

## if and match

### `if`

`if` expressions are used to conditionally execute code based on a boolean expression. The basic syntax for an `if` expression is:

```rust
if condition {
    // code to execute if condition is true
}
```

Where `condition` is a boolean expression that evaluates to either `true` or `false`.

`if` expressions can also include an `else` branch, which is executed if the condition is `false`. The syntax for an `if` expression with an `else` branch is:

```rust
if condition {
    // code to execute if condition is true
} else {
    // code to execute if condition is false
}
```

It's also possible to chain multiple `if` and `else if` branches together to check multiple conditions. The syntax for chaining `if` and `else if` branches is:

```rust
if condition1 {
    // code to execute if condition1 is true
} else if condition2 {
    // code to execute if condition1 is false and condition2 is true
} else if condition3 {
    // code to execute if condition1 and condition2 are false and condition3 is true
} else {
    // code to execute if all conditions are false
}
```

It's important to note that the conditions in an `if` expression must be a boolean expression, if the condition is not a boolean expression the code will not compile. Also, the `if` expression is an expression, it can be used to return a value or assign it to a variable.

### `match`

`match` expressions are used to compare a value against a pattern and execute code based on the first match. The basic syntax for a `match` expression is:

```rust
match value {
    pattern1 => code_to_execute1,
    pattern2 => code_to_execute2,
    ...
}
```

Where `value` is the value that is being matched against, and `pattern1`, `pattern2`, ... are patterns that are matched against the value. The code block to the right of the pattern is executed if the pattern matches the value.

Each pattern in a `match` expression can also include a guard, which is an additional boolean expression that must be true for the pattern to match. The syntax for a pattern with a guard is:

```rust
pattern if guard => code_to_execute,
```

It's also possible to include a catch-all pattern `_` which matches any value and is executed if no other patterns match. This is useful when we want to handle all possible cases but don't know what they are.

```rust
match value {
    pattern1 => code_to_execute1,
    pattern2 => code_to_execute2,
    ...
    _ => code_to_execute_default
}
```

`match` expressions are often used with enums, as they allow to match on the variant of the enum. It's also commonly used with structs and other types that have a well-defined set of possibilities.

It's important to note that the `match` expression is also an expression, it can be used to return a value or assign it to a variable.

One of the key features of the match expression is that it must be exhaustive, meaning that it must cover all possible values of the type being matched. If the match expression does not cover all possible values, the Rust compiler will raise an error.

To handle the case where none of the patterns match, it's common to use a catch-all pattern \_, which matches any value and is executed if no other patterns match. This is useful when we want to handle all possible cases but don't know what they are.

## if let

`if let` is a shorthand syntax in Rust for an `if` expression that only has a single `match` arm. It is used to match a value and extract the inner value if the match is successful.

The basic syntax for an `if let` expression is:

```rust
if let pattern = value {
    // code to execute if pattern matches value
}
```

The `if let` construct is useful when you want to match a value and extract the inner value, and you only care about one of the possible values. It's a more concise and readable way to write a similar match statement with a single arm.

```rust
if let Some(x) = maybe_value {
    // code to execute if maybe_value is Some(x)
}
```

It's also possible to add an `else` branch to an `if let` expression to execute code when the match fails.

```rust
if let pattern = value {
    // code to execute if pattern matches value
} else {
    // code to execute if pattern does not match value
}
```

## Loops

In Rust, there are several types of loops that can be used to repeat a block of code multiple times.

- `while` loops: A `while` loop repeatedly executes a block of code as long as a certain condition is true. The syntax for a `while` loop is:

  ```rust
  while condition {
      // code to execute
  }
  ```

- `for` loops: A `for` loop is used to iterate over a collection of items, such as an array or a vector. The syntax for a `for` loop is:

  ```rust
  for variable in collection {
      // code to execute
  }
  ```

- `loop`: A `loop` is used to create an infinite loop. The syntax for a `loop` is:

  ```rust
  loop {
      // code to execute
  }
  ```

- `while let`: `while let` is a shorthand syntax in Rust for a `while` loop that only has a single `match` arm. It is used to match a value and extract the inner value on each iteration, as long as the match is successful.

  ```rust
  while let pattern = value {
      // code to execute while pattern matches value
  }

  while let Some(x) = iterator.next() {
      // code to execute while iterator has values
  }
  ```

It's important to note that the `break` and `continue` statements can be used to control the execution of loops. The `break` statement is used to exit a loop, and the `continue` statement is used to skip to the next iteration of a loop.

## Control flow in loops

| Loop Construct   | Description                                                                                        | Reusable Example                                                                                                  | Advantages                                                                                           | Limitations                                                                                                                   |
| ---------------- | -------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `for` Loop       | Used to iterate over a range of values, typically used for counting loops.                         | `for i in 0..10 { println!("The value of i is: {}", i); }`                                                        | Easy to use and well-suited for counting loops                                                       | Limited to counting loops                                                                                                     |
| `while` Loop     | Used to execute a block of code repeatedly as long as a certain condition is true                  | `let mut count = 0; while count < 10 { println!("The value of count is: {}", count); count += 1; }`               | Can be used for any type of loop and is more flexible than the `for` loop                            | Can be more complex to use than the `for` loop and it can be difficult to determine the number of times the loop will execute |
| `loop` Construct | Used to execute an infinite loop and continues to execute until a `break` statement is encountered | `let mut count = 0; loop { println!("The value of count is: {}", count); count += 1; if count == 10 { break; } }` | Most flexible of the three constructs and can be used for any type of loop, including infinite loops | Requires the use of a `break` statement to exit the loop and can be difficult to determine when the loop will end             |

Real-world applications and use cases:

1. Iterating over a Collection

   ```rs
   let numbers = [1, 2, 3, 4, 5];
   for number in numbers.iter() {
       println!("The value of the number is: {}", number);
   }
   ```

1. Processing User Input

   ```rs
   use std::io;

   let mut input = String::new();

   loop {
       println!("Enter a number:");

       input.clear();
       io::stdin().read_line(&mut input).unwrap();

       match input.trim().parse::<i32>() {
           Ok(num) => {
               println!("You entered the number: {}", num);
               break;
           }
           Err(_) => {
               println!("Invalid input, please try again.");
               continue;
           }
       }
   }
   ```

   ```rs
    let mut password = String::new();
    while password != "secret" {
        println!("Enter the password:");
        std::io::stdin().read_line(&mut password).unwrap();
        password = password.trim().to_string();
    }
   ```

## Return expressions

Return expressions are a fundamental concept in Rust programming, as they allow you to return values from a function.

```rs
fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

Return expressions can also be used to return early from a function if certain conditions are met.

```rs
fn largest(a: i32, b: i32) -> i32 {
    if a > b {
        return a;
    }
    b
}
```

## Flow-sensitive analysis

- Flow-sensitive analyses are designed to ensure the correct and safe behavior of your code.

- The analyses perform checks on the flow of control through the program to make sure that every path through a function returns a value of the expected return type, that local variables are never used uninitialized, and that there is no unreachable code.

- These checks help you avoid unexpected results and crashes by catching errors before they occur.

- The Rust compiler also uses these flow-sensitive analyses to optimize your code by removing unreachable code.

- Flow-sensitive analyses are a key part of the Rust programming model, helping you write safe, reliable, and efficient code.

In Rust, the type system is affected by control flow, which means that the way code is executed affects its type. This is why the if expression must have branches with the same type. However, expressions that don't finish normally, such as break or return expressions, infinite loops, or calls to panic! or std::process::exit, are assigned the special type ! and are exempt from the rules about types having to match. The ! type indicates that the function or expression never returns.

The loop expression in Rust is offered as a solution to the issue of normal type matching by offering a way to express the intended flow of control. Writing divergent functions, such as those that never return, is a natural part of the Rust programming model, and can be achieved by using the ! type in function signatures.

In Rust, it is considered an error if a function with the ! type can return normally, as this would be contradictory to its intended behavior.

## Function and Method Calls

Function and method calls are a way to execute code that is stored in a separate function or method, respectively. In Rust, a function call is written using the function name followed by its arguments in parentheses, like this:

```rs
let result = function_name(arg1, arg2, ...); // function call
```

A method call, on the other hand, is called on an object (also known as an instance) of a struct, enum, or trait, and is written using the instance name, followed by the method name and its arguments in parentheses, like this:

```rs
let result = object_instance.method_name(arg1, arg2, ...); // method call
```

In this example, `object_instance` is an instance of a struct or enum, and `method_name` is a method defined for this instance.

Type-associated functions, also known as associated functions, are a special kind of method that are called on the type of the struct, enum, or object rather than on an instance. The syntax for calling associated functions is as follows:

```rs
let result = StructType::associated_function(arg1, arg2, ...);
```

When using generic types in a function or method call, the usual syntax `Vec<T>` does not work, as `<` is the less-than operator. In this case, you should use `::<T>`, which is called the "turbofish". However, if the type parameters can be inferred, it is considered good style to omit them.

```rs
return Vec<i32>::with_capacity(1000); // error: something about chained comparisons
let ramp = (0 .. n).collect<Vec<i32>>(); // same error
```

```rs
return Vec::<i32>::with_capacity(1000); // ok, using ::<T>
let ramp = (0 .. n).collect::<Vec<i32>>(); // ok, using ::<T>
```

This syntax is necessary because otherwise the angle brackets would be parsed as the bitwise left shift operator.

```rs
return Vec::with_capacity(10); // ok, if the fn return type is Vec<i32>
let ramp: Vec<i32> = (0 .. n).collect(); // ok, variable's type is given
```

## Fields and Elements

One of its key features is the ability to access fields and elements in various data structures such as structs, tuples, arrays, slices, and vectors.

### Struct Fields

Structs are composite data types that allow you to group multiple values together into a single object.
A struct is a composite type made up of smaller parts called fields. Each field has a name and a type.

```rs
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}

let user1 = User {
    email: String::from("someone@example.com"),
    username: String::from("someusername123"),
    active: true,
    sign_in_count: 1,
};
```

To access the fields of a struct, you use the dot operator (.) followed by the field name:

```rs
user1.email
```

If the value to the left of the dot operator is a reference or smart pointer type, it will be automatically dereferenced, just as for method calls. This makes it easy to work with complex data structures without having to worry about memory management.

### Tuple Elements

Tuples are similar to structs, but their fields have numbers rather than names. To access the elements of a tuple, you use the dot operator (.) followed by the element number:

```rs
coords.1
```

### Array Elements

Arrays are fixed-sized data structures that allow you to store multiple values of the same type. To access the elements of an array, you use square brackets ([ ]) followed by the element number:

```rs
pieces[i]
```

The value to the left of the square brackets will be automatically dereferenced. Expressions like these are known as lvalues, because they can appear on the left side of an assignment. For example:

```rs
pieces[2] = Some(Piece::new(Black, Knight, coords));
```

Note that to make an assignment, the array must be declared as a mutable variable.

### Slices

Slices are views into arrays or vectors that allow you to reference a contiguous subset of the data. To extract a slice from an array or vector, you use the range operator (..) followed by the start and end indices:

```rs
let second_half = &game_moves[midpoint .. end];
```

The range operator can also be used to produce various types of ranges, including end-exclusive (half-open) ranges, end-inclusive (closed) ranges, and full ranges. Only ranges that include a start value are iterable, as a loop must have somewhere to start.

## Reference Operators

There are two reference operators: & and &mut. These operators allow you to work with the memory address of a value rather than the value itself. The & operator creates an immutable reference to a value, while the &mut operator creates a mutable reference.

In addition to these reference operators, there is also a unary \* operator. This operator is used to access the value pointed to by a reference. When you use the . operator to access a field or method of a reference, Rust automatically dereferences the reference for you. However, in some cases you may need to explicitly access the value pointed to by a reference. This is where the \* operator comes in. By using the \* operator, you can read or write the entire value that the reference points to.

## Type Casts

There are two kinds of type casts: explicit type casts and type coercion.

Explicit type casts, also known as "type conversions", are done using the as keyword. These allow you to convert a value from one type to another.

```rs
let x: u32 = 42;
let y: i32 = x as i32;
```

In this example, the value `x` of type `u32` is explicitly cast to type `i32` using the `as` keyword, and assigned to the variable `y`.

Type coercion, on the other hand, is an implicit conversion of a value's type to a different type. This happens automatically in certain situations, such as when you pass an argument to a function that expects a different type.

```rs
fn add_one(x: i32) -> i32 {
    x + 1
}

let y: u32 = 42;
let z = add_one(y as i32);
```

In Rust, casting between two types is only safe if the two types have the same **size** and **alignment**.

Size refers to the number of bytes that the type occupies in memory. For example, `i32` and `u32` both have a size of 4 bytes, which means they have the same number of bytes and are considered to be the same size.

Alignment refers to the memory address at which a value of a particular type must be stored. For example, an `i32` value must be stored at an address that is a multiple of 4 bytes (i.e., its alignment is 4 bytes). If two types have different alignment requirements, casting between them can result in undefined behavior.

When two types have the same size and alignment, Rust will ensure that the cast is safe and valid at compile time. However, if the two types have different size or alignment, the cast can result in undefined behavior or memory safety issues. In these cases, it is generally recommended to use other methods such as transmutation, conversion or conversion traits like From and Into to convert between types safely.

Several kinds of casts are permitted, including:

1. Numeric casts: These allow you to convert a value from one numeric type to another. For example, you might cast an `i32` to a `u32`, or a `f32` to an `f64`.

   ```rs
   let x: i32 = 42;
   let y: u32 = x as u32;
   ```

1. Pointer casts: These allow you to convert a pointer from one type to another. For example, you might cast a raw pointer to a typed pointer, or a mutable reference to an immutable reference.

   ```rs
   let x: *mut i32 = &mut 42;
   let y: *const i32 = x as *const i32;
   ```

1. Reference casts: These allow you to convert a reference from one type to another. For example, you might cast an `&i32` to an `&u32`.

   ```rs
   let x: &i32 = &42;
   let y: &u32 = unsafe { &*(x as *const i32 as *const u32) };
   ```

1. Trait object casts: These allow you to convert a trait object from one type to another. For example, you might cast a `&dyn Any` trait object to a `&dyn Debug` trait object.

   ```rs
   use std::any::Any;

   let x: &dyn Any = &42;
   let y: &dyn std::fmt::Debug = x as &dyn std::fmt::Debug;
   ```

There are several rules that should be followed when using casts to ensure that the code is correct and safe:

1. Only use casts when necessary: In general, you should avoid casts whenever possible and instead use Rust's type system to ensure that your code is correct and safe. Only use casts when there is no other way to achieve the desired behavior.

1. Only use safe casts: Rust provides several safe casts that are guaranteed to be correct and safe, such as numeric casts and pointer casts. Make sure that you only use these safe casts and avoid using unsafe casts.

1. Check for overflow and underflow: Numeric casts can result in overflow or underflow if the value being casted is outside the range of the target type. Make sure to check for overflow and underflow when using numeric casts, or use the `checked_` methods provided by Rust to perform the cast safely.

1. Avoid casting between unrelated types: Casting between unrelated types can result in undefined behavior and should be avoided whenever possible. Only cast between types that are related in some way, such as numeric types or pointer types.

1. Use explicit casts when necessary: When performing a cast, make sure to use an explicit cast (using the `as` keyword) instead of relying on type coercion. This makes your code clearer and easier to understand.

1. Use unsafe code with caution: Some casts can only be performed using unsafe code. When using unsafe code, make sure that you understand the risks involved and use it carefully to ensure that your code is correct and safe.

Here are some examples of following the rules for casting in Rust:

1. Casting pointers to integers:

   ```rs
   let x: *mut i32 = &mut 42;
   let y: usize = x as usize;
   ```

   In this example, we are casting a mutable pointer `x` to an integer `y`. This is safe to do as long as the resulting integer is not used to perform any arithmetic or logical operations, as pointer arithmetic requires a pointer type.

1. Avoiding invalid pointer casts:

   ```rs
   let x: *mut i32 = &mut 42;
   let y: *mut u32 = x as *mut u32;
   ```

   In this example, we are attempting to cast a mutable pointer `x` to a mutable pointer `y` of a different type. This is only safe to do if the two types have the same size and alignment, otherwise it can result in undefined behavior.

1. Casting between references:

   ```rs
   let x: &i32 = &42;
   let y: &u32 = unsafe { &*(x as *const i32 as *const u32) };
   ```

   In this example, we are casting a reference `x` to a reference `y` by first casting it to a raw pointer and then dereferencing it as an unsigned integer reference. This is safe to do as long as the resulting reference is not used to modify the original value, as this can result in undefined behavior.

1. Using `transmute` to cast between types:

   ```rs
   use std::mem::transmute;

   let x: i32 = 42;
   let y: u32 = unsafe { transmute(x) };
   ```

   In this example, we are using the `transmute` function from the mem module to cast a signed integer `x` to an unsigned integer `y`. This is only safe to do if the two types have the same size and alignment, and if the resulting value is valid for the target type.

Rules for casting numbers:

1. Only cast between compatible types: Rust provides several numeric types, such as i32, u32, f32, and f64. Make sure that you only cast between compatible types, such as i32 to u32, or f32 to f64. Casting between incompatible types, such as from a floating-point type to an integer type, can result in undefined behavior.

   ```rs
   let x: i32 = 42;
   let y: u32 = x as u32;
   ```

   In this example, we are casting a signed integer `x` to an unsigned integer `y`, which are compatible types.

1. Check for overflow and underflow: Numeric casts can result in overflow or underflow if the value being casted is outside the range of the target type. Make sure to check for overflow and underflow when using numeric casts, or use the `checked_` methods provided by Rust to perform the cast safely.

   ```rs
   let x: i32 = 2_147_483_647;
   let y: u32 = x.checked_add(1).unwrap_or(u32::MAX);
   ```

   In this example, we are checking for overflow by adding 1 to the maximum value of a signed integer `x`, and using `checked_add` method to return `None` if overflow occurred. We then use `unwrap_or` method to set the result to the maximum value of an unsigned integer `y` in case of overflow.

1. Use explicit casts: When performing a cast, make sure to use an explicit cast (using the as keyword) instead of relying on type coercion. This makes your code clearer and easier to understand.

   ```rs
   let x: i32 = 42;
   let y: u32 = x as u32;
   ```

   In this example, we are using an explicit cast with the as keyword to cast the signed integer `x` to an unsigned integer `y`.

1. Avoid narrowing conversions: Casting from a wider type to a narrower type can result in data loss if the value being casted is too large to fit in the target type. Make sure to avoid narrowing conversions whenever possible, or use the `checked_` methods provided by Rust to perform the cast safely.

   ```rs
   let x: u64 = 2_147_483_647;
   let y: u32 = x.try_into().unwrap_or(u32::MAX);
   ```

   In this example, we are avoiding a narrowing conversion by using the `try_into` method to attempt to cast the unsigned integer `x` to a smaller unsigned integer `y`. If the value of `x` is too large to fit in `y`, the method returns `Err`, and we use `unwrap_or` to set the result to the maximum value of `y`.

1. Be aware of sign extension: When casting from a signed type to an unsigned type, the sign bit is automatically extended to fill the extra bits in the target type. Make sure to be aware of sign extension when performing these kinds of casts, as it can result in unexpected behavior.

   ```rs
   let x: i32 = -42;
   let y: u32 = x as u32;
   ```

   In this example, we are casting a negative signed integer `x` to an unsigned integer `y`. Since `y` is wider than `x`, the sign bit of `x` is automatically extended to fill the extra bits in `y`, resulting in a large positive value for `y`.

1. Use saturating casts when appropriate: Rust provides saturating casts that will truncate the value being casted to fit within the target type, rather than causing an overflow or underflow. Use saturating casts when appropriate to ensure that your code is correct and safe.

   ```rs
   let x: u8 = 255;
   let y: i8 = x as i8;
   let z: i8 = x.try_into().unwrap_or_else(|_| i8::MAX);
   ```

   In this example, we are using saturating casts to ensure that the value being casted fits within the target type. In the first line, we are casting the maximum value of an unsigned 8-bit integer `x` to a signed 8-bit integer `y`, resulting in an overflow that sets `y` to the minimum value of `i8`. In the second line, we are using `try_into` to perform a saturating cast that sets the result to the maximum value of `i8`.

- In Rust, some conversions involving reference types can be performed without a cast thanks to deref coercions.
- Deref coercion applies to types that implement the Deref built-in trait.
- Deref coercion allows smart pointer types to behave as much like the underlying value as possible.
- Examples of automatic conversions that involve reference types implementing the Deref trait include:
  - Converting a mutable reference to a non-mutable reference.
  - Converting a `&String` value to a `&str` value.
  - Converting a `&Vec<i32>` value to a `&[i32]` value.
  - Converting a `&Box<Chessboard>` value to a `&Chessboard` value.
- User-defined types can implement the Deref trait to enable deref coercion for their own smart pointer types.

  ```rs
  use std::ops::Deref;

  struct MyString(String);

  impl Deref for MyString {
      type Target = String;

      fn deref(&self) -> &Self::Target {
          &self.0
      }
  }

  fn print_string(s: &str) {
      println!("{}", s);
  }

  fn main() {
      let my_string = MyString("hello".to_string());

      // The MyString type implements Deref, so we can pass a &MyString to a function
      // that takes a &str. The compiler will automatically apply deref coercion to
      // convert the &MyString to a &String, which can then be converted to a &str.
      print_string(&my_string);
  }
  ```

Here's an example of how deref coercion can be used to convert a &String value to a &str value:

```rs
fn print_string(s: &str) {
    println!("{}", s);
}

fn main() {
    let my_string = String::from("hello");

    // We can pass a &String to a function that takes a &str thanks to deref coercion.
    // The &String value will be automatically converted to a &str.
    print_string(&my_string);
}
```

## Rust Closures

- Closures are anonymous functions that can be used to capture values from their surrounding environment and then be called like regular functions. They can be defined using the `|...| ...` syntax, where the `|...|` part specifies the closure's parameters and the `...` part defines the code to be executed.

- Closures can capture variables from their surrounding environment and store them for later use. This is called "capturing variables by reference" or "capturing variables by value".

```rs
fn main() {
    let x = 5;
    let square = |y: i32| y * x; // Captures x by reference
    println!("The square of {} is {}", x, square(x));
}
```

- Closures are implemented using traits in Rust, which means that they are fully type-safe and can be used as function arguments or return values.

```rs

// Closures can implement traits, like Fn, FnMut, and FnOnce.
fn repeat<F>(f: F, n: u32) where F: Fn() {
    for i in 0..n {
        f();
    }
}

fn main() {
    let closure = || println!("Hello, world!");
    repeat(closure, 3);
}
```

- Closures can be defined in a variety of different ways, including using the `move` keyword to explicitly capture variables by value.

```rs
fn main() {
    let x = vec![1, 2, 3];
    let print_x = move || println!("{:?}", x); // Captures x by value (the closure takes the ownership)
    print_x();
}
```

- Rust's closure syntax supports a shorthand notation that allows you to omit the parameter types and return type if they can be inferred by the compiler.

```rs
fn main() {
    let x = 5;
    let y = 7;
    let add = |a, b| a + b; // No parameter or return type annotations needed
    println!("{} + {} = {}", x, y, add(x, y));
}
```

- Closures can be used in a variety of contexts, including iterators, event handling, and concurrency.

```rs
fn main() {
    let nums = vec![1, 2, 3, 4, 5];
    let squared: Vec<_> = nums.iter().map(|x| x * x).collect(); // Use closure with map
    let evens = numbers.iter().filter(|x| x % 2 == 0).collect::<Vec<_>>();
    println!("{:?}", evens);
}
```

```rs
use std::thread;
fn main() {
    let handle = thread::spawn(|| {
        // Task to run in a new thread
    });

    // Wait for the thread to finish
    let result = handle.join().unwrap();
}
```
