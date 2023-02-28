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
