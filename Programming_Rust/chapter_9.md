# Structs

Structs in Rust programming are data structures that allow you to group together data of different types into a single unit. Structs are similar to classes in object-oriented programming but are more lightweight and focused on data storage rather than behavior.

Rust structs, sometimes called structures, resemble struct types in C and C++, classes in Python, and objects in JavaScript.

To define a struct in Rust, you can use the `struct` keyword followed by the name of the struct, as well as a set of curly braces containing the fields of the struct. For example, here's a simple struct definition for a `Person`:

```rs
struct Person {
    name: String,
    age: u8,
    is_alive: bool,
}
```

You can create an instance of a struct by using the `struct_name { field_name: value }` syntax, like so:

```rs
let person = Person {
    name: String::from("Alice"),
    age: 30,
    is_alive: true,
};
```

A struct expression starts with the type name (`Person`) and lists the name and value of each field, all enclosed in curly braces.

You can access the fields of a struct using dot notation, like so:

```rs
println!("{} is {} years old.", person.name, person.age);
```

This will output: `Alice is 30 years old`.

Structs can also be used in combination with Rust's `impl` keyword to define methods on the struct. For example:

```rs
impl Person {
    fn celebrate_birthday(&mut self) {
        self.age += 1;
    }
}
```

Now we can call the `celebrate_birthday` method on our person instance like so:

```rs
person.celebrate_birthday();
println!("{} is now {} years old.", person.name, person.age);
```

This will output: `Alice is now 31 years old`.

One advantage of using structs in Rust is that they provide a way to group related data together in a way that's easy to manage and manipulate. This can make your code more organized, easier to read, and less error-prone.

However, one limitation of structs is that they don't provide any built-in mechanisms for inheritance or polymorphism. This can make it more difficult to write code that needs to work with different types of data that have similar but not identical fields.

To overcome this limitation, Rust provides traits, which are similar to interfaces in other programming languages. By defining traits and implementing them for different structs, you can create more flexible and reusable code.

Rust has three kinds of struct types, as you mentioned:

1. Named-field structs: These are the most common type of structs in Rust. They consist of a set of named fields, each with their own type. You can create instances of named-field structs by specifying values for each field by name.

1. Tuple-like structs: These structs have a set of fields without names. Instead, the fields are identified by their position. Tuple-like structs can be useful for representing data that has a specific order but doesn't necessarily have named attributes.

1. Unit-like structs: These structs have no fields at all. They're used to represent a value that has no data, but is still meaningful in some way. For example, you might use a unit-like struct to represent the result of a function that doesn't return a value.

## Named-Field Structs

Named-field structs are the most common type of structs in Rust. They consist of a set of named fields, each with its own type. Here's an example:

```rs
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect = Rectangle { width: 30, height: 50 };
    println!("The area of the rectangle is {} square pixels.", rect.width * rect.height);
}
```

Named-field structs can be useful for representing data that has a specific set of attributes, each with a distinct name. By giving each field a name, we can create more readable and self-explanatory code.

It's common to use `CamelCase` naming conventions for struct types. This means that the first letter of each word in the struct name is capitalized, and there are no underscores between words.

```rs
struct Point2D {
    x: f32,
    y: f32,
}
```

It's also common to use singular nouns for struct names, rather than plural nouns. This is because each instance of the struct represents a single thing, rather than a collection of things. For example, we might use `Rectangle` instead of `Rectangles`.

Finally, when choosing names for struct fields, it's common to use descriptive names that make it clear what each field represents.

Here are some best practices for naming struct fields:

1. Use descriptive names: Choose names that make it clear what each field represents. For example, `width` and `height` are more descriptive than `w` and `h`.

1. Use snake_case: Rust's naming convention for variables and functions is to use snake_case, which means that words are separated by underscores. This convention should also be used for struct fields. For example, we might name a field `first_name` rather than `firstName`.

1. Be consistent: Use the same naming conventions for all fields in a struct. This makes the code more readable and easier to understand.

1. Avoid abbreviations: In general, it's better to use full words rather than abbreviations. This makes the code more self-explanatory and easier to read. However, there are some common abbreviations that are widely understood, such as `num` for `number`.

1. Use singular nouns: Structs represent a single thing, so it's common to use singular nouns for struct names and field names. For example, we might `use` user instead of `users`.

