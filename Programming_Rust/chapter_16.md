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
