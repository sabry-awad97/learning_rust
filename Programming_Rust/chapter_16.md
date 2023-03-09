# Collections

Here's a summary of the standard collections in Rust, along with their equivalents in C++, Java, Python, and JavaScript:

| Collection                                    | Description                               | Similar collection type in C++ | Similar collection type in Java | Similar collection type in Python | Similar collection type in JavaScript |
| --------------------------------------------- | ----------------------------------------- | ------------------------------ | ------------------------------- | --------------------------------- | ------------------------------------- |
| `Vec<T>`                                      | Growable array                            | `std::vector<T>`               | `ArrayList<T>`                  | `list`                            | `Array`                               |
| `VecDeque<T>`                                 | Double-ended queue (growable ring buffer) | `std::deque<T>`                | `ArrayDeque<T>`                 | `collections.deque`               | N/A                                   |
| `LinkedList<T>`                               | Doubly linked list                        | N/A                            | `LinkedList<T>`                 | `LinkedList`                      | N/A                                   |
| `BinaryHeap<T>`&lt;br&gt;where `T: Ord`       | Max heap priority queue                   | `std::priority_queue<T>`       | `PriorityQueue<T>`              | `heapq`                           | N/A                                   |
| `HashMap<K, V>`&lt;br&gt;where `K: Eq + Hash` | Key-value hash table                      | `std::unordered_map<K, V>`     | `HashMap<K, V>`                 | `dict`                            | `Map`                                 |
| `BTreeMap<K, V>`&lt;br&gt;where `K: Ord`      | Sorted key-value table                    | `std::map<K, V>`               | `TreeMap<K, V>`                 | N/A                               | `Map`                                 |
| `HashSet<T>`&lt;br&gt;where `T: Eq + Hash`    | Unordered, hash-based set                 | `std::unordered_set<T>`        | `HashSet<T>`                    | `set`                             | `Set`                                 |
| `BTreeSet<T>`&lt;br&gt;where `T: Ord`         | Sorted set                                | N/A                            | `TreeSet<T>`                    | N/A                               | `Set`                                 |

Here's a brief summary of each collection:

- `Vec<T>`: A growable array that stores elements of type `T` contiguously in memory. It's useful when you need a dynamically sized array that can be resized efficiently.
- `VecDeque<T>`: A double-ended queue that allows efficient insertion and removal from both ends. It's implemented as a growable ring buffer.
- `LinkedList<T>`: A doubly linked list that allows efficient insertion and removal at any position. It's useful when you need to frequently insert or remove elements in the middle of the list.
- `BinaryHeap<T>`: A max heap priority queue that allows efficient insertion and retrieval of the maximum element. It's useful when you need to repeatedly find the maximum element in a collection.
- `HashMap<K, V>`: A key-value hash table that allows efficient insertion, retrieval, and removal of elements by key. It's useful when you need to quickly look up values by a unique key.
- `BTreeMap<K, V>`: A sorted key-value table that allows efficient insertion, retrieval, and removal of elements by key. It's useful when you need to maintain the order of elements by key.
- `HashSet<T>`: An unordered, hash-based set that allows efficient insertion, retrieval, and removal of elements. It's useful when you need to quickly check if an element is in a collection.
- `BTreeSet<T>`: A sorted set that allows efficient insertion, retrieval, and removal of elements. It's useful when you need to maintain the order of elements.

## `Vec<T>`

`Vec<T>` is a growable array that stores elements of type `T` contiguously in memory. It's one of the most commonly used collection types in Rust and is similar to C++'s `std::vector` and Java's `ArrayList`.

`Vec<T>` has the following advantages:

- Dynamic sizing: The size of a `Vec<T>` can be changed at runtime, making it useful for situations where the size of the collection is unknown or changes frequently.
- Fast indexing: `Vec<T>` provides O(1) indexing, meaning that accessing an element at a given index is very fast.
- Efficient memory usage: `Vec<T>` stores its elements contiguously in memory, which allows for efficient use of CPU caches and reduces memory fragmentation.

However, `Vec<T>` also has some disadvantages:

- Insertion and removal: Inserting or removing elements in the middle of a `Vec<T>` can be slow, as it requires shifting all the elements after the insertion/removal point.
- Ownership: A `Vec<T>` owns its elements, meaning that it is responsible for allocating and deallocating the memory used to store them. This can make it difficult to pass a `Vec<T>` to functions that only need to borrow the elements.

