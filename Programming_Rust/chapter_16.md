# Collections

Here's a summary of the standard collections in Rust, along with their equivalents in C++, Java, Python, and JavaScript:

| Collection                           | Description                               | Similar collection type in C++ | Similar collection type in Java | Similar collection type in Python | Similar collection type in JavaScript |
| ------------------------------------ | ----------------------------------------- | ------------------------------ | ------------------------------- | --------------------------------- | ------------------------------------- |
| `Vec<T>`                             | Growable array                            | `std::vector<T>`               | `ArrayList<T>`                  | `list`                            | `Array`                               |
| `VecDeque<T>`                        | Double-ended queue (growable ring buffer) | `std::deque<T>`                | `ArrayDeque<T>`                 | `collections.deque`               | N/A                                   |
| `LinkedList<T>`                      | Doubly linked list                        | N/A                            | `LinkedList<T>`                 | `LinkedList`                      | N/A                                   |
| `BinaryHeap<T>` where `T: Ord`       | Max heap priority queue                   | `std::priority_queue<T>`       | `PriorityQueue<T>`              | `heapq`                           | N/A                                   |
| `HashMap<K, V>` where `K: Eq + Hash` | Key-value hash table                      | `std::unordered_map<K, V>`     | `HashMap<K, V>`                 | `dict`                            | `Map`                                 |
| `BTreeMap<K, V>` where `K: Ord`      | Sorted key-value table                    | `std::map<K, V>`               | `TreeMap<K, V>`                 | N/A                               | `Map`                                 |
| `HashSet<T>` where `T: Eq + Hash`    | Unordered, hash-based set                 | `std::unordered_set<T>`        | `HashSet<T>`                    | `set`                             | `Set`                                 |
| `BTreeSet<T>` where `T: Ord`         | Sorted set                                | N/A                            | `TreeSet<T>`                    | N/A                               | `Set`                                 |

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

### Sorting

#### `sort()`

The `sort()` method sorts a mutable slice or vector in ascending order using the default ordering of its elements. For example:

```rs
fn main() {
    let mut numbers = vec![3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5];
    numbers.sort();

    println!("{:?}", numbers);
}
```

#### `sort_by()`

The `sort_by()` method sorts a mutable slice or vector using a custom ordering defined by a closure. The closure should take two arguments (the elements to compare) and return an Ordering value. For example:

```rs
fn main() {
    let mut strings = vec!["rust", "is", "fun", "to", "learn"];
    strings.sort_by(|a, b| a.len().cmp(&b.len()));

    println!("{:?}", strings);
}
```

#### sort_by_key(key)

The `sort_by_key()` method is used to sort a mutable slice or vector based on a key function that is applied to each element. The key function should take an element from the slice or vector as an argument and return a value that can be compared using Rust's default ordering.

```rs
fn main() {
    let mut words = vec!["apple", "banana", "cherry", "date", "elderberry"];
    words.sort_by_key(|w| w.len());

    println!("{:?}", words);
}
```

#### `sort_unstable()` and `sort_unstable_by()`

The `sort_unstable()` and `sort_unstable_by()` methods work the same as `sort()` and `sort_by()`, but do not guarantee stability. This means that elements that compare equal may not be in the same order after sorting.

#### reverse()

In Rust, the `reverse()` method is used to reverse the order of the elements in a mutable slice or vector.

```rs
fn main() {
    let mut numbers = vec![1, 2, 3, 4, 5];
    numbers.reverse();

    println!("{:?}", numbers);
}
```

### Searching

#### `binary_search()`

The `binary_search()` method searches a sorted slice or vector for a given element using a binary search algorithm. The method returns a `Result` enum that indicates whether the element was found and at which index.

For example:

```rs
fn main() {
    let numbers = vec![1, 3, 4, 5, 7, 9];
    let index = numbers.binary_search(&5);

    match index {
        Ok(i) => println!("Found 5 at index {}", i),
        Err(_) => println!("5 not found"),
    }
}
```

If the element is not found, `binary_search()` returns an `Err` variant.

#### `binary_search_by()`

The `binary_search_by()` method is used to search a sorted slice or vector for a value using a binary search algorithm. The method takes a closure that compares the search value with an element of the slice or vector, and returns an `Ordering` value that indicates whether the search value is less than, equal to, or greater than the element.

