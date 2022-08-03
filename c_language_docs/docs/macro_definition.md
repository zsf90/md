# 宏

## #define 宏定义

#define 可以定义不带参数的宏定义，也可以定义带参数的宏函数。

### 简单的宏

```c
#define 标识符 替换列表
#define PI	3.1415
#define TWO_PI (2*PI)
#define CUBE(x) (x * x * x) /* 立方宏定义 */
#define X_MOD4(x) ((x) % 4) /* 一个数对4求余数 */
#define MAX100(x, y) ( ( (x) * (y) < 100) ) ? 1 : 0
```

### 对类型重命名

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



## #undef 删除一个宏定义

## #include 文件包含

## #if、#ifdef、#ifndef、#elif、#else、#endif 条件编译

**#ifdef** 测试一个标识符是否已经定义为宏

**#ifndef** 测试一个标识符是否为未被定义为宏

下面这段代码中第一行如果 BUFFER_SIZE 未被定义过则在第二行定义，如果定义过则不会再次定义。

```c
#ifndef BUFFER_SIZE
#define BUFFER_SIZE 256
#endif
```

## #error、#line、#pragma

## ##符号

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

## do {} while(0)

```c
#define ECHO(s) \
do { \
	gets(s); \
    puts(s); \
} while(0) \
```

## 预定义宏

C语言中预定了一些有用的宏，这些宏主要是提供当前编译的信息。


| 名字        | 描述                           |
| ----------- | ------------------------------ |
| \__LINE__ | 被编译的文件的行数             |
| \__FILE__ | 被编译的文件的名字             |
| \__DATE__ | 编译的日期                     |
| \__TIME__ | 编译的时间                     |
| \__STDC__ | 如果编译器接受标准C，那么值为1 |

## defined 运算符

这要DEBUG定义了就为真，不用给DEBUG定义值。

```c
#if defined(DEBUG)
...
#endif
```
