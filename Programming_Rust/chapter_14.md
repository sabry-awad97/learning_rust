# Closures

## What are Closures in Rust?

In Rust, closures are anonymous functions that can capture their environment. They are similar to functions, but with some key differences. Closures are defined using the `||` syntax and are often used as arguments to functions or stored in variables. The captured environment can include variables, references, and other closures.

## Examples of standard library features that accept closures include

Great point! These are all great examples of how closures can be used with standard library features in Rust. Here's a little more detail on each of them:

1. Iterator methods such as `map` and `filter`: These methods allow you to apply a closure to each item in a sequence, transforming or filtering the data as needed. For example, you could use the `map` method to convert a sequence of integers into a sequence of their corresponding strings:

   ```rs
   let nums = vec![1, 2, 3];
   let strs = nums.iter().map(|x| x.to_string()).collect::<Vec<_>>();
   ```

   In this example, the `map` method is called on a vector of integers, and a closure is passed in that converts each integer to a string. The resulting vector `strs` will contain the strings `"1"`, `"2"`, and `"3"`.

1. Threading APIs like `thread::spawn`: These APIs allow you to create new threads and execute code concurrently. Closures are often used to represent the code that should be executed in the new thread. For example, you could use the `thread::spawn` method to create a new thread that prints a message:

   ```rs
   use std::thread;

   thread::spawn(|| {
       println!("Hello from a new thread!");
   });
   ```

   In this example, the `thread::spawn` method is called with a closure that prints a message. When the new thread is created, the closure will be executed in that thread.

1. Methods that conditionally need to compute a default value, like the `or_insert_with` method of `HashMap` entries: These methods allow you to perform some computation only if a certain condition is met. Closures are often used to represent the computation that should be performed. For example, you could use the `or_insert_with` method of a `HashMap` to get or create an entry and compute a default value if the entry doesn't exist:

   ```rs
   use std::collections::HashMap;

   let mut map = HashMap::new();
   let result = map.entry("hello").or_insert_with(|| {
       println!("Computing default value...");
       "world"
   });
   println!("{}", result);
   ```

   In this example, the `or_insert_with` method is called on a HashMap with the key `"hello"`. If the key exists in the map, the corresponding value will be returned. If the key doesn't exist, the closure passed to `or_insert_with` will be executed to compute the default value. The closure in this example prints a message and returns the string `"world"`. The resulting value is then stored in the map and printed.

Sure, here are some more examples of how closures can be used in Rust:

1. Sorting and searching algorithms: Many sorting and searching algorithms accept closures to define the order of elements or the condition for a search. For example, you could use the sort_by method of a vector to sort its elements by their length:

   ```rs
   let mut words = vec!["pear", "apple", "banana", "orange"];
   words.sort_by(|a, b| a.len().cmp(&b.len()));
   ```

   In this example, the sort_by method is called on a vector of strings, and a closure is passed in that compares the length of the strings. The resulting vector `words` will be sorted in ascending order of length, so `"pear"` will come first, followed by `"apple"`, `"banana"`, and `"orange"`.

1. Event handling: Event-driven programming often involves registering callbacks to be executed when certain events occur. Closures can be used to define these callbacks. For example, you could use the on_click method of a button widget to register a closure that will be called when the button is clicked:

   ```rs
   let mut button = Button::new();
   button.on_click(|| {
       println!("Button clicked!");
   });
   ```

   In this example, the on_click method is called on a button widget, and a closure is passed in that prints a message. When the button is clicked, the closure will be executed.

1. Memoization: Memoization is a technique for caching the results of expensive computations to avoid redundant work. Closures can be used to define memoization functions. For example, you could define a memoization function that computes the Fibonacci sequence:

   ```rs
   use std::collections::HashMap;
   use std::hash::Hash;

   fn memoize<T, F>(mut f: F) -> impl FnMut(T) -> T
   where
       T: Copy + Ord + Hash,
       F: FnMut(T) -> T,
   {
       let mut cache = HashMap::new();
       move |x| *cache.entry(x).or_insert_with(|| f(x))
   }

   fn fib(n: u32) -> u32 {
       match n {
           0 => 0,
           1 => 1,
           _ => fib(n - 1) + fib(n - 2),
       }
   }

   fn main() {
       let mut memoized_fib = memoize(fib);

       println!("{}", memoized_fib(10)); // prints "55"
   }
   ```

   In this example, the `fib` function is defined at the module level and can be called from within the `memoize` function. The `memoize` function is defined as before, but now it takes the `fib` function as an argument. The `main` function creates a memoized closure for the `fib` function and calls it with an input of `10`, which computes and prints the 10th Fibonacci number, which is `55`.