```rs
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];
    let search_value = 3;

    let result = numbers.binary_search_by(|&x| x.cmp(&search_value));

    match result {
        Ok(index) => println!("Found {} at index {}", search_value, index),
        Err(_) => println!("{} not found", search_value),
    }
}
```

Note that the `binary_search_by()` method assumes that the slice or vector is sorted in ascending order according to Rust's default ordering. If the slice or vector is sorted in a different order, you can pass a custom comparison function to the method using the `binary_search_by_key()` method instead.

#### `binary_search_by_key()`

the `binary_search_by_key()` method is used to search a sorted slice or vector for a value using a binary search algorithm based on a key derived from each element of the slice or vector. The method takes a closure that maps an element of the slice or vector to its key, and a search key, and returns an `Ordering` value that indicates whether the search key is less than, equal to, or greater than the key of the element.

```rs
fn main() {
    let words = vec!["apple", "banana", "cherry", "date", "elderberry"];
    let search_key = "cherry";

    let result = words.binary_search_by_key(&search_key, |word| word);

    match result {
        Ok(index) => println!("Found {} at index {}", search_key, index),
        Err(_) => println!("{} not found", search_key),
    }
}
```

#### `contains()`

The `contains()` method checks if a slice or vector contains a given element. The method returns a boolean value.

For example:

```rs
fn main() {
    let numbers = vec![1, 3, 4, 5, 7, 9];
    let has_five = numbers.contains(&5);

    if has_five {
        println!("The vector contains 5");
    } else {
        println!("The vector does not contain 5");
    }
}
```

### Comparing Slices

In Rust, slices can be compared using the `==` operator, which compares the elements of the slices for equality. Slices are considered equal if they have the same length and their corresponding elements are equal according to the `PartialEq` trait, which is implemented by default for most built-in types and can be implemented for custom types.

Here is an example that demonstrates how to compare two slices of integers for equality:

```rs
fn main() {
    let a = &[1, 2, 3];
    let b = &[1, 2, 3];
    let c = &[1, 2, 4];

    println!("a == b: {}", a == b);
    println!("a == c: {}", a == c);
}
```

Note that the `==` operator compares the contents of the slices, but not their addresses in memory. Two slices containing the same elements but allocated in different memory locations will still compare as equal.

Also note that the `==` operator compares the entire slice, including any unused elements beyond the end of the slice. If you only want to compare a portion of the slice, you can use the `[..]` syntax to create a sub-slice and compare it instead.

#### starts_with()

The `starts_with()` method is used to check if a slice starts with another slice. The method takes a reference to another slice as an argument, and returns a boolean value indicating whether the first slice starts with the same elements as the second slice.

```rs
fn main() {
    let a = &[1, 2, 3, 4, 5];
    let b = &[1, 2, 3];

    println!("a starts with b: {}", a.starts_with(b));
}
```

Note that the `starts_with()` method compares only the first few elements of the slice, up to the length of the second slice. If the second slice is longer than the first slice, the method will always return `false`. Also note that the `starts_with()` method only checks for equality of the elements in the two slices, and does not check if they have the same addresses in memory.

#### ends_with()

The `ends_with()` method is used to check if a slice ends with another slice.

```rs
fn main() {
    let a = &[1, 2, 3, 4, 5];
    let b = &[3, 4, 5];

    println!("a ends with b: {}", a.ends_with(b));
}
```

### Random Elements

In Rust, the standard library provides the `rand` crate which can be used to generate random numbers and random elements from a slice. To use the `rand` crate, you need to add it to your project's dependencies in `Cargo.toml`.

Here is an example that demonstrates how to use the `rand` crate to generate a random element from a slice of integers:

```rs
use rand::prelude::SliceRandom;

fn main() {
    let slice = &[1, 2, 3, 4, 5];

    let mut rng = rand::thread_rng();
    let random_element = slice.choose(&mut rng).unwrap();

    println!("Random element: {}", random_element);
}
```

In this example, a slice `slice` containing the elements `[1, 2, 3, 4, 5]` is used to generate a random element using the `choose()` method from the `rand` crate. The `choose()` method returns an `Option` containing a reference to a randomly selected element from the slice. In this example, the `unwrap()` method is called on the `Option` to retrieve the randomly selected element.

Note that the `choose()` method may return `None` if the slice is empty. Also note that the `rand` crate provides other methods for generating random numbers and selecting random elements from slices, such as `gen_range()` and `shuffle()`.