## Module-level privacy

In Rust, structs are private by default, which means that they can only be accessed within the same module where they are declared and any nested submodules. This is known as `module-level privacy`.

To make a struct accessible from outside of its module, you need to make it public by using the `pub` keyword. For example, to make a struct named Person public, you would declare it like this:

```rs
pub struct Person {
    name: String,
    age: u32,
}
```

Once a struct is made public, it can be used in other modules and even other crates.

It's also worth noting that individual fields of a struct can have different visibility than the struct itself. This means that you can make some fields public while keeping others private. To do this, you can use the `pub` keyword on individual fields:

```rs
pub struct Person {
    pub name: String,
    age: u32,
}
```

In this example, the `name` field is public while the `age` field is private. This allows you to control the visibility of individual fields within a struct.

When creating a named-field struct value, you can use another struct of the same type to supply values for fields that you omit.

```rs
struct Person {
    name: String,
    age: u32,
}

let alice = Person {
    name: String::from("Alice"),
    age: 30,
};

let bob = Person {
    name: String::from("Bob"),
    ..alice
};
```

In this example, we use `..alice` to indicate that any fields not explicitly mentioned should take their values from the `alice` struct. In other words, `bob` will have the same `name` and `age` as `alice`, except with a different name.

This can be a convenient way to create new struct values that are similar to existing ones, while avoiding repetition of the same field values.

```rs
// Define a struct to represent the sorcerer in the story
struct Sorcerer {
    magic_level: i32, // the sorcerer's level of control over magic
}

// Implement methods on the Sorcerer struct
impl Sorcerer {
    // A method to control the apprentice's magic level
    fn control_apprentice(&self, apprentice: &mut Apprentice) {
        // If the apprentice's magic level is greater than the sorcerer's, he's out of control
        if apprentice.magic_level > self.magic_level {
            println!("The apprentice is out of control!");
            // Reset the apprentice's magic level to be the same as the sorcerer's
            apprentice.magic_level = self.magic_level;
        }
    }
}

// Define a struct to represent the apprentice in the story
struct Apprentice {
    magic_level: i32, // the apprentice's level of skill in magic
}

// Implement methods on the Apprentice struct
impl Apprentice {
    // A method to make the apprentice work and increase his magic level
    fn work(&mut self) {
        // Increase the apprentice's magic level by 10
        self.magic_level += 10;
    }
}

// The main function, where the story unfolds
fn main() {
    // Create a sorcerer with a high level of control over magic
    let sorcerer = Sorcerer { magic_level: 100 };
    // Create an apprentice with a lower level of skill in magic
    let mut apprentice = Apprentice { magic_level: 50 };

    // The apprentice starts to work and his magic level increases
    apprentice.work();

    // The sorcerer checks on the apprentice to make sure he's not out of control
    sorcerer.control_apprentice(&mut apprentice);

    // The apprentice continues to work and increase his magic level, but the sorcerer keeps him in check
    apprentice.work();
    sorcerer.control_apprentice(&mut apprentice);

    // The apprentice works too hard and his magic level exceeds the sorcerer's level of control
    apprentice.work();

    // The sorcerer checks on the apprentice again and resets his magic level
    sorcerer.control_apprentice(&mut apprentice);
}
```

## Tuple-Like Structs

A tuple-like struct is a struct that has unnamed fields, just like a tuple. It is defined using the `struct` keyword, followed by the struct name, and then a tuple of the field types.

The values held by a tuple-like struct are called `elements`.

```rs
struct Point(f32, f32);

fn main() {
    let p = Point(1.0, 2.0);
    println!("The x-coordinate is: {}", p.0);
    println!("The y-coordinate is: {}", p.1);
}
```

However, since the fields are unnamed, we use the position of the field in the tuple to access it, starting from `0`.

## Unit-Like Structs

A unit-like struct is a struct that has no fields, and is used to represent a value that has no meaningful information. It is defined using the struct keyword, followed by the struct name, and then an empty set of curly braces.

```rs
struct EmptyStruct;

fn main() {
    let empty = EmptyStruct;
    println!("This struct is empty!");
}
```

Unit-like structs can be useful in a few cases. For example, if you need to define a type that has no meaningful information, but you want to distinguish it from other types, you can use a unit-like struct. They can also be used to implement traits or derive implementations, without needing to store any actual data in the struct.