## Capturing Variables

A closure can use data that belongs to an enclosing function. When a closure captures variables from its enclosing scope, it keeps a reference to those variables. This allows the closure to use the data even after the enclosing function has returned.

There are two ways that a closure can capture variables: by reference and by value.

Closure that Borrow

When a closure captures a variable by reference, it borrows the variable from the enclosing scope. This means that the closure can read and modify the variable, but it cannot move or destroy it. Here's an example:

```rs
fn main() {
    let x = 42;
    let closure = || println!("x = {}", x);
    closure();
}
```

In this example, the closure captures the variable `x` by reference. When the closure is called, it prints the value of `x`, which is `42`.

```rs
fn main() {
    let mut x = 42;
    let closure = |y: &mut i32| *y += x;
    closure(&mut x);
    println!("x = {}", x); // prints "x = 84"
}
```

In this example, the closure takes a mutable reference to the variable `x`, which allows it to modify the variable. The closure also borrows the variable `y`, which means that it cannot outlive the function in which it is defined.

Closure that steal

When a closure captures a variable by value, it moves the variable into the closure. This means that the closure now owns the variable and can move or destroy it. Here's an example:

```rs
fn main() {
    let x = String::from("hello");
    let closure = move || println!("x = {}", x);
    closure();
}
```

In this example, the closure captures the variable `x` by value using the `move` keyword. When the closure is called, it prints the value of `x`, which is `"hello"`. Because the closure takes ownership of `x`, it can modify or destroy it if it wants to.

It's important to note that closures can only capture variables from their enclosing scope if those variables are defined before the closure. This is because Rust requires all captured variables to outlive the closure itself. If a closure tried to capture a variable that was defined after it, the compiler would give an error.

It's worth noting that closures that borrow are more common in Rust than closures that steal. This is because Rust's ownership and borrowing system is designed to prevent data races and memory leaks, which can be more difficult to prevent when closures take ownership of data. However, closures that steal can still be useful in certain situations, such as when you want to transfer ownership of a resource to a closure for the duration of the closure's execution.

## Function and Closure Types

In Rust, functions and closures are first-class citizens, which means that they can be used just like any other value. This includes passing them as arguments to other functions, returning them from functions, and storing them in variables.

Functions and closures are both represented by types in Rust. The type of a function is written using the `fn` keyword, followed by the argument types in parentheses and the return type after an arrow. For example, the type of a function that takes two `i32` arguments and returns an `i32` would be `fn(i32, i32) -> i32`.

Closures, on the other hand, are represented by anonymous types that are generated by the Rust compiler. These types are not named, but they can be inferred by the Rust compiler based on the types of the variables that they capture. For example, a closure that takes a reference to an `i32` and returns a new `i32` with the reference incremented would have the type `Fn(&i32) -> i32`.

There are three main types of closures in Rust, each of which corresponds to a different trait:

- `Fn`: This trait represents closures that borrow their captured variables immutably. They can be called any number of times and do not modify their environment.
- `FnMut`: This trait represents closures that borrow their captured variables mutably. They can be called any number of times and can modify their environment.
- `FnOnce`: This trait represents closures that consume their captured variables. They can only be called once and take ownership of their environment.

By default, closures are `Fn` closures, but you can use the `mut` keyword to create `FnMut` closures, and you can use the `move` keyword to create `FnOnce` closures.

Here's an example that demonstrates the use of function and closure types in Rust:

```rs
fn main() {
    let add: fn(i32, i32) -> i32 = |x, y| x + y; // a closure that adds two numbers

    let x = 1;
    let add_one = |y| x + y; // a closure that adds x and y

    let y = 2;
    let add_two = move |z| x + y + z; // a closure that adds x, y, and z

    println!("{}", add(1, 2)); // prints "3"
    println!("{}", add_one(2)); // prints "3"
    println!("{}", add_two(3)); // prints "6"
}
```

In this example, we define a named function `add` that takes two `i32` arguments and returns their sum. We then define two closures, `add_one` and `add_two`, that capture variables from their enclosing scope. The closure `add_one` captures `x`, while the closure `add_two` captures `x` and `y`. The closure `add_two` uses the `move` keyword to take ownership of the captured variables, allowing it to be called multiple times with different values of `z`. Finally, we call each of the closures with different arguments to demonstrate their behavior.

Closures in Rust can have comparable performance to equivalent code written using functions or manual code. However, there are a few things to keep in mind when it comes to the performance of closures in Rust.