Overall, `Vec<T>` is a versatile and efficient collection type that is suitable for many use cases. Rust's ownership model ensures that `Vec<T>` is safe to use and prevents common programming errors like buffer overflows and use-after-free bugs.

### Creating a vector

Sure, here are some common ways to create a `Vec<T>` in Rust:

1. Using the `vec!` macro:

```rs
let v = vec![1, 2, 3];
```

This creates a `Vec<i32>` containing the elements `1`, `2`, and `3`.

1. Using the `new` method and `push` method:

```rs
let mut v = Vec::new();
v.push(1);
v.push(2);
v.push(3);
```

This creates an empty `Vec<i32>` and adds the elements `1`, `2`, and `3` to it.

1. Using the `with_capacity` method and `push` method:

```rs
let mut v = Vec::with_capacity(10);
for i in 0..10 {
    v.push(i);
}
```

This creates a `Vec<i32>` with a capacity of 10 and adds the numbers `0` through `9` to the vector.

1. Using the `from_iter` method:

```rs
let v = Vec::from_iter(1..=3);
```

This creates a `Vec<i32>` containing the elements `1`, `2`, and `3` by converting an iterator of the same type to a vector.

1. Using the `collect` method:

```rs
let v: Vec<i32> = (1..=3).collect();
```

This creates a `Vec<i32>` containing the elements `1`, `2`, and `3` by collecting an iterator of the same type into a vector.

1. Using the `into_iter` method and `collect` method:

```rs
let v = ["1", "2", "3"].into_iter().map(|x| x.parse().unwrap()).collect::<Vec<i32>>();
```

This creates a `Vec<i32>` containing the elements `1`, `2`, and `3` by converting an array of strings to an iterator of integers, and then collecting the iterator into a vector.

1. Using the `from` method:

```rs
let v = Vec::from(&[1, 2, 3][..]);
```

This creates a `Vec<i32>` containing the elements `1`, `2`, and `3` by converting a slice of the same type to a vector.

### Accessing Elements

Sure! Once you have created a `Vec<T>` in Rust, you can access its elements using indexing or iteration.

To access an element at a specific index, you can use the indexing operator `[]` with the desired index value. Here's an example:

```rs
let v = vec![1, 2, 3];
println!("{}", v[1]); // Output: 2
```

In this example, the value at index `1` in the `Vec<i32>` `v` is accessed and printed.

You can also use iteration to access each element in a `Vec<T>`. One way to do this is to use a `for` loop with the `iter` method, which returns an iterator over the vector's elements:

```rs
let v = vec![1, 2, 3];
for i in v.iter() {
    println!("{}", i);
}
```

Another way to iterate over a `Vec<T>` is to use a `for` loop with the `into_iter` method, which consumes the vector and returns an iterator that yields ownership of the vector's elements:

```rs
let v = vec![1, 2, 3];
for i in v.into_iter() {
    println!("{}", i);
}
```

However, note that after iterating over a `Vec<T>` with `into_iter`, the vector will be empty and its elements will have been moved elsewhere.

Finally, you can also use the `get` method to access an element at a given index in a `Vec<T>`. This method returns an `Option<&T>` value, which is either `Some(&element)` if the element at the given index exists, or `None` if the index is out of bounds. Here's an example:

```rs
let v = vec![1, 2, 3];
match v.get(1) {
    Some(x) => println!("{}", x),
    None => println!("Index out of bounds!"),
}
```

Note that the indexing operator `[]` can also be used to access elements of a `Vec<T>` if you are sure the index is within bounds. However, if you are not sure, using the `get` method with a pattern match is a safer option that avoids panics.

### Growing and Shrinking Vectors

Sure, let's talk about growing and shrinking vectors in Rust!

A `Vec<T>` in Rust is designed to be a growable array, which means you can easily add or remove elements from it. You can add elements to the end of a vector using the `push` method, like this:

```rs
let mut v = vec![1, 2, 3];
v.push(4);
println!("{:?}", v); // Output: [1, 2, 3, 4]
```

In this example, the `push` method is used to add the value `4` to the end of the `Vec<i32>` `v`.

You can also insert elements at a specific index in a vector using the `insert` method:

```rs
let mut v = vec![1, 2, 3];
v.insert(1, 4);
println!("{:?}", v); // Output: [1, 4, 2, 3]
```

In this example, the `insert` method is used to insert the value `4` at index `1` in the `Vec<i32>` `v`.

To remove elements from a vector, you can use the `pop` method to remove the last element, or the `remove` method to remove an element at a specific index. Here are some examples:

