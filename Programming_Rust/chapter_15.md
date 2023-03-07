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

### drain Methods

The `drain` method is another way to consume a collection while modifying it. It removes and returns a range of elements specified by a `Range` or `RangeInclusive`, while also modifying the original collection in place. The `drain` method returns an iterator that produces the removed elements, and the original collection is left with the remaining elements after the range is removed.

The `drain` method is defined for many standard collections, including `Vec`, `LinkedList`, and `HashMap`. Here is an example of using the `drain` method on a `Vec`:

```rs
let mut numbers = vec![1, 2, 3, 4, 5];
let removed_numbers: Vec<_> = numbers.drain(1..4).collect();

assert_eq!(numbers, vec![1, 5]);
assert_eq!(removed_numbers, vec![2, 3, 4]);
```

In this example, the `drain` method is called on `numbers` with the range `1..4`, which removes the second through fourth elements of the vector (`[2, 3, 4]`) and returns them as an iterator. The `collect` method is then called on this iterator to collect the removed elements into a new vector called `removed_numbers`. The resulting `numbers` vector contains the remaining elements (`[1, 5]`).

Note that the `drain` method modifies the original collection in place, so it cannot be used on a shared reference (`&mut`).

### Other Iterator Sources

| Type or trait                                         | Expression                        | Notes                                                                                                                               |
| ----------------------------------------------------- | --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| std::ops::Range                                       | `1..10`                           | Endpoints must be an integer type to be iterable. Range includes start value and excludes end value.                                |
| std::ops::RangeFrom                                   | `1..`                             | Unbounded iteration. Start must be an integer. May panic or overflow if the value reaches the limit of the type.                    |
| std::ops::RangeInclusive                              | `1..=10`                          | Like Range, but includes end value.                                                                                                 |
| Option&lt;T&gt;                                       | `Some(10).iter()`                 | Behaves like a vector whose length is either 0 (None) or 1 (Some(v)).                                                               |
| Result&lt;T, E&gt;                                    | `Ok("blah").iter()`               | Similar to Option, producing Ok values.                                                                                             |
| Vec&lt;T&gt;, &\[T\]                                  | `v.windows(16)`                   | Produces every contiguous slice of the given length, from left to right. The windows overlap.                                       |
|                                                       | `v.chunks(16)`                    | Produces nonoverlapping, contiguous slices of the given length, from left to right.                                                 |
|                                                       | `v.chunks_mut(1024)`              | Like chunks, but slices are mutable.                                                                                                |
|                                                       | `v.split(\|byte\| byte & 1 != 0)` | Produces slices separated by elements that match the given predicate.                                                               |
|                                                       | `v.split_mut(...)`                | As above, but produces mutable slices.                                                                                              |
|                                                       | `v.rsplit(...)`                   | Like split, but produces slices from right to left.                                                                                 |
|                                                       | `v.splitn(n, ...)`                | Like split, but produces at most n slices.                                                                                          |
| String, &str                                          | `s.bytes()`                       | Produces the bytes of the UTF-8 form.                                                                                               |
|                                                       | `s.chars()`                       | Produces the chars the UTF-8 represents.                                                                                            |
|                                                       | `s.split_whitespace()`            | Splits string by whitespace, and produces slices of nonspace characters.                                                            |
|                                                       | `s.lines()`                       | Produces slices of the lines of the string.                                                                                         |
|                                                       | `s.split('/')`                    | Splits string on a given pattern, producing the slices between matches. Patterns can be many things: characters, strings, closures. |
|                                                       | `s.matches(char::is_numeric)`     | Produces slices matching the given pattern.                                                                                         |
| std::collections::HashMap, std::collections::BTreeMap | `map.keys()`                      | Produces shared references to keys of the map.                                                                                      |
|                                                       | `map.values()`                    | Produces shared references to values of the map.                                                                                    |
|                                                       | `map.values_mut()`                | Produces mutable references to entries’ values.                                                                                     |
| std::collections::HashSet, std::collections::BTreeSet | `set1.union(set2)`                | Produces shared references to elements of union of set1 and set2.                                                                   |
|                                                       | `set1.intersection(set2)`         | Produces shared references to elements of intersection of set1 and set2.                                                            |
| std::sync::mpsc::Receiver                             | `recv.iter()`                     | Produces values sent from another thread on the corresponding Sender.                                                               |
| std::io::Read                                         | `stream.bytes()`                  | Produces bytes from an I/O stream.                                                                                                  |
|                                                       | `stream.chars()`                  | Parses stream as UTF-8 and produces chars.                                                                                          |
| std::io::BufRead                                      | `bufstream.lines()`               | Parses stream as UTF-8, produces lines as Strings.                                                                                  |
|                                                       | `bufstream.split(0)`              | Splits stream on given byte, produces inter-byte Vec&lt;u8&gt; buffers.                                                             |
| std::fs::ReadDir                                      | `std::fs::read_dir(path)`         | Produces directory entries.                                                                                                         |
| std::net::TcpListener                                 | `listener.incoming()`             | Produces incoming network connections.                                                                                              |
| Free functions                                        | `std::iter::empty()`              | Returns None immediately. Useful for cases where an iterator is expected but there is no data to be returned.                       |
|                                                       | `std::iter::once(5)`              | Produces the given value and then ends.                                                                                             |
|                                                       | `std::iter::repeat("#9")`         | Produces the given value forever.                                                                                                   |

