## Rust for newbs

```rust
fn main() {
  println!("Hello, SeeByte!");
}
```

---

## Agenda!

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

## Quick overview



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

Note: no trailing ;



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

---

## Why is rust interesting



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
```



### ...to rival Erlang
```rust
let pair = (4, 5);

match pair {
  (0, 0) => println!("Origin"),
  (0, y) => println!("Y-axis, coordinate {}", y),
  (x, 0) => println!("X-axis, coordinate {}", x),
  (x, y) => {
    let distance = ((x*x + y*y) as f32).sqrt();
    println!("({}, {}), and {} units from origin", x, y, distance);
  }
};

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



### Iterators

```rust
let numbers = vec![1, 2, 3, 4, 5];
for x in &numbers {
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



### Functional style
```rust
let numbers = vec![1, 2, 3, 4, 5];
numbers.iter().enumerate().for_each(|(i, x)| {
    println!("numbers[{}] = {}", i, x);
});
```

```
numbers[0] = 1
numbers[1] = 2
numbers[2] = 3
numbers[3] = 4
numbers[4] = 5
```



### Parallel with ease

```rust
let numbers = vec![1, 2, 3, 4, 5];
numbers.par_iter().for_each(|x| {
    println!("x = {}", x);
});
```

---

## Libraries

What already exists?



- Lots of web type tools
  - Rocket: generic web framework and routing tools
  - serde: Generic **ser**ialisation and **de**serialisation library
  - diesel: Powerful ORM for databases (integrates well with the two above)
- Countless other small libraries. Some are amazingly well documented and maintained. Most are small abandonded projects by people learning Rust.


### Rocket

A simple routing/web framework that can create REST API's.

```rust
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



### serde

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

---

## Should you use rust?



# Duh

---

## Summary

---

## Questions