```rs
let mut v = vec![1, 2, 3];
v.pop();
println!("{:?}", v); // Output: [1, 2]

v.remove(0);
println!("{:?}", v); // Output: [2]
```

In the first example, the `pop` method is used to remove the last element of the `Vec<i32>` `v`. In the second example, the `remove` method is used to remove the element at index `0` of `v`.

Here is a table summarizing the methods:

| Method                              | Description                                                                                                  |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| `slice.len()`                       | Returns the length of the slice                                                                              |
| `slice.is_empty()`                  | Returns `true` if the slice is empty                                                                         |
| `Vec::with_capacity(n)`             | Creates a new vector with capacity for `n` elements                                                          |
| `vec.capacity()`                    | Returns the current capacity of the vector                                                                   |
| `vec.reserve(n)`                    | Increases the capacity of the vector by at least `n` elements                                                |
| `vec.reserve_exact(n)`              | Increases the capacity of the vector by exactly `n` elements                                                 |
| `vec.shrink_to_fit()`               | Shrinks the capacity of the vector to fit its length                                                         |
| `vec.push(value)`                   | Adds an element to the end of the vector                                                                     |
| `vec.pop()`                         | Removes and returns the last element of the vector                                                           |
| `vec.insert(index, value)`          | Inserts an element at the given index in the vector                                                          |
| `vec.remove(index)`                 | Removes and returns the element at the given index in the vector                                             |
| `vec.resize(new_len, value)`        | Resizes the vector to the given length and fills any new elements with the given value                       |
| `vec.resize_with(new_len, closure)` | Resizes the vector to the given length and fills any new elements using the given closure                    |
| `vec.truncate(new_len)`             | Truncates the vector to the given length                                                                     |
| `vec.clear()`                       | Removes all elements from the vector                                                                         |
| `vec.extend(iterable)`              | Appends the elements from an iterable to the end of the vector                                               |
| `vec.split_off(index)`              | Splits the vector into two at the given index, returning the second half                                     |
| `vec.append(&mut vec2)`             | Appends the elements from another vector to the end of the vector                                            |
| `vec.drain(range)`                  | Removes and returns the elements in the given range from the vector                                          |
| `vec.retain(test)`                  | Removes all elements from the vector that do not satisfy the given test                                      |
| `vec.dedup()`                       | Removes consecutive duplicate elements from the vector                                                       |
| `vec.dedup_by(same)`                | Removes consecutive elements from the vector that satisfy the given equality test                            |
| `vec.dedup_by_key(key)`             | Removes consecutive elements from the vector that have the same key, as determined by the given key function |

Here's an example of each function:

| Method                            | Example                                                                                                 |
| --------------------------------- | ------------------------------------------------------------------------------------------------------- |
| slice.len()                       | let s = \[1, 2, 3\]; assert_eq!(s.len(), 3);                                                            |
| slice.is_empty()                  | let s = \[1, 2, 3\]; assert_eq!(s.is_empty(), false);                                                   |
| Vec::with_capacity(n)             | let mut v = Vec::with_capacity(10);                                                                     |
| vec.capacity()                    | let mut v = vec!\[1, 2, 3\]; assert_eq!(v.capacity(), 3);                                               |
| vec.reserve(n)                    | let mut v = vec!\[1, 2, 3\]; v.reserve(10);                                                             |
| vec.reserve_exact(n)              | let mut v = vec!\[1, 2, 3\]; v.reserve_exact(10);                                                       |
| vec.shrink_to_fit()               | let mut v = vec!\[1, 2, 3\]; v.shrink_to_fit();                                                         |
| vec.push(value)                   | let mut v = vec!\[1, 2, 3\]; v.push(4);                                                                 |
| vec.pop()                         | let mut v = vec!\[1, 2, 3\]; assert_eq!(v.pop(), Some(3));                                              |
| vec.insert(index, value)          | let mut v = vec!\[1, 2, 3\]; v.insert(1, 4);                                                            |
| vec.remove(index)                 | let mut v = vec!\[1, 2, 3\]; v.remove(1);                                                               |
| vec.resize(new_len, value)        | let mut v = vec!\[1, 2, 3\]; v.resize(5, 0);                                                            |
| vec.resize_with(new_len, closure) | let mut v = vec!\[1, 2, 3\]; v.resize_with(5, Default::default);                                        |
| vec.truncate(new_len)             | let mut v = vec!\[1, 2, 3\]; v.truncate(1);                                                             |
| vec.clear()                       | let mut v = vec!\[1, 2, 3\]; v.clear();                                                                 |
| vec.extend(iterable)              | let mut v = vec!\[1, 2, 3\]; v.extend(\[4, 5, 6\].iter().copied());                                     |
| vec.split_off(index)              | let mut v = vec!\[1, 2, 3\]; let s = v.split_off(1);                                                    |
| vec.append(&mut vec2)             | let mut v = vec!\[1, 2, 3\]; let mut v2 = vec!\[4, 5, 6\]; v.append(&mut v2);                           |
| vec.drain(range)                  | let mut v = vec!\[1, 2, 3\]; v.drain(1..2);                                                             |
| vec.retain(test)                  | let mut v = vec!\[1, 2, 3\]; v.retain(\|&x\| x > 1);                                                    |
| vec.dedup()                       | let mut v = vec!\[1, 2, 2, 3, 3\]; v.dedup();                                                           |
| vec.dedup_by(same)                | let mut v = vec!\["apple", "banana", "pear", "orange", "peach", "berry"\]; v.dedup_by(\|a, b\| a == b); |
| vec.dedup_by_key(key)             | let mut v = vec!\[1, 2, 2, 3, 3\]; v.dedup_by_key(\|x\| \*x / 2)                                        |