```rs
use std::collections::{HashMap, HashSet};
use std::io::{BufRead, Read};
use std::net::TcpListener;

// std::ops::Range
for i in 1..10 {
    println!("{}", i);
}

// std::ops::RangeFrom
for i in (1..).take(10) {
    println!("{}", i);
}

// std::ops::RangeInclusive
for i in 1..=10 {
    println!("{}", i);
}

// Option<T>
let maybe_number = Some(42);
for number in maybe_number {
    println!("{}", number);
}

// Result<T, E>
let result = Result::<&str, &str>::Ok("hello");
for value in result {
    println!("{}", value);
}

// Vec<T>, &[T]
let numbers = vec![1, 2, 3, 4, 5, 6];
for window in numbers.windows(3) {
    println!("{:?}", window);
}
for chunk in numbers.chunks(3) {
    println!("{:?}", chunk);
}
for byte in b"hello, world".split(|byte| *byte == b',') {
    println!("{:?}", byte);
}

// String, &str
let text = "hello, world";
for byte in text.bytes() {
    println!("{}", byte);
}
for char in text.chars() {
    println!("{}", char);
}
for word in text.split_whitespace() {
    println!("{}", word);
}
for line in text.lines() {
    println!("{}", line);
}
for part in text.split(',') {
    println!("{}", part);
}

// std::collections::HashMap, std::collections::BTreeMap
let mut map = HashMap::new();
map.insert("one", 1);
map.insert("two", 2);
for key in map.keys() {
    println!("{}", key);
}
for value in map.values() {
    println!("{}", value);
}
for value in map.values_mut() {
    *value += 1;
}

// std::collections::HashSet, std::collections::BTreeSet
let set1: HashSet<i32> = [1, 2, 3].iter().cloned().collect();
let set2: HashSet<i32> = [3, 4, 5].iter().cloned().collect();
for element in set1.union(&set2) {
    println!("{}", element);
}
for element in set1.intersection(&set2) {
    println!("{}", element);
}

// std::sync::mpsc::Receiver
let (sender, receiver) = std::sync::mpsc::channel();
std::thread::spawn(move || {
    sender.send(42).unwrap();
});
for message in receiver.iter() {
    println!("{}", message);
}

// std::io::Read
let mut buffer = [0; 1024];
let mut stream = std::io::Cursor::new(b"hello, world");
while let Ok(n) = stream.read(&mut buffer) {
    if n == 0 {
        break;
    }
    println!("{:?}", &buffer[..n]);
}

// std::io::BufRead
let mut bufstream = std::io::Cursor::new("hello\nworld\n");
for line in bufstream.lines() {
    println!("{}", line.unwrap());
}
bufstream.set_position(0);
for byte in bufstream.split(b'\n') {
    println!("{:?}", byte.unwrap());
}

// std::fs::ReadDir
let mut dir = std::fs::read_dir(".").unwrap();
while let Some(entry) = dir.next() {
    if let Ok(entry) = entry {
        println!("{:?}", entry.path());
    }
}

fn is_empty<T: Iterator>(mut iter: T) -> bool {
    iter.next().is_none()  // Returns true if iter produces no values.
}

// std::iter::empty
assert!(is_empty(std::iter::empty::<u32>()));
assert!(!is_empty(std::iter::once(1)));

// std::iter::once
let mut twos = std::iter::once(2);  // An iterator that produces a single 2.

// std::iter::repeat
let mut ones = std::iter::repeat(1);  // An infinite sequence of 1s.
let mut zeroes = std::iter::repeat_with(|| 0);  // An infinite sequence of 0s.
```

