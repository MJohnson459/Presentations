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

`Rust is a systems programming language that runs blazingly fast, prevents segfaults, and guarantees thread safety.` - Unbiased rust website

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

Types

---

Struct

```rust
struct Point {
  x: u32,
  y: u32,
}
```

functions

```rust
impl Point {
  fn length(self&) -> f32 {
    ((self.x*self.x + self.y*self.y) as f32).sqrt()  // note no trailing ;
  }
}
```

traits



---

Enum <- the cool type

```rust
enum Methods {
  Delete,
  Get,
  Post(Body),
  Put(Body),
}
```

---

Matching

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

---

## Why is rust interesting

---

## Should you use rust?

---

## Summary

---

## Questions