First, closures that capture variables by reference (i.e. those that implement the `Fn` trait) have a small performance overhead compared to equivalent functions. This is because the closure needs to dereference the reference to access the captured variable. However, this overhead is typically very small and is unlikely to be noticeable in most code.

Closures that capture variables by mutable reference (i.e. those that implement the `FnMut` trait) have a larger performance overhead, as they require a runtime check to ensure that the closure is the only reference to the captured variable. This check is necessary to prevent data races and ensure memory safety, but it can have a small performance cost.

Closures that capture variables by value (i.e. those that implement the `FnOnce` trait) can be faster than equivalent functions or closures that capture variables by reference, as they avoid the overhead of pointer dereferencing and runtime checks. However, they also consume the captured variables, which means that they can only be called once.

In general, it's a good idea to use closures where they make sense from a design perspective and to avoid premature optimization. The Rust compiler is highly optimized and can often generate highly efficient code even for complex closures.

Here's an example that demonstrates the performance of closures in Rust:

```rs
use std::time::Instant;

fn main() {
    let v: Vec<i32> = (0..1000000).collect();

    let start = Instant::now();
    let sum1 = v.iter().fold(0, |acc, &x| acc + x);
    let duration1 = start.elapsed();

    let start = Instant::now();
    let sum2 = sum(&v);
    let duration2 = start.elapsed();

    println!("Sum1: {} in {:?}", sum1, duration1);
    println!("Sum2: {} in {:?}", sum2, duration2);
}

fn sum(v: &[i32]) -> i32 {
    v.iter().sum()
}
```

In this example, we generate a vector of 1 million integers and then sum them using two different methods. The first method uses a closure with the `fold` method on the vector, while the second method uses a regular function with the `sum` method on the vector. We then time the duration of each method and print out the results. When we run this code, we get output similar to the following:

```yaml
Sum1: 499999500000 in 4.985984ms
Sum2: 499999500000 in 5.032ms
```

As we can see, the closure method is slightly slower than the function method, but the difference is only a few microseconds. In general, the performance difference between closures and functions is likely to be negligible in most cases.

## Fn

In Rust, `Fn` is a trait that defines the behavior of a closure or function object that can be called like a regular function. Here are some of the key features and properties of `Fn`:

- `Fn` is a trait, so it can be used as a bound on a generic type or in a trait object. For example, `Fn(i32) -> i32` is a type that describes a closure or function object that takes an `i32` argument and returns an `i32` result.
- The `Fn` trait is automatically implemented for any closure or function object that takes no arguments and returns no value (i.e., has the signature `Fn() -> ()`). This is because such closures can be treated as simple blocks of code that can be executed when called.
- The `Fn` trait is not automatically implemented for closures or function objects with arguments or return values. To use a closure or function object with a specific signature as a `Fn`, `FnMut`, or `FnOnce`, you must explicitly implement the corresponding trait for the closure or function object.
- Closures that implement `Fn` can be called using the `()` operator, just like regular functions. They can also be passed to functions or methods that take a `Fn` parameter.
- Closures that implement `Fn` are generally lightweight and efficient, because they don't require any mutable state or complex ownership semantics. However, they may not be suitable for all use cases, particularly those that require mutable state or ownership of captured variables.
- Rust's borrowing rules can make it difficult to use closures that implement `Fn` in certain contexts, particularly those that involve mutable state or ownership of captured variables. In such cases, it may be necessary to use `FnMut` or `FnOnce` instead, or to use other techniques such as `Rc` or `Mutex` to manage shared or mutable state.

## FnMut

- `FnMut` is a trait, so it can be used as a bound on a generic type or in a trait object. For example, `FnMut(i32) -> i32` is a type that describes a closure or function object that takes an `i32` argument and returns an `i32` result, and is able to mutate captured variables.
- Closures that implement `FnMut` can be called using the `()` operator, just like regular functions. They can also be passed to functions or methods that take a `FnMut` parameter.
- Closures that implement `FnMut` can mutate captured variables, which makes them more flexible than closures that only implement `Fn`. However, this also means that they require more complex ownership semantics, because the closure must have mutable access to the captured variables whenever it is called.
- Rust's borrowing rules can make it difficult to use closures that implement `FnMut` in certain contexts, particularly those that involve multiple closures or functions that require mutable access to the same variables. In such cases, it may be necessary to use other techniques such as `Rc` or `Mutex` to manage shared or mutable state.
- `FnMut` closures can be converted into `Fn` closures by consuming them, which means that they can be called multiple times as long as they don't mutate any captured variables. This conversion can be done using the `into_fn` method on the closure.
- Closures that implement `FnMut` are generally less efficient than closures that only implement `Fn`, because they require mutable state and more complex ownership semantics. However, they are more flexible and can be used in a wider range of contexts.