## Iterator adapters

Iterator adapters are methods that take an iterator and return a new iterator with a modified behavior.
Here are some common iterator adapters in Rust:

| Method              | Description                                                                                                              |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| `map(f)`            | Transforms each value of an iterator by applying function f to it, returning a new iterator.                             |
| `filter(p)`         | Produces a new iterator yielding only the elements of the original iterator that satisfy p.                              |
| `enumerate()`       | Yields tuples containing an index and the value of the original iterator.                                                |
| `skip(n)`           | Skips the first n elements of the iterator, producing the remaining values.                                              |
| `take(n)`           | Produces an iterator of the first n elements of the original iterator.                                                   |
| `skip_while(p)`     | Skips elements of the iterator while the predicate p is true, then produces the remaining values.                        |
| `take_while(p)`     | Takes elements of the iterator while the predicate p is true, then stops producing values.                               |
| `peekable()`        | Provides a way to look at the next element of an iterator without consuming it.                                          |
| `fuse()`            | Wraps an iterator, ensuring that it terminates correctly even if the underlying iterator panics.                         |
| `chain(iter)`       | Chains two iterators together, producing a new iterator that first produces values from the first, then from the second. |
| `zip(iter)`         | Pairs up two iterators, producing a new iterator that returns tuples of their next elements.                             |
| `map_while(f)`      | A version of map that halts when f returns None.                                                                         |
| `flat_map(f)`       | Flattens nested iterators produced by applying function f to each element of the original iterator.                      |
| `inspect(f)`        | Calls function f on each element of the iterator, and then returns the original element.                                 |
| `scan(init, f)`     | Similar to fold, but produces an iterator of intermediate values rather than a final value.                              |
| `fold(init, f)`     | Accumulates a value over the iterator, starting with the initial value init.                                             |
| `try_fold(init, f)` | Like fold, but can stop and return None if f returns None.                                                               |
| `collect()`         | Consumes the iterator and collects all its values into a collection such as a Vec or HashMap.                            |
| `for_each(f)`       | Calls function f on each element of the iterator, discarding the return value.                                           |
| `find(p)`           | Searches for an element of the iterator that satisfies the predicate p.                                                  |
| `position(p)`       | Like find, but returns the index of the element rather than the element itself.                                          |
| `rposition(p)`      | Like position, but searches from the end of the iterator.                                                                |
| `find_map(f)`       | Similar to find, but applies function f to the iterator’s elements and returns the first non-None result.                |
| `partition(p)`      | Splits the iterator into two based on whether each element satisfies predicate p.                                        |
| `unzip()`           | Transposes a collection of tuples into a tuple of collections.                                                           |
| `zip_eq(iter)`      | Like zip, but stops when either iterator stops, ensuring that both produce the same number of elements.                  |
| `step_by(n)`        | Produces an iterator that yields every nth element of the original iterator.                                             |
| `by_ref()`          | Borrows the iterator, allowing it to be used again later, for example in a loop.                                         |
| `rev()`             | Produces an iterator that yields the elements of the original iterator in reverse order.                                 |
| `cloned()`          | Produces an iterator that clones each element of the original iterator.                                                  |
| `copied()`          | Like cloned, but converts each element to its Copy type first.                                                           |

### map and filter

| Method | Description                                                                          |
| ------ | ------------------------------------------------------------------------------------ |
| map    | Transforms each element of the iterator using a closure.                             |
| filter | Filters the iterator, returning only the elements that satisfy a predicate function. |

These adapters' signatures are as follows:

```rs
fn map<B, F>(self, f: F) -> impl Iterator<Item = B>
where
    Self: Sized,
    F: FnMut(Self::Item) -> B;

fn filter<P>(self, predicate: P) -> impl Iterator<Item = Self::Item>
where
    Self: Sized,
    P: FnMut(&Self::Item) -> bool;
```

```rs
// Using `map` to square each element of the iterator
let numbers = vec![1, 2, 3];
let squared_numbers: Vec<i32> = numbers.iter().map(|x| x * x).collect();
assert_eq!(squared_numbers, vec![1, 4, 9]);

// Using `filter` to select even numbers from the iterator
let numbers = vec![1, 2, 3, 4, 5];
let even_numbers: Vec<i32> = numbers.iter().filter(|x| x % 2 == 0).collect();
assert_eq!(even_numbers, vec![2, 4]);
```

