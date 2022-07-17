## ops 模块是一些运算符的重载功能的集合

## `Example`
```rust
use std::ops::{Add, Sub};

#[derive(Debug, Copy, Clone, PartialEq)]
struct Point {
    x: i32,
    y: i32,
}

impl Add for Point {
    type Output = Self;

    fn add(self, other: Self) -> Self {
        Self {x: self.x + other.x, y: self.y + other.y}
    }
}

impl Sub for Point {
    type Output = Self;

    fn sub(self, other: Self) -> Self {
        Self {x: self.x - other.x, y: self.y - other.y}
    }
}

fn main() {
    let p1 = Point { x: 1, y: 10 };
    let p2 = Point { x: 20, y: 40 };

    println!("{:#?}", p1 - p2);
}
```

输出结果：
```bash
    Finished dev [unoptimized + debuginfo] target(s) in 0.08s
     Running `target\debug\grrs.exe`
Point {
    x: -19,
    y: -30,
}
```