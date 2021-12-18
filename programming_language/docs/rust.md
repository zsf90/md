# Rust 语言编程文档

`Rust` 是比较新的编程语言，性能较好可与C/C++比较，也是适合写系统的语言。

## Hello,World!
第一个程序当然是 "Hello,World!"。
```rust
// hello.rs

fn main() {
	println!("Hello,World!");
}
```

## Cargo
查看帮助可以使用 `cargo --help`，查看指定命令的帮助 `cargo build --help`。

### 查看帮助
```shell
$ cargo --help
Rust's package manager

USAGE:
    cargo [+toolchain] [OPTIONS] [SUBCOMMAND]

OPTIONS:
    -V, --version                  Print version info and exit
        --list                     List installed commands
        --explain <CODE>           Run `rustc --explain CODE`
    -v, --verbose                  Use verbose output (-vv very verbose/build.rs output)
    -q, --quiet                    No output printed to stdout
        --color <WHEN>             Coloring: auto, always, never
        --frozen                   Require Cargo.lock and cache are up to date
        --locked                   Require Cargo.lock is up to date
        --offline                  Run without accessing the network
        --config <KEY=VALUE>...    Override a configuration value (unstable)
    -Z <FLAG>...                   Unstable (nightly-only) flags to Cargo, see 'cargo -Z help' for details
    -h, --help                     Prints help information

Some common cargo commands are (see all commands with --list):
    build, b    Compile the current package
    check, c    Analyze the current package and report errors, but don't build object files
    clean       Remove the target directory
    doc, d      Build this package's and its dependencies' documentation
    new         Create a new cargo package
    init        Create a new cargo package in an existing directory
    run, r      Run a binary or example of the local package
    test, t     Run the tests
    bench       Run the benchmarks
    update      Update dependencies listed in Cargo.lock
    search      Search registry for crates
    publish     Package and upload this package to the registry
    install     Install a Rust binary. Default location is $HOME/.cargo/bin
    uninstall   Uninstall a Rust binary

See 'cargo help <command>' for more information on a specific command.
```

### 创建一个项目
`cargo new hello_cargo`，该命令生成的结构：
```shell
zsf90@DESKTOP-N95R1I7:$ tree
.
├── Cargo.toml
└── src
    └── main.rs
```

**Cargo.toml**
这个文件使用 [TOML](https://toml.io/) (Tom's Obvious, Minimal Language) 格式，这是 Cargo 配置文件的格式。
```toml
[package]
name = "hello_cargo"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
rand = "0.8.4"
```

## 变量与常量

```rust
// hello.rs

fn main() {
	let height = 10; // 定义了一个变量，不可变
	let mut width = 20; // 定义了一个可变变量，加上 mut
	const PI: f64 = 3.1415926; // 定义了一个常量
}
```

上面代码中 `height`是一个不可变的变量，`width` 是一个可变的变量，`PI` 是一个常量，定义常量必须显式指明数据类型，否则不能通过编译。

### 隐藏（Shadowing）
用 let 定义的不可变变量可以多次定义，最新的变量覆盖原来的变量，rust中这这为**隐藏**。

**main.rs**
```rust
fn main() {
    let x = 5;

    let x = x + 1;

    {
        let x = x * 2;
        println!("The value of x in the inner scope is: {}", x);
    }

    println!("The value of x is: {}", x);
}
```

## 复合类型
复合类型（Compound types）可以将多个值组合成一个类型。Rust 有两个原生的复合类型：元组（tuple）和数组（array）。

### 元组类型 ()
元组的元素可以是多种数据类型。
```rust
fn main() {
	let tup: (i32, f64, u8) = (500, 5.5, 1);
}
```

没有任何值的元组 () 是一种特殊的类型，只有一个值，也写成 () 。该类型被称为 单元类型（unit type），而该值被称为 单元值（unit value）。如果表达式不返回任何其他值，则会隐式返回单元值。

### 数组类型 []
数组是一种只能保存单一类型数据的结构。
```rust 
fn fn main() {
	let a = [1,2,3,4,5];

	let b: [i32; 5] = [1, 2, 3, 4, 5];

	let c = [3; 5]; // let c = [3, 3, 3, 3, 3];

}
```

## 所有权

### 栈与堆
指针动态分配的内存在**堆**上。
定义的变量分配在**栈**里。后进先出（last in, first out）
#### 栈
栈以放入值的顺序存储值并以相反顺序取出值。这也被称作 **后进先出**（last in, first out）。想象一下一叠盘子：当增加更多盘子时，把它们放在盘子堆的顶部，当需要盘子时，也从顶部拿走。不能从中间也不能从底部增加或拿走盘子！增加数据叫做 **进栈**（pushing onto the stack），而移出数据叫做 **出栈**（popping off the stack）。

**栈**中的所有数据都必须占用已知且固定的大小。在编译时大小未知或大小可能变化的数据，要改为存储在堆上。

#### 堆
堆是缺乏组织的：当向堆放入数据时，你要请求一定大小的空间。内存分配器（memory allocator）在堆的某处找到一块足够大的空位，把它标记为已使用，并返回一个表示该位置地址的 指针（pointer）。这个过程称作 在堆上分配内存（allocating on the heap），有时简称为 “分配”（allocating）。将数据推入栈中并不被认为是分配。因为指针的大小是已知并且固定的，你可以将指针存储在栈上，不过当需要实际数据时，必须访问指针。

### 所有权规则
1. Rust 中的每一个值都有一个被称为其 所有者（owner）的变量。
2. 值在任一时刻有且只有一个所有者。
3. 当所有者（变量）离开作用域，这个值将被丢弃。