### filter_map and flat_map

| Method     | Description                                                                                                                                                                                                                                                          |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| filter_map | Applies the given closure to each element of the iterator, producing an iterator over only the values where the closure returns Some.                                                                                                                                |
| flat_map   | Applies the given closure to each element of the iterator, producing a flattened iterator over the values produced by the closure. The closure should return an iterator, and its output will be flattened. If the closure returns None, its output will be ignored. |

```rs
fn filter_map<B, F>(self, f: F) -> impl Iterator<Item = B>
where
    Self: Sized,
    F: FnMut(Self::Item) -> Option<B>;
```

```rs
let numbers = vec![1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
let result = numbers.iter().filter_map(|n| {
    if n % 2 == 0 {
        Some((n as f32).sqrt())
    } else {
        None
    }
});

for n in result {
    println!("{}", n);
}
```

```rs
fn flat_map<U, F>(self, f: F) -> impl Iterator<Item = U::Item>
where
    F: FnMut(Self::Item) -> U,
    U: IntoIterator;
```

```rs
let sentence = "The quick brown fox jumps over the lazy dog";
let words = sentence.split(' ');
let chars = words.flat_map(|word| word.chars());

for c in chars {
    print!("{}", c);
}
```

### flatten

| Method  | Description                                                                             |
| ------- | --------------------------------------------------------------------------------------- |
| flatten | Converts an iterator of iterators into a single iterator that yields elements directly. |

```rs
fn flatten(self) -> impl Iterator<Item = Self::Item::Item>
where
    Self::Item: IntoIterator;

fn flatten(self) -> Flatten<Self>
where
    Self: Sized,
    Self::Item: IntoIterator,
```

```rs
let nested = vec![vec![1, 2], vec![3, 4, 5], vec![6]];
let flattened: Vec<i32> = nested.into_iter().flatten().collect();
assert_eq!(flattened, vec![1, 2, 3, 4, 5, 6]);
```

### take and take_while

| Method     | Description                                                                           |
| ---------- | ------------------------------------------------------------------------------------- |
| take       | returns the first `n` items of an iterator, or all items if there are fewer than `n`. |
| take_while | returns items from the input iterator until the given closure returns `false`.        |

```rs
fn take(self, n: usize) -> impl Iterator<Item = Self::Item>
where
    Self: Sized;
```

```rs
let numbers = vec![1, 2, 3, 4, 5];
let first_three = numbers.iter().take(3);
for n in first_three {
    println!("{}", n); // prints 1, 2, 3
}
```

```rs
fn take_while<P>(self, predicate: P) -> impl Iterator<Item = Self::Item>
where
    Self: Sized,
    P: FnMut(&Self::Item) -> bool;
```

```rs
let numbers = vec![1, 2, 3, 4, 5];
let less_than_four = numbers.iter().take_while(|&n| *n < 4);
for n in less_than_four {
    println!("{}", n); // prints 1, 2, 3
}
```

### skip and skip_while

| Method     | Description                                                                                                                                                      |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| skip       | skips the first `n` elements of an iterator, returning a new iterator that begins with the `n+1`th element.                                                      |
| skip_while | skips elements of an iterator until a given predicate is `true`, and returns a new iterator beginning with the first element for which the predicate is `false`. |

```rs
fn skip(self, n: usize) -> Skip<Self>

fn skip_while<P>(self, predicate: P) -> SkipWhile<Self, P>
where
    P: FnMut(&Self::Item) -> bool,
```

```rs
let numbers = vec![1, 2, 3, 4, 5];
let new_numbers = numbers.iter().skip(2);
println!("{:?}", new_numbers.collect::<Vec<&i32>>()); // Output: [&3, &4, &5]
```

```rs
let numbers = vec![1, 2, 3, 4, 5];
let new_numbers = numbers.iter().skip_while(|&x| x < &3);
println!("{:?}", new_numbers.collect::<Vec<&i32>>()); // Output: [&3, &4, &5]
```

### peekable

`Peekable` is an iterator adapter that wraps another iterator and allows you to peek at the next item without consuming it. It is used to "look ahead" at the next item in the iterator sequence, without actually consuming that item until it is explicitly requested.

```rs
fn peekable(self) -> std::iter::Peekable<Self>
where
    Self: Sized;
```

