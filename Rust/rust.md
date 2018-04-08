## Rust for newbs

```rust
fn main() {
  println!("Welcome!");
}
```

---

## Agenda

- What is rust?
- Why is rust interesting?
- Should you use rust?

---

## What is rust

> Rust is a systems programming language that runs blazingly fast, prevents segfaults, and guarantees thread safety.

\- unbiased website



### Features
- zero-cost abstractions
- move semantics
- guaranteed memory safety
- threads without data races
- trait-based generics
- pattern matching
- type inference
- minimal runtime
- efficient C bindings

Note:
Systems level programming language
Similar to C++ with modern functional features added

How easy can you make correct code while making it very hard to write incorrect code


---

## Semantics overview

Note:
A quick run through of what it looks like



### Variables

```rust
let x: i32 = 5;
let mut y = 5.6;
let z = &y;
```

Note:
- Strongly typed language but will try to deduce types (auto, var, etc)
- Can manually specify the type for more clarity. Be a good programmer and make it readable!
- immutable by default!
- references but lifetimes are tracked "borrow"



### Struct

```rust
struct Point {
  x: i32,
  y: i32,
}
```

Note:
- No classes, only structs.
- Structs are basically the data portion of a class (POD)



### functions

```rust
impl Point {
  fn length(self&) -> f32 {
    ((self.x*self.x + self.y*self.y) as f32).sqrt()
  }
}
```

Note:
- Implement functions for structs.
- No default functions. None. No constructors etc.
- Return is done by not having a trailing `;`
  - Takes some getting used to but is a good way to highlight bad code
- (optional) explicit casting



### traits

```rust
trait Coordinate {
  fn distance(self&) -> f32;
}

impl Coordinate for Point {
  fn distance(self&) -> f32 {
    self.distance()
  }
}
```

Note:
- traits allow you to define interfaces
- there are traits for common operations such as Add, Subtract, Debug, etc



### Macros

```rust
#[derive(Debug)]
struct Point {
  x: i32,
  y: i32,
}

let p = Point{x: 4, y: 2};
println!("Point = {:?}", p);
// prints "Point = Point { x: 4, y: 2 }"
```

Note:
- Automatically implements the Debug trait for the struct
- Good libraries commonly supply macros to make the API cleaner



### Enum "the cool type"

```rust
enum Methods {
  Delete,
  Get,
  Post(Body),
  Put(Body),
}
```

Note:
- Not tied to native type similar to C++11 `enum class`
- Can wrap a type (will come back to later with Options and Results)



### Matching

```rust
let number = 5;
let size = match number {
  0 => "none",
  2 | 3 => "tiny",
  4...7 => "small",
  8...20 => "medium",
  _ => "large"
};

println!("size = {}", size); // "size = small"
```

Note:
- Can match against patterns
- Must be exhaustive. Can't just ignore values.
- Can "destructure" structs and types



### Option enum

```rust
fn divide(numerator: f64, denominator: f64) -> Option<f64> {
  if denominator == 0.0 {
    None
  } else {
    Some(numerator / denominator)
  }
}

fn main() {
  match divide(1.1, 4.5) {
    Some(x) => println!("x = {}", x),
    None => println!("Unable to divide"),
  }
}

```

Note:
- Used as a return when the function might not produce a valid value
- Completely removes the need for `Null` type
- Option is generic "templated" on the Some(T) type



### Result enum

```rust
fn double_number(number_str: &str) -> Result<i32, ParseIntError> {
  match number_str.parse::<i32>() {
    Ok(n) => Ok(2 * n),
    Err(err) => Err(err),
  }
}

fn main() {
  match double_number("10") {
    Ok(n) => assert_eq!(n, 20),
    Err(err) => println!("Error: {:?}", err),
  }
}
```

Note:
- Like `Option` but returns an error if a value can't be produced
- Main method of propogating errors
- ? operators to make them less verbose when needed


---

## Why is Rust interesting?



### Tooling

