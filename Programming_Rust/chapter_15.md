# Iterators

Iterators are a powerful feature in Rust that allow you to loop over collections of data, such as arrays or vectors, without having to write explicit loop constructs. They are a core part of the Rust language and are widely used in many Rust programs.

## What are Iterators?

Iterators are objects that provide a way to traverse through a sequence of values, one at a time. In Rust, iterators are a way to abstract over different collections, so you can write generic code that works with many different data structures. Iterators are implemented as a set of traits, which define a common interface for iterating over collections.

Rust's standard library provides a number of built-in iterators that can traverse various types of collections, as well as custom iterators that can be implemented for specific use cases. Iterators can also be used to generate sequences of values from external sources, such as network connections or other threads. Rust's for loop syntax makes it easy to use iterators, and iterators themselves provide a number of methods for transforming and manipulating the sequence of values they produce.

The most commonly used iterator trait is the Iterator trait. This trait defines a number of methods that allow you to traverse through a collection of values, such as `next()`, which returns the next element in the sequence, or `for_each()`, which applies a closure to each element in the sequence.

Rust's iterators are indeed flexible and efficient.

```rs
fn triangle(n: i32) -> i32 {
    let mut sum = 0;
    for i in 1..=n {
        sum += i;
    }
    sum
}
```

In this example, the `RangeInclusive` iterator produces a sequence of integers from `1` to `n`, inclusive, which are then summed using a `for` loop. This is a concise and readable way to compute the nth triangle number.

One advantage of using an iterator in this case is that it makes the code more composable and reusable. For example, suppose you wanted to compute the sum of the first `n` odd integers instead of the first `n` positive integers. You could easily modify the code to use a different iterator:

```rs
fn sum_of_first_n_odd_numbers(n: i32) -> i32 {
    (1..=n).map(|i| 2*i - 1).sum()
}
```

## The Iterator and IntoIterator Traits

In Rust, an iterator is any value that implements the `Iterator` trait, which is defined in the s`td::iter` module of the standard library. The `Iterator` trait provides a standard way to traverse a sequence of items, and it defines methods for advancing the `iterator`, accessing the current item, and checking whether the iterator has reached the end of the sequence. Any type that implements this trait can be used with Rust's for loop syntax, as well as with a variety of other methods provided by the standard library.

```rs
trait Iterator {
    type Item;
    fn next(&mut self) -> Option<Self::Item>;
    ... // many default methods
}
```

The `Iterator` trait in Rust has an associated type `Item` which represents the type of the values being produced by the iterator. The `next` method is the core method of the `Iterator` trait, which returns an `Option<Self::Item>` representing the next value in the sequence, or `None` if there are no more values. The `Iterator` trait also provides many default methods for transforming and combining iterators, such as `map`, `filter`, `fold`, `zip`, and many others.

If a type can be naturally iterated over, it can implement the `std::iter::IntoIterator` trait, which has an `into_iter` method that takes a value and returns an iterator over it

The `IntoIterator` trait is used to convert a value into an iterator by implementing the `into_iter` method. The trait is defined as follows:

```rs
pub trait IntoIterator where
    Self::IntoIter: Iterator,
{
    type Item;
    type IntoIter: Iterator;

    fn into_iter(self) -> Self::IntoIter;
}

```

The `IntoIterator` trait has two associated types: `Item` and `IntoIter`. `Item` represents the type of the elements that will be produced by the iterator, while `IntoIter` represents the type of the iterator itself.

The `into_iter` method takes ownership of the value and returns an iterator that can be used to iterate over its elements.

For example, `Vec<T>` implements IntoIterator, which means you can call into_iter on a `Vec<T>` to get an iterator that produces the elements of the vector. Similarly, a string slice (`&str`) implements `IntoIterator`, which means you can call `chars` on a string slice to get an iterator that produces its Unicode scalar values.

Implementing `IntoIterator` allows types to be used with Rust's for loop syntax and other iterator-related functionality in the standard library.

```rs
fn main() {
    let v = vec![1, 2, 3];
    for i in v {
        println!("{}", i);
    }
}
```

In this example, `v` is a `Vec<i32>` and is converted into an iterator using the `into_iter` method, which is then used in the for loop to iterate over its elements.