```rs
let numbers = vec![1, 2, 3, 4, 5];
let mut iter = numbers.iter().peekable();

while let Some(&num) = iter.next() {
    println!("Current number: {}", num);

    if let Some(&next_num) = iter.peek() {
        println!("Next number: {}", next_num);
    } else {
        println!("End of sequence");
    }
}
```

Peekable iterators are essential when you can’t decide how many items to consume from an iterator until you've gone too far. For example, if you're parsing numbers from a stream of characters, you can't decide where the number ends until you've seen the first nonnumber character following it:

```rs
use std::iter::Peekable;

fn parse_number<I>(tokens: &mut Peekable<I>) -> u32
where
    I: Iterator<Item = char>,
{
    let mut n = 0;
    loop {
        match tokens.peek() {
            Some(r) if r.is_digit(10) => {
                n = n * 10 + r.to_digit(10).unwrap();
            }
            _ => return n,
        }
        tokens.next();
    }
}

fn main() {
    let mut chars = "226153980,1766319049".chars().peekable();
    assert_eq!(parse_number(&mut chars), 226153980);
    assert_eq!(chars.next(), Some(','));
    assert_eq!(parse_number(&mut chars), 1766319049);
    assert_eq!(chars.next(), None);
}
```

### fuse

The `fuse` iterator adapter is used to make an iterator "fuseable", which means that once it returns `None` or panics, it will continue to do so on subsequent calls. This is useful for iterators that have a finite length or can only be traversed once, as it ensures that subsequent calls to the iterator do not accidentally modify state or produce unexpected behavior.

```rs
struct Flaky(bool);
impl Iterator for Flaky {
    type Item = &'static str;
    fn next(&mut self) -> Option<Self::Item> {
        if self.0 {
            self.0 = false;
            Some("totally the last item")
        } else {
            self.0 = true;
            None
        }
    }
}

fn main() {
    let mut flaky = Flaky(true);
    assert_eq!(flaky.next(), Some("totally the last item"));
    assert_eq!(flaky.next(), None);
    assert_eq!(flaky.next(), Some("totally the last item"));
    let mut not_flaky = Flaky(true).fuse();
    assert_eq!(not_flaky.next(), Some("totally the last item"));
    assert_eq!(not_flaky.next(), None);
    assert_eq!(not_flaky.next(), None);
}
```

### Reversible Iterators and rev

In Rust, an iterator is a unidirectional sequence of values. It starts from the first element, proceeds sequentially through each element, and then terminates at the last element.

However, there are cases where we need to traverse a sequence of elements in reverse order. For example, we might want to traverse a string backward or access the last elements of a vector.

To support iteration in reverse order, Rust provides a "Reversible Iterator" trait called `DoubleEndedIterator`. This trait extends the `Iterator` trait to provide two additional methods, `next_back()` and `peek_back()`, that allow us to iterate backward through the sequence.

In addition to the `DoubleEndedIterator` trait, Rust also provides a `rev()` method that returns a new iterator that produces the elements of the original iterator in reverse order. The `rev()` method can be used with any iterator that implements the `DoubleEndedIterator` trait.

Here is a table of the methods provided by the `DoubleEndedIterator` trait:

| Method        | Description                                                                                               |
| ------------- | --------------------------------------------------------------------------------------------------------- |
| `next_back()` | Returns the next element of the iterator in reverse order.                                                |
| `peek_back()` | Returns a reference to the next element of the iterator in reverse order, without advancing the iterator. |

```rs
let bee_parts = ["head", "thorax", "abdomen"];
let mut iter = bee_parts.iter();

assert_eq!(iter.next(), Some(&"head"));
assert_eq!(iter.next_back(), Some(&"abdomen"));
assert_eq!(iter.next(), Some(&"thorax"));
assert_eq!(iter.next_back(), None);
assert_eq!(iter.next(), None);
```

Here is an example of using `rev()` method to reverse the order of a vector:

```rs
let v = vec![1, 2, 3, 4];
for i in v.iter().rev() {
    println!("{}", i);
}
```

In this example, we first call the `iter()` method on the vector to create an iterator that produces references to its elements. We then call the `rev()` method to create a new iterator that produces the elements in reverse order. Finally, we use a `for` loop to iterate over the elements and print them to the console.

### inspect

`inspect()` is an iterator adapter in Rust that can be used for debugging purposes. It lets you inspect the values produced by the iterator at any point in the iteration process, without changing the output of the iterator.