The `shuffle()` method is used to randomly shuffle the elements of a mutable slice. The method shuffles the elements in place, so the order of the elements in the original slice is modified.

```rs
use rand::seq::SliceRandom;

fn main() {
    let slice = &mut [1, 2, 3, 4, 5];

    let mut rng = rand::thread_rng();
    slice.shuffle(&mut rng);

    println!("Shuffled slice: {:?}", slice);
}
```

The `gen_range()` method is used to generate a random integer within a given range. The method takes two arguments: a lower bound and an upper bound for the range, and returns a randomly generated integer between these bounds.

```rs
use rand::Rng;

fn main() {
    let mut rng = rand::thread_rng();
    let random_number = rng.gen_range(1..=100);

    println!("Random number: {}", random_number);
}
```

Note that the `gen_range()` method can also take a range of floating-point numbers as an argument, and can generate random numbers of different types, such as `u8`, `i16`, and `f32`.

## `VecDeque<T>`

In Rust, `VecDeque<T>` is a double-ended queue (deque) that provides constant time insertion and removal of elements from both ends of the queue. It is implemented as a dynamic array of chunks, where each chunk is a fixed size array that holds a portion of the deque's elements.

Here is an example that demonstrates how to use `VecDeque<T>` to create a deque, add elements to both ends of the deque, and remove elements from both ends of the deque:

```rs
use std::collections::VecDeque;

fn main() {
    // create a new deque
    let mut deque: VecDeque<i32> = VecDeque::new();

    // add elements to the front of the deque
    deque.push_front(1);
    deque.push_front(2);
    deque.push_front(3);

    // add elements to the back of the deque
    deque.push_back(4);
    deque.push_back(5);
    deque.push_back(6);

    // remove elements from the front of the deque
    let front = deque.pop_front();
    let second = deque.pop_front();
    let third = deque.pop_front();

    // remove elements from the back of the deque
    let back = deque.pop_back();
    let second_to_last = deque.pop_back();
    let third_to_last = deque.pop_back();

    // print the deque
    println!("Deque: {:?}", deque);
}
```

Output:

```css
deque: VecDeque([4, 5]);
```

In this example, a new `VecDeque<i32>` is created using the `new()` method from the `std::collections` module. Elements are added to the front of the deque using the `push_front()` method, and elements are added to the back of the deque using the `push_back()` method.

Elements are removed from the front of the deque using the `pop_front()` method, and elements are removed from the back of the deque using the `pop_back()` method. The `pop_front()` and `pop_back()` methods return an `Option<T>` that contains the removed element, or `None` if the deque is empty.

Note that `VecDeque<T>` provides several other methods for working with deques, such as `get()`, `get_mut()`, `split_off()`, and `rotate_left()` and `rotate_right()`, which allow you to access, modify, split, and rotate the elements of the deque.

## `BinaryHeap<T>`

In Rust, `BinaryHeap<T>` is a priority queue that stores elements in a binary heap data structure. A binary heap is a complete binary tree where each node's value is greater than or equal to its children's values (for a max heap), or less than or equal to its children's values (for a min heap).

`BinaryHeap<T>` provides a simple interface for inserting elements and removing the top element (the maximum or minimum, depending on the type of heap), while maintaining the heap property. It also provides methods for inspecting the top element and checking whether the heap is empty.

Here is an example that demonstrates how to use `BinaryHeap<T>` to create a max heap, insert elements into the heap, remove the maximum element from the heap, and iterate over the elements in the heap:

```rs
use std::collections::BinaryHeap;

fn main() {
    // create a new max heap
    let mut heap = BinaryHeap::new();

    // insert elements into the heap
    heap.push(4);
    heap.push(1);
    heap.push(7);
    heap.push(3);
    heap.push(9);
    heap.push(2);

    // remove the maximum element from the heap
    let max = heap.pop();

    // iterate over the elements in the heap
    while let Some(element) = heap.pop() {
        println!("Element: {}", element);
    }
}
```

Output:

```yaml
Element: 7
Element: 4
Element: 3
Element: 2
Element: 1
```

In this example, a new `BinaryHeap<i32>` is created using the `new()` method from the `std::collections` module. Elements are inserted into the heap using the `push()` method, and the maximum element is removed from the heap using the `pop()` method, which returns an `Option<T>` that contains the maximum element, or `None` if the heap is empty.