When you write a for loop in Rust, the compiler automatically expands it into calls to the `IntoIterator` and `Iterator` traits. Specifically, the for loop takes the collection or range expression that follows the `for` keyword and calls the `into_iter` method on it to create an iterator. The loop then repeatedly calls the `next` method on the iterator and executes the loop body for each item yielded by `next`.

For example, the following for loop:

```rs
let v = vec!["antimony", "arsenic", "aluminum", "selenium"];
for x in v {
    println!("{}", x);
}
```

Is equivalent to this code using `IntoIterator` and `Iterator` traits:

```rs
let v = vec!["antimony", "arsenic", "aluminum", "selenium"];
let mut iter = v.into_iter();
loop {
    match iter.next() {
        Some(x) => {
            println!("{}", x);
        },
        None => break,
    }
}

let v = vec!["antimony", "arsenic", "aluminum", "selenium"];
let mut iter = v.into_iter();
while let Some(x) = iter.next() {
    println!("{}", x);
}
```

As you can see, the `for` loop is a convenient shorthand for iterating over collections and ranges in Rust.

To summarize the terminology related to iterators:

- Iterator: Any type that implements the `Iterator` trait.
- Iterable: Any type that implements the `IntoIterator` trait, which allows it to produce an iterator by calling `into_iter` method.
- Item: The values produced by an iterator. In the example given, "antimony", "arsenic", and so on are items.
- Consumer: The code that receives and processes the items produced by an iterator. In this case, the for loop acts as the consumer.

The `Iterator` trait specifies that the `next` method should return `None` when there are no more items to produce. If you call `next` again after it has returned `None`, most iterators will continue to return `None`, indicating that there are no more items to produce. However, this behavior is not guaranteed by the trait and some iterators may have different behavior when `next` is called after producing all items. It's up to the implementation of the iterator to decide how to behave in such cases.

## Creating Iterators

There are different ways to create iterators in Rust, including using range syntax, using the Iterator trait's methods, or creating custom iterators.

One way to create an iterator is by using range syntax, which creates a range of values that can be iterated over. The syntax for a range is `start..end` for exclusive ranges (excluding the end value) or `start..=end` for inclusive ranges (including the end value). For example, `1..5` creates an iterator that produces the values 1, 2, 3, and 4, while `1..=5` creates an iterator that produces the values 1, 2, 3, 4, and 5.

Another way to create an iterator is by using the methods provided by the Iterator trait. For example, the `iter` method can be used to create an iterator over a collection such as a vector or an array. The `iter_mut` method can be used to create a mutable iterator over a collection, which allows modifying the elements in the collection through the iterator. The `into_iter` method can be used to create an iterator that consumes a collection and returns ownership of its elements.

Custom iterators can also be created by implementing the Iterator trait. This involves defining the associated types and the `next` method. The associated types specify the type of items produced by the iterator and the type of iterator that the `into_iter` method returns. The `next` method returns an Option containing the next item in the iterator or None if there are no more items. The implementation can also include mutable state that is used to produce the items, such as a counter or a pointer to the current element in a collection.

### iter and iter_mut Methods

The `iter` method creates an iterator over immutable references to the items in a collection, while the `iter_mut` method creates an iterator over mutable references. These methods are available for all collections that implement the `IntoIterator` trait.

```rs
let mut vec = vec![1, 2, 3];

for x in vec.iter() {
    println!("{}", x);
}

for x in vec.iter_mut() {
    *x *= 2;
}

println!("{:?}", vec); // prints [2, 4, 6]
```

The first loop uses `iter` to create an iterator over immutable references to the vector's items. The loop prints each item in the vector.

The second loop uses `iter_mut` to create an iterator over mutable references to the vector's items. The loop multiplies each item in the vector by 2.

Note that in the second loop, we need to dereference `x` using the `*` operator before we can modify the item it refers to. This is because `iter_mut` returns an iterator over references, not the items themselves.

Each type is free to implement `iter` and `iter_mut` in whatever way makes the most sense for its purpose.

The `iter` method on `std::path::Path` returns an iterator that produces one path component at a time. This allows you to easily iterate over the components of a path and perform operations on them individually. Here's an example:

```rs
use std::path::Path;

let path = Path::new("/usr/local/bin");
for component in path.iter() {
    println!("{:?}", component);
}
```

This will print out:

```sh
"usr"
"local"
"bin"
```

Similarly, the `iter_mut` method returns an iterator that produces mutable references to each item in a collection, allowing you to modify the items as you iterate over them.

When a type has multiple common ways to iterate over its values, it usually provides dedicated methods for each method of traversal. This is because a generic `iter` method would be ambiguous. For instance, the `&str` string slice type doesn't have an `iter` method. Instead, calling `s.bytes()` returns an iterator that produces each byte of `s`, while `s.chars()` treats the contents as UTF-8 and generates an iterator of Unicode characters as defined by the Unicode standard. Additionally, `s.lines()` returns an iterator over each line in the string, separated by the newline character '\n'.

### IntoIterator Implementations

Implementing the `IntoIterator` trait for a custom type allows instances of that type to be used in `for` loops and other iterator methods.

Here's an example of implementing `IntoIterator` for a custom `Counter` type:

```rs
struct Counter {
    count: usize,
}

impl Counter {
    fn new() -> Counter {
        Counter { count: 0 }
    }
}

impl Iterator for Counter {
    type Item = usize;

    fn next(&mut self) -> Option<Self::Item> {
        self.count += 1;
        Some(self.count)
    }
}

impl IntoIterator for Counter {
    type Item = usize;
    type IntoIter = Counter;

    fn into_iter(self) -> Self::IntoIter {
        self
    }
}
```

In this example, `Counter` is a custom type that can be used to generate a sequence of numbers. The `Iterator` implementation defines the sequence and the `IntoIterator` implementation allows instances of `Counter` to be used directly in `for` loops and other iterator methods.

The `into_iter` method returns the iterator itself, because `Counter` implements `Iterator`. In other cases, the `into_iter` method might return a different type that implements `Iterator`, such as a `Vec<T>::into_iter()` method that returns an iterator over the elements of the vector.

Most collections provide multiple implementations of `IntoIterator`:

- When given a shared reference to the collection, `into_iter()` returns an iterator that produces shared references to the items. For example, `(&my_vec).into_iter()` would produce an iterator whose item type is `&T`.
- When given a mutable reference to the collection, `into_iter()` returns an iterator that produces mutable references to the items. For example, `(&mut my_vec).into_iter()` would produce an iterator whose item type is `&mut T`.
- When passed the collection by value, `into_iter()` returns an iterator that takes ownership of the collection and produces items by value. The items' ownership moves from the collection to the consumer. For example, `my_vec.into_iter()` would produce an iterator whose item type is `T`. When the iterator is dropped, any elements remaining in the original collection are also dropped, and the collection itself is disposed of.

Some types may only implement one or two of the IntoIterator implementations for various reasons, such as ensuring that their invariants are maintained during iteration. For example, a set-like data structure such as `HashSet` or `BTreeSet` would not implement the mutable reference implementation because modifying an element in the set could potentially violate the set's constraints. Similarly, `HashMap` and `BTreeMap` only provide mutable references to the values, but not the keys, since changing a key could change the position of the element in the map, which could violate the map's ordering or hashing rules.

Rust's design philosophy emphasizes efficiency and predictability. Therefore, types in Rust often provide only those implementations of `IntoIterator` that are efficient and predictable, omitting those that could be expensive or exhibit surprising behavior. For example, some types do not provide implementations of `IntoIterator` on mutable references because modifying elements could violate the type's invariants. Rust's approach helps ensure that iteration is efficient, predictable, and safe.

Slices provide implementations of the `IntoIterator` trait for shared references (`&[T]`) and mutable references (`&mut [T]`). Since slices don't own their elements, they don't provide an implementation for consuming the slice and returning elements by value.

The `into_iter` method called on a slice returns an iterator that produces shared references to the elements if called on a `&[T]`, and mutable references if called on a `&mut [T]`. This is in line with Rust's ownership and borrowing rules, which ensure that mutable references are exclusive and there can be no data races.

