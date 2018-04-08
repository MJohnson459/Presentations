## Rust for newbs

```rust
fn main() {
  println!("Hello, SeeByte!");
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

---

## Semantics overview



### Struct

```rust
struct Point {
  x: u32,
  y: u32,
}
```



### functions

```rust
impl Point {
  fn length(self&) -> f32 {
    ((self.x*self.x + self.y*self.y) as f32).sqrt()
  }
}
```

Note:
no trailing `;`
explicit casting



### traits

```rust
trait Coordinate {
  fn distance(self&) -> f32;
}
```

```rust
impl Coordinate for Point {
  fn distance(self&) -> f32 {
    self.distance()
  }
}
```



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
Not tied to base type
Can wrap a type



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



### Option and Result enums

```rust
fn divide(numerator: f64, denominator: f64) -> Option<f64> {
  if denominator == 0.0 {
    None
  } else {
    Some(numerator / denominator)
  }
}

fn double_number(number_str: &str) -> Result<i32, ParseIntError> {
  match number_str.parse::<i32>() {
    Ok(n) => Ok(2 * n),
    Err(err) => Err(err),
  }
}
```



### Option and Result enums

```rust
fn main() {

  match divide(1.1, 4.5) {
    Some(x) => println!("x = {}", x),
    None => println!("Unable to divide"),
  }

  match double_number("10") {
    Ok(n) => assert_eq!(n, 20),
    Err(err) => println!("Error: {:?}", err),
  }
}
```

---

## Why is Rust interesting?



### Tooling

- Cargo: a well integrated package manager
- rustup: a tool for managing the release channel for the `rustc` compiler



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
feature tags allow compile time branching (i.e. selective compilation).
All versions must be sematically versioned major.minor.patch



### Iterators

```rust
let numbers = vec![1, 2, 3, 4, 5];
for x in numbers.iter() {
  println!("x = {}", x);
}
```

```
x = 1
x = 2
x = 3
x = 4
x = 5
```



### Parallel iterators with rayon

```rust
use rayon::prelude::*;

let numbers = vec![1, 2, 3, 4, 5];
numbers.par_iter().for_each(|x| {
    println!("x = {}", x);
});
```



### Macros in Rocket

A simple routing/web framework that can create REST API's.

```rust
use rocket::*;

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



### Macros in serde

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