To iterate over the elements in the heap, the `pop()` method is called in a loop until it returns `None`, and each element is printed to the console.

Note that `BinaryHeap<T>` provides several other methods for working with heaps, such as `peek()`, `is_empty()`, `len()`, and `clear()`, which allow you to inspect, check, and modify the elements of the heap.

`BinaryHeap<T>` can hold any type of value that implements the `Ord` trait, which is a built-in trait in Rust that defines a total order on values.

```rs
use std::collections::BinaryHeap;
use std::cmp::Ordering;

#[derive(Debug, Eq)]
struct Person {
    name: String,
    age: u32,
}

impl Ord for Person {
    fn cmp(&self, other: &Self) -> Ordering {
        self.age.cmp(&other.age)
    }
}

impl PartialOrd for Person {
    fn partial_cmp(&self, other: &Self) -> Option<Ordering> {
        Some(self.cmp(other))
    }
}

impl PartialEq for Person {
    fn eq(&self, other: &Self) -> bool {
        self.age == other.age
    }
}

fn main() {
    // create a new max heap of Persons
    let mut heap = BinaryHeap::new();

    // insert persons into the heap
    heap.push(Person { name: "Alice".to_string(), age: 35 });
    heap.push(Person { name: "Bob".to_string(), age: 28 });
    heap.push(Person { name: "Charlie".to_string(), age: 42 });

    // remove the oldest person from the heap
    let oldest = heap.pop();

    println!("Oldest person: {:?}", oldest);
}
```

Note that `BinaryHeap<T>` can also be used with custom comparator functions, which allows for more flexibility in how elements are ordered within the heap.

```rs
use std::collections::BinaryHeap;

#[derive(Debug, PartialEq, Eq, PartialOrd, Ord)]
struct Person {
    name: String,
    age: u32,
}

fn main() {
    // create a new binary heap with a custom comparison function
    let heap = BinaryHeap::from(vec![
        Person {
            name: "Alice".to_string(),
            age: 35,
        },
        Person {
            name: "Bob".to_string(),
            age: 28,
        },
        Person {
            name: "Charlie".to_string(),
            age: 42,
        },
    ]);

    // define a custom comparison function that orders persons by name
    let name_comparator = |a: &Person, b: &Person| a.name.cmp(&b.name);

    // convert the binary heap into a vector sorted by name
    // obtain the elements of the heap in sorted order using the IntoIter iterator
    let mut sorted_by_name: Vec<Person> = heap.into_iter().collect();

    // sort the elements of the vector using the name_comparator function
    sorted_by_name.sort_by(name_comparator);

    println!("Persons sorted by name: {:?}", sorted_by_name);
}
```

Note that when defining a custom comparison function, it's important to ensure that the function defines a strict weak ordering on the elements. In other words, the comparison function must satisfy the following properties:

- Reflexivity: The relation must be be reflexive: `cmp(a, a) == Ordering::Equal` for all elements `a`.
- Transitivity: The relation must be be transitive: if `cmp(a, b) == Ordering::Les`s and `cmp(b, c) == Ordering::Less`, then `cmp(a, c) == Ordering::Less`.
- Anti-symmetry: The relation must be be anti-symmetric: if `cmp(a, b) == Ordering::Less`, then `cmp(b, a) == Ordering::Greater`.
- Totality: The relation must be be total: `cmp(a, b)` must return `Ordering::Less`, `Ordering::Equal`, or `Ordering::Greater` for any two elements `a` and `b`.

Reflexivity is a property of a relation that says that for any element A, the relation holds true between A and A. In the context of a comparison function, this means that the function must return Ordering::Equal when comparing the same element to itself.

Transitivity is a property of a relation that says that if the relation holds between A and B, and between B and C, then it must also hold between A and C. In the context of a comparison function, this means that if the function returns Ordering::Less when comparing A to B, and returns Ordering::Less when comparing B to C, then it must return Ordering::Less when comparing A to C.

Anti-symmetry is a property of a relation that says that if the relation holds between A and B, then it must not hold between B and A. In the context of a comparison function, this means that if the function returns Ordering::Less when comparing A to B, then it must return Ordering::Greater when comparing B to A.