### Joining

In Rust, the `concat()` method can be called on a slice of type `&[T]` to join all elements into a single owned value. This method takes no arguments and returns a new `Vec<T>`.

```rs
let slice1 = &[1, 2, 3];
let slice2 = &[4, 5, 6];

let concatenated = slice1.concat(&slice2);
println!("{:?}", concatenated);
```

Note that `concat()` takes a reference to the slices and returns a new owned `Vec<T>`. The original slices remain unchanged. If you want to join multiple slices together, you can call `concat()` repeatedly.

The `join()` method can be called on an iterator over strings to concatenate all elements into a single string separated by a given separator. This method takes a single argument, a string slice, which is used as the separator between the joined elements.

```rs
let strings = vec!["foo", "bar", "baz"];
let joined = strings.join(", ");
println!("{}", joined);
```

Note that `join()` takes ownership of the iterator, so the original vector is moved and can no longer be used. If you want to join elements without consuming the iterator, you can use the `intersperse()` method from the itertools crate, which adds separators between the elements of the iterator.

### Splitting

It’s easy to get many non-mut references into an array, slice, or vector at once:

```rs
let v = vec![0, 1, 2, 3];
let a = &v[i];
let b = &v[j];
let mid = v.len() / 2;
let front_half = &v[..mid];
let back_half = &v[mid..];
```

Getting multiple mutable references to the same slice or vector is not allowed in Rust, as it violates the borrowing rules that ensure memory safety. If you have a mutable reference to a slice or vector, you cannot get another mutable reference to the same slice or vector until the first mutable reference goes out of scope.

However, there are several ways to work around this restriction depending on your use case.

#### slice.split_at(index), slice.split_at_mut(index)

The `split_at` method can be used to split a slice into two non-overlapping slices at a given index. The first slice will contain elements up to the index (exclusive), while the second slice will contain the elements from the index (inclusive) to the end of the original slice.

Here's an example of using `split_at` to split a slice into two:

```rs
let v = [1, 2, 3, 4, 5];
let (left, right) = v.split_at(3);
assert_eq!(left, [1, 2, 3]);
assert_eq!(right, [4, 5]);
```

The `split_at_mut` method is similar, but it returns mutable references to the two slices:

```rs
let mut v = [1, 2, 3, 4, 5];
let (left, right) = v.split_at_mut(3);
left[0] = 0;
right[0] = 6;
assert_eq!(v, [0, 2, 3, 6, 5]);
```

#### slice.split_first(), slice.split_first_mut()

The `split_first()` method returns an optional reference to the first element of a slice, and a slice of the rest of the elements. If the slice is empty, it returns `None`.

Here's an example of using `split_first()`:

```rs
let v = [1, 2, 3, 4, 5];
if let Some((first, rest)) = v.split_first() {
    println!("The first element is {} and the rest is {:?}", first, rest);
} else {
    println!("The slice is empty");
}
```

The `split_first_mut()` method is similar, but returns a mutable reference to the first element:

```rs
let mut v = [1, 2, 3, 4, 5];
if let Some((first, rest)) = v.split_first_mut() {
    *first = 0;
    println!("The first element is now {} and the rest is {:?}", first, rest);
} else {
    println!("The slice is empty");
}
```

#### slice.split_last(), slice.split_last_mut()