- Cargo: a well integrated package manager
- rustup: a tool for managing the release channel for the `rustc` compiler
- rustc: the compiler. amazing errors. the best errors.



### Cargo

```toml
[package]
name = "marvin"
version = "0.1.0"
authors = ["Michael Johnson <mjohnson459@gmail.com>"]

[features]
logging = []

[dependencies]
chrono = { version="", features = ["serde"] }
diesel = { version = "1.1", features = ["postgres", "serde_json", "chrono"] }
diesel_full_text_search = "1.0"
serde = "1.0"
rocket = "0.3.6"
r2d2 = "0.8"

[build-dependencies]
diesel_migrations = { version = "1.1", features = ["postgres"] }
```

Note:
- toml file is a standardised version of .ini file
- dependencies are downloaded and managed by cargo
- all dependencies are compiled on your system!
- feature tags allow compile time branching (i.e. selective compilation).
- all dependencies must be sematically versioned major.minor.patch
- can push to cargo.io central crate repo (or set up a local repository)



### Parallel iterators with Rayon

```rust
let numbers = vec![1, 2, 3, 4, 5];

// procedural style
for x in numbers.iter() {
  println!("x = {}", x);
}

// functional style
numbers.iter().for_each(|x| println!("x = {}", x));

// parallel for loop
extern crate rayon;
use rayon::prelude::*;
numbers.par_iter().for_each(|x| println!("x = {}", x));
```

Note:
- Some intesting libraries made possible by cargo
- like C++, most of the standard library is based on iterators.
- more syntactically pleasing in rust
- rust libraries can add new traits to old types!
- Rayon adds `par_iter()` for multi-threaded "embarrasingly parallel" tasks



### Macros with Rocket

A simple routing/web framework that can create REST API's.

```rust
extern crate rocket;

#[get("/hello/<name>/<age>")]
fn hello(name: String, age: u8) -> String {
    format!("Hello, {} year old named {}!", age, name)
}

fn main() {
    rocket::ignite()
        .mount("/", routes![hello])
        .launch();
}
```

Note:
- looks like pythons decorators
- best named factory functions ever



### Macros with serde

```rust
extern crate serde;
extern crate serde_json;
// could equally use xml, yaml, toml, etc

#[derive(Serialize, Deserialize, Debug)]
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let point = Point { x: 1, y: 2 };

    // Convert the Point to a JSON string.
    let serialized = serde_json::to_string(&point).unwrap();

    // Prints serialized = {"x":1,"y":2}
    println!("serialized = {}", serialized);

    // Convert the JSON string back to a Point.
    let deserialized: Point = serde_json::from_str(&serialized).unwrap();

    // Prints deserialized = Point { x: 1, y: 2 }
    println!("deserialized = {:?}", deserialized);
}
```

Note:
- very simple way to serialize and deserialize structs
- can be used to support multiple file formats



### Compiler errors

Live demonstration time!

---

## Should you use rust?



# Duh



### Seriously though

Do you...
- get annoyed at the pace of change with C++?
- disagree that learning a language means remembering all of the quirky ways it tries to break your code?
- like C++ but really want a good package manager?
- like dynamic languages but wish they ran faster and had less bugs?



### Cons

- When you start using macros, the error messages can look like the C++ template mess
- If you haven't encounted functional programming before you might find it frustrating to learn Rust
- Writing C++/Java code in Rust just doesn't work properly
- While there are a ton of libraries out there, they are in varying degrees of completion



### Pros

- There are a ton of libraries out there, waiting to be completed by adventuring programmers!
- Excellent tooling makes getting started relatively painless
- Documentation is fantastic
- Syntax should be fairly familiar to most people being a mix of C style conventions and functional
- Immutable by default and variable lifetime management "force" you to write better code
  - There are numerous lessons that will make your code better in any language


---

# Questions

---

# Links

- https://doc.rust-lang.org/book/second-edition/
- https://doc.rust-lang.org/rust-by-example/
- https://doc.rust-lang.org/reference/