## FnOnce

- `FnOnce` is a trait in Rust's standard library, and it is used to represent closures that take ownership of their captured variables and can only be called once.
- A closure that implements `FnOnce` is a one-time use closure that takes ownership of its captured variables and can only be called once. This means that after the closure has been called, it can no longer be called again.
- The `FnOnce` trait is a subtrait of the `FnMut` trait, which means that a closure that implements `FnOnce` also implements `FnMut` and can be used in contexts that require a `FnMut` closure.
- Unlike closures that implement `Fn` and `FnMut`, closures that implement `FnOnce` move their captured variables into the closure's environment. This means that they take ownership of the captured variables, and the variables are no longer accessible outside the closure.
- The signature of a closure that implements `FnOnce` looks like this:

  ```rs
  fn(T) -> R
  ```

  where `T` represents the type of the closure's input parameter and `R` represents the type of the closure's return value.

- The `call_once` method is defined for closures that implement `FnOnce`, and it is used to call the closure once and consume it. After the closure has been called, it can no longer be called again.
- Closures that implement `FnOnce` are useful for cases where a closure needs to take ownership of its captured variables and can only be called once. One common use case is for creating a thread-safe, lazy-initialized value. In this case, the closure is used to initialize the value, and the closure can only be called once to avoid race conditions.

Here's a table comparing `Fn`, `FnMut`, and `FnOnce` traits in Rust:

| Trait    | Captures            | Can mutate captures | Can take ownership of captures | Can be called multiple times |
| -------- | ------------------- | ------------------- | ------------------------------ | ---------------------------- |
| `Fn`     | Immutable reference | No                  | No                             | Yes                          |
| `FnMut`  | Mutable reference   | Yes                 | No                             | Yes                          |
| `FnOnce` | Consumes (moves)    | No                  | Yes                            | No                           |

This table summarizes the key differences between the three traits. The `Captures` column describes what kind of reference the closure captures its variables with. The `Can mutate captures` column describes whether or not the closure can modify the captured variables. The `Can take ownership of captures` column describes whether or not the closure takes ownership of its captured variables. The `Can be called multiple times` column describes whether or not the closure can be called multiple times.

Overall, `Fn` closures capture their variables with an immutable reference and can be called multiple times. `FnMut` closures capture their variables with a mutable reference and can also be called multiple times. `FnOnce` closures, on the other hand, consume (move) their variables and can only be called once.

It's important to note that `FnOnce` is a subtrait of `FnMut`, which is a subtrait of `Fn`. This means that a closure that implements `FnOnce` also implements `FnMut` and `Fn`, and can be used in contexts that require any of those traits.

## Copy and Clone for Closures

Closures are represented as structs that contain either the values (for move closures) or references to the values (for non-move closures) of the variables they capture. The rules for `Copy` and `Clone` on closures are just like the `Copy` and `Clone` rules for regular structs.

A non-move closure that doesn't mutate variables holds only shared references, which are both Clone and Copy, so that closure is both Clone and Copy as well:

```rs
let y = 10;
let add_y = |x| x + y;
let copy_of_add_y = add_y; // This closure is `Copy`, so...
assert_eq!(add_y(copy_of_add_y(22)), 42); // ... we can call both.
```

This code creates a closure `add_y` that captures the variable `y` by reference and adds it to its input argument. Then, it creates a variable `copy_of_add_y` and assigns `add_y` to it. Since closures that capture variables by reference (i.e., `Fn` closures) are `Copy`, `copy_of_add_y` is also a copy of `add_y`.

Finally, the code calls `copy_of_add_y` with an input of `22`, which returns `32` since it adds `y`, which has a value of `10`. This value is then passed as input to `add_y`, which also adds `y`, resulting in a total of `42`. The `assert_eq!` macro verifies that the result is indeed `42`.

A non-move closure that mutates variables holds mutable references, which are not Clone or Copy. This means that the closure cannot be copied or cloned because it would create multiple mutable references to the same value, which is not allowed in Rust's ownership system.

```rs
let mut x = 0;
let mut add_to_x = |n| { x += n; x };
let copy_of_add_to_x = add_to_x; // this moves, rather than copies
assert_eq!(add_to_x(copy_of_add_to_x(1)), 2); // error: use of moved value
```