The `split_last()` method returns a tuple containing the last element of the slice and all the other elements as a slice. It returns `None` if the slice is empty.

Here's an example:

```rs
let numbers = [1, 2, 3, 4, 5];
if let Some((last, elements)) = numbers.split_last() {
    println!("The last element is {}, and the other elements are {:?}", last, elements);
} else {
    println!("The slice is empty");
}
```

The `split_last_mut()` method is similar to `split_last()`, but it returns a mutable reference to the last element and a mutable slice of the other elements.

Here's an example:

```rs
let mut numbers = [1, 2, 3, 4, 5];
if let Some((last, elements)) = numbers.split_last_mut() {
    *last = 6;
    elements[0] = 0;
    println!("The last element is now {}, and the other elements are {:?}", last, elements);
} else {
    println!("The slice is empty");
}
```

#### slice.split(is_sep), slice.split_mut(is_sep)

The `split()` and `split_mut()` methods on slices return an iterator over the non-overlapping substrings of the original slice that are delimited by a separator determined by a closure.

The `is_sep` closure takes a single argument of type `&T` (where `T` is the element type of the slice) and returns a boolean indicating whether the given element should be considered a separator or not.

Here's an example of using `split()` to split a slice of integers into sub-slices delimited by odd numbers:

```rs
let v = vec![1, 2, 3, 4, 5, 6, 7, 8, 9];
let mut iter = v.split(|x| x % 2 != 0);
while let Some(subslice) = iter.next() {
    println!("{:?}", subslice);
}
```

In this example, the closure `|x| x % 2 != 0` is used to determine the separator. This closure returns `true` for odd numbers and `false` for even numbers, so the slice is split into sub-slices delimited by odd numbers.

The `split_mut()` method works similarly to `split()`, but it returns an iterator over mutable references to the non-overlapping sub-slices of the original slice.

#### slice.rsplit(is_sep), slice.rsplit_mut(is_sep)

The `rsplit` and `rsplit_mut` methods are similar to the `split` and `split_mut` methods, except that they start splitting the slice from the end rather than from the beginning.

```rs
fn main() {
    let s = "one two three four five";
    let words: Vec<&str> = s.split_whitespace().collect();
    let mut last_word = "";
    let mut rest = &s[..];

    for word in words.rsplit(|c: &char| c.is_whitespace()) {
        if word != "" {
            last_word = word;
            rest = &rest[..word.as_ptr() as usize - s.as_ptr() as usize];
            break;
        }
    }

    println!("last word: {}", last_word);
    println!("rest: '{}'", rest);
}
```

#### slice.chunks(n), slice.chunks_mut(n)

In Rust, the `chunks()` method and `chunks_mut()` method are used to split a slice into smaller fixed-size chunks.

The `chunks()` method returns an iterator over immutable references to the smaller chunks, while the `chunks_mut()` method returns an iterator over mutable references to the smaller chunks. Both methods take an argument `n` that specifies the size of each chunk.

Here's an example of using the `chunks()` method:

```rs
let slice = [1, 2, 3, 4, 5, 6];
let mut iter = slice.chunks(2);

assert_eq!(iter.next(), Some(&[1, 2][..]));
assert_eq!(iter.next(), Some(&[3, 4][..]));
assert_eq!(iter.next(), Some(&[5, 6][..]));
assert_eq!(iter.next(), None);
```

In this example, we define a slice of integers `slice` and create an iterator over its chunks of size 2 using the `chunks()` method. We then use the `assert_eq!()` macro to compare the iterator's first three elements with their expected values. Note that the `&` operator is used to take a reference to each chunk, and the `[..]` syntax is used to create a slice from the reference.

Here's an example of using the `chunks_mut()` method:

```rs
let mut slice = [1, 2, 3, 4, 5, 6];
let mut iter = slice.chunks_mut(2);

if let Some(chunk) = iter.next() {
    chunk[0] = 7;
}

assert_eq!(slice, [7, 2, 3, 4, 5, 6]);
```

In this example, we define a mutable slice of integers `slice` and create an iterator over its mutable chunks of size 2 using the `chunks_mut()` method. We then modify the first element of the first chunk using the index notation. Finally, we use the `assert_eq!()` macro to compare the modified slice with its expected value.

Both `chunks()` and `chunks_mut()` can also be used with slices of other types, such as characters or strings. Note that the size of the slice must be divisible by the chunk size, or the last chunk may have a smaller size.

#### slice.rchunks(n), slice.rchunks_mut(n)

