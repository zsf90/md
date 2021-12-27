# C 语言编程文档

## 预处理指令

### #define 宏定义

#define 可以定义不带参数的宏定义，也可以定义带参数的宏函数。

#### 简单的宏

```c
#define 标识符 替换列表
#define PI	3.1415
#define TWO_PI (2*PI)
#define CUBE(x) (x * x * x) /* 立方宏定义 */
#define X_MOD4(x) ((x) % 4) /* 一个数对4求余数 */
#define MAX100(x, y) ( ( (x) * (y) < 100) ) ? 1 : 0
```

#### 对类型重命名

```c
#define BOOL int
```

### 带参数的宏定义

```c
#define MAX(x, y)	((x)>(y) ? (x) : (y))
#define IS_EVEN(n)	((n) % 2 == 0)
#define SCALE(x) ((x)*10)
```

如果宏有参数，每次参数在替换列表中出现时都要放在圆括号中。

> 在宏定义中缺少圆括号会导致C语言中最让人讨厌的错误。程序通常仍然可以编译通过，而且宏似乎也可以工作，仅在少说情况下会出错。

### #undef 删除一个宏定义

### #include 文件包含

### #if、#ifdef、#ifndef、#elif、#else、#endif 条件编译

**#ifdef** 测试一个标识符是否已经定义为宏

**#ifndef** 测试一个标识符是否为未被定义为宏

下面这段代码中第一行如果 BUFFER_SIZE 未被定义过则在第二行定义，如果定义过则不会再次定义。

```c
#ifndef BUFFER_SIZE
#define BUFFER_SIZE 256
#endif
```



### #error、#line、#pragma

### ## 符号

```c
#define GENERIC_MAX(type) \
type type##_max(type: x, type y) \
{\
	return x > y ? x : y; \
}\
```

例子：

```c
#include <stdio.h>
#include <stdlib.h>
#include <assert.h>
#include <string.h>

#define GENERIC_MAX(type) \
type type##_max(type x, type y) \
{\
    return x > y ? x : y; \
}\

GENERIC_MAX(float)
GENERIC_MAX(int)

int main(void)
{
    float f1 = float_max(4.5f, 5.4f);
    printf("MAX Value: %.2f\n", f1);
    int i1 = int_max(45, 65);
    printf("MAX Value: %d\n", i1);

    return 0;
}
```

### do {} while(0)

```c
#define ECHO(s) \
do { \
	gets(s); \
    puts(s); \
} while(0) \
```

### 预定义宏

C语言中预定了一些有用的宏，这些宏主要是提供当前编译的信息。

| 名字      | 描述                           |
| --------- | ------------------------------ |
| \__LINE__ | 被编译的文件的行数             |
| \__FILE__ | 被编译的文件的名字             |
| \__DATE__ | 编译的日期                     |
| \__TIME__ | 编译的时间                     |
| \__STDC__ | 如果编译器接受标准C，那么值为1 |

### defined 运算符

这要DEBUG定义了就为真，不用给DEBUG定义值。

```c
#if defined(DEBUG)
...
#endif
```

## 标准库

C语言的标准库总共划分成15个部分，每个部分用一个头文件描述。标准头主要由函数原型、类型定义、以及宏定义组成。如果我们的文件中调用了头中的函数，或是使用了头中定义的类型或宏，那么我们就需要在文件开头将相应的头包含进来。当一个文件包含了多个头时，#include指令的顺序无关紧要。



### 对标准库中使用的名字一些限制

- 有一个下划线或一个大写字母开头或有两个下划线开头的标识符
- 由一个下划线开头的标识符被保留，仅可以在函数内部声明
- 在标准库中所有外部链接的标识符被保留

### 使用宏隐藏函数

C程序员经常会用宏来代替小的函数，这在标准库中同样很常见。C语言标准允许在头中定义与库函数同名的宏，为了起到保护作用，还要求有实际的函数存在。对于库的头，声明一个函数并同时定义一个有相同名字的宏的情况并不少见。