```rs
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];
    let sum = numbers
        .iter()
        .inspect(|x| println!("Inspecting: {}", x))
        .fold(0, |acc, x| acc + x);
    println!("Sum: {}", sum);
}
```

the `inspect()` method is used to print out each element of the iterator as it is being processed by `fold()`. This can be useful for understanding how the iterator is working, or for debugging if you are having issues with your iterator.

```rs
fn main() {
    let upper_case: String = "große"
        .chars()
        .inspect(|c| println!("before: {:?}", c))
        .flat_map(|c| c.to_uppercase())
        .inspect(|c| println!(" after: {:?}", c))
        .collect();
    assert_eq!(upper_case, "GROSSE");
}
```

The `inspect()` method is used to print each character before and after the uppercase transformation.
The `inspect()` method is a way to peek at each element of an iterator as it passes through a pipeline of methods.

### chain

`chain()` is an iterator adapter method in Rust that chains two iterators together to create a new iterator that will first return all the items from the first iterator, and then all the items from the second iterator.

```rs
fn chain<U>(self, other: U) -> impl Iterator<Item = Self::Item>
where
    Self: Sized,
    U: IntoIterator<Item = Self::Item>;
```

In other words, you can chain an iterator together with any iterable that produces the same item type.

```rs
let v: Vec<i32> = (1..4).chain(vec![20, 30, 40]).collect();
assert_eq!(v, [1, 2, 3, 20, 30, 40]);
```

### enumerate

`enumerate()` is a method provided by iterators in Rust, which adds a counter to the elements of an iterator and yields them as tuples containing the index and the value. The syntax of `enumerate()` is as follows:

```rs
fn enumerate(self) -> Enumerate<Self>
```

Here, `self` represents the iterator on which `enumerate()` is called, and the method returns an `Enumerate` struct, which implements the `Iterator` trait and generates tuples `(usize, T)` where `T` is the element type of the iterator.

Example usage of `enumerate()`:

```rs
let fruits = vec!["apple", "banana", "cherry"];

for (index, fruit) in fruits.iter().enumerate() {
    println!("Index {}: {}", index, fruit);
}
```

Output:

```yaml
Index 0: apple
Index 1: banana
Index 2: cherry
```

In this example, `enumerate()` is called on an iterator created from a vector of fruits. The `for` loop iterates over the enumerated iterator, unpacks the tuples into variables `index` and `fruit`, and prints them out. The output shows that the `enumerate()` method has added an index to each fruit.

### zip

`zip()` is a method in Rust that takes two iterators and combines them into a new iterator, producing a sequence of pairs where the i-th pair contains the i-th element from each of the input iterators. The resulting iterator stops when either of the input iterators stops.

Here's an example usage of `zip()` in Rust:

```rs
let v1 = vec![1, 2, 3];
let v2 = vec!["one", "two", "three"];

let zipped = v1.iter().zip(v2.iter());

for (i, &s) in zipped {
    println!("{} is {}", i, s);
}
```

Output:

```python
1 is one
2 is two
3 is three
```

In the example above, `v1.iter()` returns an iterator over the elements of `v1`, while `v2.iter()` returns an iterator over the elements of `v2`. These two iterators are then passed as arguments to the `zip()` method, which returns an iterator that produces pairs of elements, one from `v1` and one from `v2`.

Finally, the resulting iterator is looped through using a `for` loop that destructures each pair into its individual components `(i, &s)` so that they can be printed to the console.

### by_ref

`by_ref` is an iterator adapter method in Rust which converts an iterator into a reference to an iterator. This is useful when we want to consume part of an iterator and then pass the rest of it to another function or closure. `by_ref` creates a new iterator that borrows from the original iterator, and can be used to implement "splitting" an iterator in two parts.

Here's an example of how `by_ref` can be used:

```rs
fn main() {
    let nums = vec![1, 2, 3, 4, 5];
    let mut iter = nums.iter();
    let first_three = iter.by_ref().take(3);

    for num in first_three {
        println!("num: {}", num);
    }

    let rest = iter.take(2);
    for num in rest {
        println!("num: {}", num);
    }
}
```

In this example, we create a vector `nums` containing some integers, and create an iterator `iter` over it. We then use `by_ref` to create a new iterator `first_three` that borrows from `iter` and takes the first three elements. We iterate over `first_three` to print out its contents.