In Rust, the `rchunks()` method and `rchunks_mut()` method are used to split a slice into smaller fixed-size chunks from the end of the slice.

Here's an example of using the `rchunks()` method:

```rs
let slice = [1, 2, 3, 4, 5, 6];
let mut iter = slice.rchunks(2);

assert_eq!(iter.next(), Some(&[5, 6][..]));
assert_eq!(iter.next(), Some(&[3, 4][..]));
assert_eq!(iter.next(), Some(&[1, 2][..]));
assert_eq!(iter.next(), None);
```

Here's an example of using the `rchunks_mut()` method:

```rs
let mut slice = [1, 2, 3, 4, 5, 6];
let mut iter = slice.rchunks_mut(2);

if let Some(chunk) = iter.next() {
chunk[1] = 7;
}

assert_eq!(slice, [1, 2, 3, 4, 7, 6]);
```

#### slice.chunks_exact(n), slice.chunks_exact_mut(n)

Here's an example of using the `chunks_exact()` method:

```rs
let slice = [1, 2, 3, 4, 5, 6];
let mut iter = slice.chunks_exact(2);

assert_eq!(iter.next(), Some(&[1, 2][..]));
assert_eq!(iter.next(), Some(&[3, 4][..]));
assert_eq!(iter.next(), Some(&[5, 6][..]));
assert_eq!(iter.next(), None);
```

Here's an example of using the `chunks_exact_mut()` method:

```rs
let mut slice = [1, 2, 3, 4, 5, 6];
let mut iter = slice.chunks_exact_mut(2);

if let Some(chunk) = iter.next() {
    chunk[0] = 7;
}

assert_eq!(slice, [7, 2, 3, 4, 5, 6]);
```

Both `chunks_exact()` and `chunks_exact_mut()` can also be used with slices of other types, such as characters or strings. Note that if the size of the slice is not divisible by the chunk size, the last chunk may have a smaller size and can be accessed using the `remainder()` method on the iterator.

#### slice.rchunks_exact(n), slice.rchunks_exact_mut(n)

Here's an example of using the `rchunks_exact()` method:

```rs
let slice = [1, 2, 3, 4, 5, 6];
let mut iter = slice.rchunks_exact(2);

assert_eq!(iter.next(), Some(&[5, 6][..]));
assert_eq!(iter.next(), Some(&[3, 4][..]));
assert_eq!(iter.next(), Some(&[1, 2][..]));
assert_eq!(iter.next(), None);
```

Here's an example of using the `rchunks_exact_mut()` method:

```rs
let mut slice = [1, 2, 3, 4, 5, 6];
let mut iter = slice.rchunks_exact_mut(2);

if let Some(chunk) = iter.next() {
    chunk[1] = 7;
}

assert_eq!(slice, [1, 2, 3, 7, 5, 6]);
```

#### slice.windows(n)

In Rust, `slice.windows(n)` returns an iterator over all contiguous windows of length n in the slice.

```rs
let numbers = [1, 2, 3, 4, 5];
for window in numbers.windows(3) {
    println!("{:?}", window);
}
```

Note that the slices returned by `windows(n)` are views into the original slice, so they don't allocate any memory. If you need to perform some operation on each window and want to modify the original slice, you can use the `windows_mut(n)` method instead.

### Swapping

`slice.swap(i, j)` is a method on slices that swaps the elements at indices `i` and `j`.

Here's an example:

```rs
fn main() {
    let mut numbers = [1, 2, 3, 4, 5];
    numbers.swap(1, 3);

    println!("{:?}", numbers);
}
```

`slice_a.swap(&mut slice_b)` is a method on slices that swaps the elements of two slices. This method requires that the two slices have the same length.

Here's an example:

```rs
fn main() {
    let mut slice_a = [1, 2, 3, 4, 5];
    let mut slice_b = [6, 7, 8, 9, 10];

    slice_a.swap(&mut slice_b);

    println!("slice_a: {:?}", slice_a);
    println!("slice_b: {:?}", slice_b);
}
```

In Rust, `vec.swap_remove(i)` is a method on vectors that removes the element at index `i` and returns it, while preserving the order of the other elements in the vector. This method is more efficient than using `remove` followed by `swap`. It's useful when you don't care about the order of the items left in the vector.

Here's an example:

```rs
fn main() {
    let mut numbers = vec![1, 2, 3, 4, 5];
    let removed = numbers.swap_remove(2);

    println!("Removed element: {}", removed);
    println!("Numbers after removal: {:?}", numbers);
}
```