在`<ctype.h>` 中有大量这样成对的函数或宏定义的例子， 例如用来检测一个其字符是否可以显示的函数 `isprint`。通常的做法是在 `<ctype.h>`中定义 `isprint`作为一个函数：

```c
int isprint(int c);
```

并同时把它定义为一个宏

```c
#define isprint(c)	((c) >= 0x20 && (c) <= 0x7e)
```

> 在默认情况下，对` isprint` 的调用会被作为宏调用（因为宏名会在预处理时被替换）。
>
> 在大多数情况下，我们对于使用宏来代替实际的函数还是满意的，因为这样可能会提高程序的运行速度。然而在某些情况下，我们可能需要的是一个真实的函数。这可能是由于需要尽量缩小可执行代码的大小，也可能是因为需要一个指向这个函数的指针。

如果需要调用真实的函数而不是宏定义，我们可以使用 `#undef` 指令 来删除宏定义，从而可以访问到真实的函数。

下面代码通过取消了 `isprint`的宏定义来使用 `isprint` 函数.

```c
#include <ctype.h>
#undef isprint
```

注：此外，我们也可以通过给名字加圆括号来屏蔽个别宏调用：

```c
(isprint)(c)
```

### 标准库列表

1. `<assert.h>` - 诊断
2. `<ctype.h>` - 字符处理
3. `<errno.h>` - 错误
4. `<float.h>` - 浮点型的特性
5. `<limits.h>` - 整型的大小
6. `<locale.h>` - 本地化
7. `<math.h>` - 数学计算
8. `<setjmp.h>` 非本地跳转
9. `<signal.h>` - 信号处理
10. `<stdarg.h>` - 可变实际参数
11. `<stddef.h>` - 常用定义
12. `<stdio.h>` - 输入/输出
13. `<stdlib.h>` - 常用实用程序
14. `<string.h>` - 字符串处理
15. `<time.h>` - 时间和日期

### 1. `<assert.h>` - 诊断

### 11. `<stddef.h>` - 常用定义

`<stddef.h>` 提供了常用的类型和宏的定义，但没有声明任何函数。定义的类型包括：

- `ptrdiff_t` 当进行指针相减运算时，其结果的类型。
- size_t 运算符 `sizeof` 的返回值类型。
- `wchar_t` 一种足够大的、可以用于表示所有支持的地区的所有字符的类型。

这3中类型都是整数类型。其中 `ptrdiff_t` 必须是带符号的类型，而 size_t 则必须是无符号的类型。

`<stddef.h>` 中还定义了两个宏。一个是 **NULL**，用来表示空指针。另一个是 `offsetof` 需要两个、参数：类型（一个结构体类型）和指定成员（结构体的一个成员）。`offsetof`宏会计算结构体的起点到指定成员间的字节数。

```c
#define NULL ((void *)0)


#define offsetof(TYPE, MEMBER) __builtin_offsetof (TYPE, MEMBER)
```



## 输入输出

包含输入输出头文件 `#include <stdio.h>`。

函数列表：

1. `printf()` 格式化打印数据
2. `scanf()` 
3. `putchar()`
4. `getchar()`
5. `puts()` 
6. `gets()`
7. `fprintf()` 是 `printf()`的文件版本。
8. `fread()`
9. `fwrite()`
10. `sprintf()`
11. `sscanf()`
12. `perror()`
13. `vfprintf()`
14. `vprintf()`
15. `vsprintf()`

### 文件指针

C语言中流的访问是通过**文件指针**（file pointer）实现的。FILE * 在 `<stdio.h>` 头文件中定义。

```c
FILE *fp1, *fp2;
```

### 标准流和重定向

| 文件指针 | 流       | 默认的含义 |
| -------- | -------- | ---------- |
| stdin    | 标准输入 | 键盘       |
| stdout   | 标准输出 | 屏幕       |
| stderr   | 标准错误 | 屏幕       |
|          |          |            |