The two `IntoIterator` variants for shared and mutable references are equivalent to calling `iter` or `iter_mut` on the referent. The reason Rust provides both is for ergonomics and consistency. Using `iter()` and `iter_mut()` directly on a collection is more concise and easier to read than having to write `into_iter()` with an explicit reference. Additionally, many Rust developers are used to using `iter()` and `iter_mut()` and may not immediately think to use `into_iter()`. Providing both methods allows for consistent and flexible use of iteration across different types and use cases.

IntoIterator can also be useful in generic code:

For example, you can write a function that takes any collection that can be iterated over and performs some operation on each element, without having to know the exact type of the collection. This makes the code more reusable and flexible.

Similarly, by specifying the `Item` type using the `Item=U` syntax, you can further constrain the types that can be iterated over to only those that produce elements of a specific type, which can be useful in certain situations where you need to work with a specific type of element.

```rs
fn print_all_items<T, I>(items: I)
where
    T: std::fmt::Display,
    I: IntoIterator<Item = T>,
{
    for item in items {
        println!("{}", item);
    }
}

fn main() {
    let vec = vec![1, 2, 3];
    print_all_items(vec); // prints 1, 2, 3

    let arr = [4, 5, 6];
    print_all_items(arr); // prints 4, 5, 6

    let set = std::collections::HashSet::<u32>::new();

    print_all_items(set);
}
```

That is correct. The `iter` and `iter_mut` methods are not defined by a common trait, but by convention, many iterable types provide those methods. In contrast, `IntoIterator` is a trait that defines a single method `into_iter`, making it more suitable for use in generic functions. Here's an example:

```rs
fn sum<T>(iterable: T) -> i32
where
    T: IntoIterator<Item = i32>,
{
    iterable.into_iter().sum()
}

fn main() {
    let vec = vec![1, 2, 3];
    sum(vec); // 6
}
```

In this example, the `sum` function takes any iterable type `T` that produces `i32` items, and calculates the sum of all the items using the `sum` method on the iterator returned by `into_iter()`. This allows the function to work with a wide range of iterable types, including arrays, vectors, and ranges.

### from_fn and successors

The `from_fn` and `successors` methods are used to create iterators from functions that generate new values.

The `from_fn` method creates an iterator by repeatedly calling a function that returns an `Option` type until it returns `None`. The iterator produced by `from_fn` will yield each non-`None` value that the function returns. The signature of the `from_fn` method is as follows:

```rs
pub fn from_fn<F>(f: F) -> FromFn<F>
    where F: FnMut() -> Option<Self::Item>
```

Here, `F` is the type of the function that generates the new values. The `from_fn` method returns an `FromFn` iterator that yields values produced by the function.

Here's an example that uses `from_fn` to create an iterator that generates the Fibonacci sequence:

```rs
fn main() {
    let mut fib = (0, 1);

    let mut iter = std::iter::from_fn(move || {
        let next_fib = fib.0 + fib.1;
        fib = (fib.1, next_fib);
        Some(fib.0)
    });

    for _ in 0..10 {
        println!("{:?}", iter.next().unwrap());
    }
}
```

The `successors` method is similar to `from_fn`, but it starts with a initial value and applies a function to each element to generate the next value in the sequence. The iterator produced by `successors` will yield the initial value followed by each successive value generated by the function. The signature of the `successors` method is as follows:

```rs
pub fn successors<T, F>(first: Option<T>, succ: F) -> Successors<T, F>
    where F: FnMut(&T) -> Option<T>
```

Here, `T` is the type of the values generated by the iterator, and `F` is the type of the function that generates the next value in the sequence. The `successors` method returns a `Successors` iterator that yields the initial value followed by each successive value generated by the function.

Here's an example that uses `successors` to create an iterator that generates powers of two:

```rs
fn main() {
    let powers_of_two = std::iter::successors(Some(1), |x| match (*x as i32).checked_mul(2) {
        Some(result) => Some(result),
        None => {
            eprintln!("Error: Overflow occurred when calculating the power of two.");
            None
        }
    });
    for (i, p) in powers_of_two.take(10).enumerate() {
        println!("2^{} = {}", i, p);
    }
}
```

This will print the first 10 powers of two. The `checked_mul` method is used to avoid integer overflow when computing the next power of two.
