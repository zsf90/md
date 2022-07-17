`Rust` 是静态类型的编程语言，每个变量在编译时必须直到所有 变量的类型。

## 标量类型

**标量** (scalar) 类型代表一个单独的值。
Rust 有四种基本的标量类型：`整型`、`浮点型`、`布尔类型`和`字符类型`。

### 整型

|长度|有符号|无符号|
|---|---|---|
|8-bit|i8|u8|
|16-bit|i16|u16|
|32-bit|i32|u32|
|64-bit|i64|u64|
|128-bit|i128|u128|
|arch|isize|usize|

`Rust` 中整型字面值：

|数字字面值|例子|
|---|---|
|Decimal (十进制)|98_222|
|Hex (十六进制)|0xff|
|Octal (八进制)|0o77|
|Binary (二进制)|0b1111_0000|
|Byte (单字节字符)(仅限于u8)|b'A'|

### 浮点型
Rust 也有两个原生的 浮点数（floating-point numbers）类型，它们是带小数点的数字。Rust 的浮点数类型是 f32 和 f64，分别占 32 位和 64 位。默认类型是 f64，因为在现代 CPU 中，它与 f32 速度几乎一样，不过精度更高。所有的浮点型都是有符号的。

例子：
```rust
fn main() {
    let x = 2.0; // f64

    let y: f32 = 3.0; // f32
}
```

### 布尔类型
正如其他大部分编程语言一样，Rust 中的布尔类型有两个可能的值：true 和 false。Rust 中的布尔类型使用 bool 表示。例如：
```rust
fn main() {
    let t = true;

    let f: bool = false; // with explicit type annotation
}
```

### 字符类型
Rust的 char 类型是语言中最原生的字母类型。下面是一些声明 char 值的例子：
```rust
fn main() {
    let c = 'z';
    let z: char = 'ℤ'; // with explicit type annotation
    let heart_eyed_cat = '😻';
}

```

### 复合类型
复合类型（Compound types）可以将多个值组合成一个类型。Rust 有两个原生的复合类型：元组（tuple）和数组（array）。

#### 元组类型
元组是一个将多个其他类型的值组合进一个复合类型的主要方式。元组长度固定：一旦声明，其长度不会增大或缩小。

我们使用包含在圆括号中的逗号分隔的值列表来创建一个元组。元组中的每一个位置都有一个类型，而且这些不同值的类型也不必是相同的。这个例子中使用了可选的类型注解：

```rust
fn main() {

    let tup = (500, 3.2, 2);

    let (mut x, y, z) = tup;

    println!("x: {}, y: {}, z: {}", x, y, z);

    x = 44;

    println!("x: {}, y: {}, z: {}", x, y, z);

    println!("tup.0: {}", tup.0);
}
}
```

#### 数组类型
另一个包含多个值的方式是 数组（array）。与元组不同，数组中的每个元素的类型必须相同。Rust 中的数组与一些其他语言中的数组不同，Rust中的数组长度是固定的。

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];

    for i in a.iter() {
        println!("i: {}", i);
    }

    let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
    ];

    for i in months.iter() {
        println!("{}", i);
    }
}
```

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5]; // a 为 5 个 i32 类型的变量，并且赋初值

let b = [10; 5]; // 分配b为 5个 10的数组

for i in a {
    println!("{}", i);
}
```

#### 摄氏温度转华氏温度
第一个版本
```rust
fn main() {
    fahrenheit_to_celsius();
}

fn fahrenheit_to_celsius() {
    let mut result: f64 = 0.0;

    let mut celsius = String::new(); // 摄氏度
    let mut fahrenheit = String::new(); // 华氏度
    let mut to_type = String::new(); // 为 0 摄氏度转华氏度，为 1 华氏度转摄氏度

    println!("请选择转换类型(输入 '0' 代表摄氏温度转华氏温度，输入'1' 代表华氏温度转摄氏温度)");
    // 1. 让用户选择转换类型
    std::io::stdin()
        .read_line(&mut to_type)
        .expect("读取数据失败");

    let to_type: u8 = to_type.trim().parse().expect("数据解析失败");
    if to_type == 0 {
        // F = 9 / 5C + 32
        println!("请输入摄氏温度");
        std::io::stdin()
            .read_line(&mut celsius)
            .expect("数据读取失败");
        let celsius: f64 = celsius.trim().parse().expect("数据解析失败");
        result = 9.0 / 5.0 * celsius + 32.0;
    } else if to_type == 1 {
        // C = 5 / 9 (F-32)
        println!("请输入华氏温度");
        std::io::stdin()
            .read_line(&mut fahrenheit)
            .expect("数据读取失败");
        let fahrenheit: f64 = fahrenheit.trim().parse().expect("数据解析失败");
        result = 5.0 / 9.0 * (fahrenheit - 32.0);
    } else {
        println!("请选择 0 或 1");
    }
    println!("结果为：{}", result);
}

```
改进版本

```rust
fn main() {
    fahrenheit_to_celsius();
}

fn fahrenheit_to_celsius() {
    let mut celsius = String::new(); // 摄氏度
    let mut fahrenheit = String::new(); // 华氏度
    let mut to_type = String::new(); // 为 0 摄氏度转华氏度，为 1 华氏度转摄氏度

    println!("请选择转换类型(输入 '0' 代表摄氏温度转华氏温度，输入'1' 代表华氏温度转摄氏温度)");
    // 1. 让用户选择转换类型
    std::io::stdin()
        .read_line(&mut to_type)
        .expect("读取数据失败");

    let to_type = match to_type.trim().parse() {
        Ok(v) => v,
        Err(e) => panic!("数据输入类型错误：{}", e),
    };
    let result = match to_type {
        0 => {
            println!("请输入摄氏温度");
            std::io::stdin()
                .read_line(&mut celsius)
                .expect("数据读取失败");

            let celsius: f64 = match celsius.trim().parse() {
                Ok(v) => v,
                Err(e) => panic!("数据输入类型错误：{}", e),
            };
            Some(9.0 / 5.0 * celsius + 32.0)
        }
        1 => {
            // C = 5 / 9 (F-32)
            println!("请输入华氏温度");
            std::io::stdin()
                .read_line(&mut fahrenheit)
                .expect("数据读取失败");
            let fahrenheit: f64 = match fahrenheit.trim().parse() {
                Ok(v) => v,
                Err(e) => panic!("数据输入类型错误：{}", e),
            };
            Some(5.0 / 9.0 * (fahrenheit - 32.0))
        }
        _ => None,
    };

    match result {
        Some(v) => {
            println!("结果为：{}", v);
        }
        None => println!("选择错误!,只能输入 0 或 1"),
    }
}
```