Totality is a property of a relation that says that for any two elements A and B, the relation must hold true in one of three ways: A is less than B, A is equal to B, or A is greater than B. In the context of a comparison function, this means that the function must return Ordering::Less, Ordering::Equal, or Ordering::Greater when comparing any two elements.

In the context of a BinaryHeap, these properties are important because the heap relies on the comparison function to determine the order of its elements. If the function doesn't satisfy these properties, the heap may not behave correctly, leading to incorrect results or even runtime errors.

## HashMap<K, V>

`HashMap` is a data structure in Rust that is used to store key-value pairs. It is useful in situations where you need to quickly access data based on a specific key. HashMaps are commonly used in computer science and programming for tasks such as indexing and searching data.

### Real-world examples

- A phone book application that uses HashMap to store contact information, where each contact name is the key and the phone number is the value.
- A website that uses HashMap to store user information, where the user ID is the key and the user's information (such as name, email address, and password) is the value.
- A game that uses HashMap to store player information, where the player's name is the key and the player's score is the value.

### Basic Syntax and Usage

#### Creating a new HashMap

```rs
use std::collections::HashMap;

let mut my_map = HashMap::new();
```

#### Adding elements to a HashMap

```rs
my_map.insert("key1", "value1");
my_map.insert("key2", "value2");
```

#### Accessing an element in a HashMap

```rs
if let Some(value) = my_map.get("key1") {
    println!("{}", value);
}
```

#### Modifying an element in a HashMap

```rs
my_map.insert("key1", "new_value1");
```

#### Removing an element from a HashMap

```rs
my_map.remove("key1");
```

### Advanced Features and Methods

#### Iterating over keys, values, or both

```rs
for key in map.keys() {
    println!("Key: {}", key);
}

for value in map.values() {
    println!("Value: {}", value);
}

for (key, value) in map.iter() {
    println!("Key: {}, Value: {}", key, value);
}
```

#### Combining HashMaps

```rs
let mut map1 = HashMap::new();
map1.insert("key1", "value1");

let mut map2 = HashMap::new();
map2.insert("key2", "value2");

map1.extend(map2);
```

#### Updating values using closure

```rs
map.entry("key").and_modify(|value| *value = "new value");
```

#### Using custom hash functions

```rs
use std::hash::{Hash, Hasher};

struct CustomHasher(u64);

impl Hasher for CustomHasher {
    fn write(&mut self, bytes: &[u8]) {
        // Custom hash function logic
    }

    fn finish(&self) -> u64 {
        self.0
    }
}

impl Hash for CustomHasher {
    fn hash<H: Hasher>(&self, state: &mut H) {
        self.0.hash(state);
    }
}

let mut map: HashMap<i32, i32, CustomHasher> = HashMap::with_hasher(CustomHasher(0));
```

### Most common HashMap methods

| Method                                                               | Description                                                                                                      |
| -------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `new()`                                                              | Creates a new, empty HashMap                                                                                     |
| `with_capacity(capacity: usize)`                                     | Creates a new HashMap with the specified initial capacity                                                        |
| `insert(key: K, value: V)`                                           | Inserts a key-value pair into the HashMap                                                                        |
| `get(&key: &K) -> Option<&V>`                                        | Returns a reference to the value associated with the given key, or None if the key is not in the HashMap         |
| `get_mut(&key: &K) -> Option<&mut V>`                                | Returns a mutable reference to the value associated with the given key, or None if the key is not in the HashMap |
| `remove(&key: &K) -> Option<V>`                                      | Removes and returns the value associated with the given key, or None if the key is not in the HashMap            |
| `contains_key(&key: &K) -> bool`                                     | Returns true if the HashMap contains the given key                                                               |
| `len() -> usize`                                                     | Returns the number of elements in the HashMap                                                                    |
| `is_empty() -> bool`                                                 | Returns true if the HashMap is empty                                                                             |
| `clear()`                                                            | Removes all elements from the HashMap                                                                            |
| `iter() -> Iter<'_, K, V>`                                           | Returns an iterator over the key-value pairs in the HashMap                                                      |
| `iter_mut() -> IterMut<'_, K, V>`                                    | Returns a mutable iterator over the key-value pairs in the HashMap                                               |
| `keys() -> Keys<'_, K, V>`                                           | Returns an iterator over the keys in the HashMap                                                                 |
| `values() -> Values<'_, K, V>`                                       | Returns an iterator over the values in the HashMap                                                               |
| `entry(&key: K) -> Entry<'_, K, V>`                                  | Returns an Entry for the given key, allowing for more complex operations on the HashMap                          |
| `reserve(&additional: usize)`                                        | Reserves capacity for at least `additional` more elements to be inserted in the HashMap                          |
| `drain() -> Drain<'_, K, V>`                                         | Removes all elements from the HashMap and returns an iterator over the removed key-value pairs                   |
| `retain(predicate: impl FnMut(&K, &mut V) -> bool)`                  | Retains only the key-value pairs that satisfy the given predicate                                                |
| `merge(key: K, value: V, f: impl FnOnce(V, V) -> V)`                 | Inserts the given key-value pair into the HashMap or updates the existing value using the given closure          |
| `from_iter(iter: impl IntoIterator<Item = (K, V)>) -> HashMap<K, V>` | Creates a new HashMap from the given iterator of key-value pairs                                                 |