## Struct Layout

In Rust, the memory layout of a struct is determined by the layout of its fields. The Rust compiler determines this layout based on the following rules:

- The layout of a struct is determined by the layout of its fields, in the order they are defined. Each field is aligned to its natural alignment, which is determined by its type.

- The size of a struct is the sum of the sizes of its fields, rounded up to the nearest multiple of the largest alignment of any of its fields.

- These rules ensure that the memory layout of a struct is well-defined and predictable, which can be important when working with low-level code or interfacing with other languages.

To see the layout of a struct, you can use the std::mem::size_of function, which returns the size of a value in bytes. Here's an example:

```rs
struct Foo {
    a: u8,
    b: u16,
    c: u32,
}

fn main() {
    println!("The size of Foo is {} bytes", std::mem::size_of::<Foo>());
}
```

The output will depend on the size of the various integer types on your system, but it should be `8` bytes on most systems.

Alignment refers to the positioning of data in memory on boundaries that are multiples of the data's size. In computer memory, data is accessed in blocks of a certain size, called the "word size". The word size is typically either 32 bits (4 bytes) or 64 bits (8 bytes), depending on the architecture of the computer.

When data is stored in memory, it is often stored in units of the word size. For example, a 32-bit integer might be stored in a single 32-bit word, while a 64-bit integer might be stored in two consecutive 32-bit words. However, some types of data have alignment requirements that dictate how they must be stored in memory. For example, a 64-bit integer might need to be stored in a memory location that is aligned on an 8-byte boundary, so that it is accessed efficiently by the CPU.

In Rust, the alignment of a type is determined by its size and type. For example, an `i32` has a size of 4 bytes, and is aligned on a 4-byte boundary. A `u64` has a size of 8 bytes, and is aligned on an 8-byte boundary.

When a struct is defined in Rust, the alignment of the struct is determined by the alignment requirements of its fields. Each field is aligned on its natural alignment boundary, which is determined by the size and type of the field. The largest alignment requirement of any field in the struct determines the alignment of the entire struct.

## Defining Methods with impl

Methods are defined using the impl keyword followed by the name of the struct or enum, and then the body of the method. The impl block can appear anywhere in the code, and can contain multiple methods.

```rs
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect = Rectangle { width: 30, height: 50 };
    let area = rect.area();
    println!("The area of the rectangle is {} square pixels.", area);
}
```

Rust also allows us to define "associated functions" within an `impl` block, which are functions that are associated with the `struct` or `enum`, but don't take a reference to `self` as a parameter. These functions are similar to `static` methods in other languages.

```rs
impl Rectangle {
    fn new(width: u32, height: u32) -> Rectangle {
        Rectangle { width, height }
    }
}

fn main() {
    let rect = Rectangle::new(30, 50);
}
```

This syntax is different from the syntax we used to call the `area` method, because `new` doesn't require an instance of `Rectangle` to be created first. Instead, we call it directly on the `Rectangle` struct, using the double colon syntax.

## Passing Self as a Box, Rc, or Arc

In Rust, it's common to pass ownership of an object from one part of the code to another using a `Box`, `Rc`, or `Arc`. These are all types of "smart pointers" that allow you to manage the lifetime of an object by keeping track of how many references to the object exist at any given time.

When defining a method that takes ownership of self and returns a new instance of the struct, it's possible to use one of these smart pointers to return the new instance. This can be useful when you want to create a new instance of the struct that has a longer lifetime than the original instance.

Here's an example of defining a method that returns a new instance of the struct, using a `Box` to manage ownership:

```rs
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    // define a method named double that takes ownership of self using a Box<Self> parameter.
    fn double(self: Box<Self>) -> Box<Self> {
        // create a new instance of the Rectangle struct with twice the width and height of the original instance,
        // and return it wrapped in a Box.
        Box::new(Rectangle {
            width: self.width * 2,
            height: self.height * 2,
        })
    }
}


fn main() {
    // create an instance of Rectangle and wrap it in a Box
    let rect = Box::new(Rectangle { width: 10, height: 20 });
    // a new Box containing the new instance of Rectangle with double the width and height of the original instance.
    let doubled_rect = rect.double();
    println!("{:?}", doubled_rect);
}
```

We can also use `Rc` or `Arc` instead of `Box` to manage ownership in a similar way.