After consuming the first three elements of `iter`, we create another iterator `rest` by calling `iter.take(2)`, which will take the next two elements of `iter`. We iterate over `rest` to print out its contents.

Note that if we didn't use `by_ref` to create `first_three`, the call to `iter.take(2)` would consume the first five elements of `iter` instead of just the last two, since `iter` has already been partially consumed by `first_three`.

### cloned, copied

The `cloned()` and `copied()` iterator adapters create a new iterator that clones or copies each item of the original iterator.

The `cloned()` method is used when the original iterator's items are references to objects, and we want to obtain new objects that are cloned from the referenced ones.

For example:

```rs
let numbers = vec![1, 2, 3];
let cloned_numbers: Vec<i32> = numbers.iter().cloned().collect();
assert_eq!(cloned_numbers, vec![1, 2, 3]);
```

Here, the `iter()` method creates an iterator over references to the elements in the `numbers` vector. The `cloned()` method creates a new iterator that clones each element, yielding a new `i32` value for each original value. The `collect()` method consumes the cloned iterator and creates a new `Vec<i32>` from the cloned values.

The `copied()` method is used when the original iterator's items are primitive values or values that implement the `Copy` trait, and we want to obtain new values that are copied from the original ones.

For example:

```rs
let numbers = [1, 2, 3];
let copied_numbers: Vec<i32> = numbers.iter().copied().collect();
assert_eq!(copied_numbers, vec![1, 2, 3]);
```

Here, the `iter()` method creates an iterator over references to the elements in the `numbers` array. The `copied()` method creates a new iterator that copies each element, yielding a new `i32` value for each original value. The `collect()` method consumes the copied iterator and creates a new `Vec<i32>` from the copied values.

### cycle

The `cycle` iterator adapter is used to repeat the elements of an iterator infinitely. It creates a new iterator that cycles through the elements of the original iterator repeatedly.

Here is an example of using `cycle` with a vector:

```rs
let v = vec![1, 2, 3];
let mut cycle_iter = v.iter().cycle();
assert_eq!(cycle_iter.next(), Some(&1));
assert_eq!(cycle_iter.next(), Some(&2));
assert_eq!(cycle_iter.next(), Some(&3));
assert_eq!(cycle_iter.next(), Some(&1));
assert_eq!(cycle_iter.next(), Some(&2));
assert_eq!(cycle_iter.next(), Some(&3));
```

In the example, the `iter()` method is used to create an iterator over the elements of the vector `v`. The `cycle()` method is then called on the iterator to create a new iterator that cycles through the elements of `v`. The `next()` method is then used to iterate through the elements of the new iterator. Because `cycle` repeats the elements of the original iterator, the iterator never ends, so `next()` can be called repeatedly without ever returning `None`.

```rs
use std::iter::{once, repeat};

fn main() {
    let fizzes = repeat("").take(2).chain(once("fizz")).cycle();
    let buzzes = repeat("").take(4).chain(once("buzz")).cycle();
    let fizzes_buzzes = fizzes.zip(buzzes);
    let fizz_buzz = (1..100).zip(fizzes_buzzes).map(|tuple| match tuple {
        (i, ("", "")) => i.to_string(),
        (_, (fizz, buzz)) => format!("{}{}", fizz, buzz),
    });
    for line in fizz_buzz {
        println!("{}", line);
    }
}
```

This code is an implementation of the classic "FizzBuzz" programming problem.

The program creates two infinite iterators using the `repeat()` function, one for "fizzes" and one for "buzzes". The `take()` method is used to limit each iterator to a certain number of elements, and `chain()` is used to append a single "fizz" or "buzz" to the end of each iterator.

The `cycle()` method is then called on each iterator to create an infinite cycle of "fizzes" and "buzzes". The `zip()` method is used to combine the two cycles into a single iterator of tuples.

The program then creates a range from 1 to 99 using the `..` operator and calls `zip()` on that range and the `fizzes_buzzes` iterator. The resulting iterator contains tuples of the form `(i, (fizz, buzz))`, where `i` is the current number and `fizz` and `buzz` are the next values from the "fizzes" and "buzzes" cycles, respectively.

The `map()` method is then called on this iterator to transform each tuple into a string. If both `fizz` and `buzz` are empty strings, then the string representation of `i` is returned. Otherwise, the `fizz` and `buzz` strings are concatenated and returned.

Finally, a `for` loop is used to print out each string in the resulting iterator.