### Entries

The `entry()` method is a feature of Rust's `HashMap` data structure that provides a way to access an entry in the map and modify it if it exists or insert a new entry if it does not. This method returns an Entry enum that represents either an existing or a new entry in the map.

The `entry()` method can be very useful in situations where you need to access or modify entries in a HashMap in a single expression.

```rs
use std::collections::HashMap;
fn main() {
    let mut map = HashMap::new();

    // Insert a new entry
    let entry = map.entry("foo").or_insert(1);
    assert_eq!(*entry, 1);

    // Modify an existing entry
    *map.entry("foo").or_insert(2) += 1;
    assert_eq!(map.get("foo"), Some(&2));

    // Insert a new entry with a default value
    let entry = map.entry("bar").or_insert_with(|| 0);

    // Modify an existing entry with a closure
    map.entry("bar").and_modify(|v| *v = 2);
}
```

### Best Practices

- Use immutable references when possible. This helps avoid unintended changes to the `HashMap`.
- Avoid unnecessary clones. Cloning a `HashMap` can be expensive, so it's best to avoid doing so unless it's absolutely necessary. Instead, use references to access the data in the `HashMap`.

- Choose appropriate key types. Keys in a `HashMap` should be hashable and implement the `Eq` and `PartialEq` traits.
- Use the entry method when adding or updating elements in a `HashMap`. The entry method provides a way to access the value of a key-value pair in a `HashMap`, and allows you to insert or modify the value if it doesn't exist.
- Use the `get_or_insert` method when accessing or inserting elements in a `HashMap`. The `get_or_insert` method provides a way to access the value of a key-value pair in a `HashMap`, and allows you to insert the key-value pair if it doesn't exist.

## `BTreeMap<K, V>`

BTreeMap is a sorted map data structure in Rust that is based on a B-tree. It stores key-value pairs in sorted order based on the keys. It is useful in situations where you need to quickly find the value associated with a key, and also need the keys to be sorted.

One advantage of using BTreeMap is that it provides fast lookups, inserts, and deletions even when the number of elements is large. It has a logarithmic time complexity for these operations, which means the time it takes to perform these operations increases only logarithmically with the number of elements in the map. Additionally, BTreeMap is sorted, which allows you to iterate through the elements in order.

One disadvantage of using BTreeMap is that it has a higher memory overhead than other data structures like HashMaps, due to the structure of the B-tree. Another disadvantage is that it may be slower than HashMaps for small numbers of elements, due to the overhead of maintaining the B-tree structure.

### Syntax and Usage

```rs
use std::collections::BTreeMap;
let mut btree_map = BTreeMap::new();
btree_map.insert("a", 1);
btree_map.insert("b", 2);
btree_map.insert("c", 3);
if let Some(value) = btree_map.get("a") {
    println!("value for key 'a' is {}", value);
}
btree_map.insert("a", 4);
btree_map.remove("a");
```

### Practical Examples and Scenarios

BTreeMap is useful in situations where you need to maintain a collection of key-value pairs in sorted order, such as in a dictionary or a database index. It can also be used for tasks such as finding the nearest neighbor of a point in a multi-dimensional space.

### Performance Considerations

When using BTreeMap, it is important to consider the tradeoff between memory usage and performance. Because of the B-tree structure, BTreeMap has a higher memory overhead than other data structures like HashMaps. Additionally, BTreeMap may be slower than HashMaps for small numbers of elements, due to the overhead of maintaining the B-tree structure.