```rs
use std::rc::Rc;

struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn double(self: Rc<Self>) -> Rc<Self> {
        Rc::new(Rectangle {
            width: self.width * 2,
            height: self.height * 2,
        })
    }
}

fn main() {
    let rect = Rc::new(Rectangle { width: 10, height: 20 });
    let doubled_rect = rect.double();
}
```

Using `Arc` is similar to using `Rc`, but it allows the reference count to be shared across multiple threads.

```rs
use std::sync::Arc;

struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn double(self: Arc<Self>) -> Arc<Self> {
        Arc::new(Rectangle {
            width: self.width * 2,
            height: self.height * 2,
        })
    }
}

fn main() {
    let rect = Arc::new(Rectangle { width: 10, height: 20 });
    let doubled_rect = rect.double();
}
```

Box, Rc, and Arc are smart pointer types in Rust that provide different ownership and borrowing semantics for managing heap-allocated memory.

- `Box<T>`: is a simple pointer type that represents ownership of a heap-allocated T value. Box is used when you need to allocate memory on the heap and manage its lifetime. A common use case is when you have a large data structure that needs to be moved around between functions or stored in a collection.

- `Rc<T>`: is a reference-counted pointer type that represents shared ownership of a heap-allocated T value. Rc is used when you need multiple owners of a value and you don't want to transfer ownership or mutate the value. A common use case is when you have a data structure that needs to be shared between multiple parts of a program, such as a tree or graph data structure.

- `Arc<T>`: is an atomic reference-counted pointer type that represents shared ownership of a heap-allocated T value, similar to `Rc<T>`. However, it is used in a multi-threaded environment where multiple threads need to share ownership of a value. Arc provides thread-safe shared ownership of a value by using atomic operations to increment and decrement the reference count.

1. Using Box:

   ```rs
   struct Node<T> {
       value: T,
       next: Option<Box<Node<T>>>, // allocate memory on the heap for each node.
   }

   let mut head = Node {
       value: 1,
       next: None,
   };

   let tail = Node {
       value: 2,
       next: None,
   };

   head.next = Some(Box::new(tail));
   ```

1. Using Rc:

   ```rs
   use std::rc::Rc;

   struct Node<T> {
       value: T,
       next: Option<Rc<Node<T>>>, // share ownership of each node between multiple parts of the program.
   }

   let mut head = Rc::new(Node {
       value: 1,
       next: None,
   });

   let tail = Rc::new(Node {
       value: 2,
       next: None,
   });

   head.next = Some(tail.clone());
   ```

1. Using Arc:

   ```rs
   use std::sync::Arc;
   use std::thread;

   struct Counter {
       count: Arc<u32>,
   }

   let counter = Arc::new(Counter { count: Arc::new(0) });

   let mut handles = Vec::new();

   for i in 0..10 {
       let counter = counter.clone();
       let handle = thread::spawn(move || {
           *counter.count += 1;
       });
       handles.push(handle);
   }

   for handle in handles {
       handle.join().unwrap();
   }

   println!("Final count: {}", *counter.count);
   ```

   The Arc type ensures thread-safe shared ownership of the counter value.

Here's a table comparing Box, Rc, and Arc in Rust:

|                                      | `Box<T>`                                | `Rc<T>`                                                                  | `Arc<T>`                                                               |
| ------------------------------------ | --------------------------------------- | ------------------------------------------------------------------------ | ---------------------------------------------------------------------- |
| Ownership                            | Unique ownership                        | Shared ownership                                                         | Shared ownership                                                       |
| Thread-safe                          | Yes                                     | No                                                                       | Yes                                                                    |
| Performance                          | Fast and efficient                      | Fast but less efficient                                                  | Slower but more efficient                                              |
| Use case                             | Storing data on the heap                | Sharing immutable data                                                   | Sharing mutable data                                                   |
| Cloning                              | Deep copy of the entire object          | Increment reference count                                                | Increment reference count                                              |
| Dropped when reference count is zero | Yes                                     | Yes                                                                      | Yes                                                                    |
| Usage                                | Suitable for single ownership scenarios | Suitable for multiple ownership scenarios where immutability is required | Suitable for multiple ownership scenarios where mutability is required |

It's important to note that the choice of which to use depends on the specific use case and requirements of the program.
