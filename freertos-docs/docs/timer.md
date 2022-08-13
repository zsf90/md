# 定时器

使用 `FreeRTOS`软件定时器时需要引入一下头文件：

```c
#include "FreeRTOS.h"
#include "timers.h"
```

## 创建定时器

函数原型：

```c
TimerHandle_t xTimerCreate( const char *pcTimerName, 
 const TickType_t xTimerPeriod, 
 const UBaseType_t uxAutoReload, 
 void * const pvTimerID, 
 TimerCallbackFunction_t pxCallbackFunction );
```

### pcTimerName

该参数给定时器起个名字。

### xTimerPeriod

该参数用来设定定时器的周期，也就是经过多次时间触发一次回调函数。用 `pdMS_TO_TICKS()`用把 `ms`转化成时钟节拍数。

### uxAutoReload

该参数设定是否自动重新开始定时，也就是是否只定时一次还是自动循环下去。

### pvTimerID

给该定时器创建一个ID，用于在回调函数中区分是那个定时器超时了。

### pxCallbackFunction

该参数是定时器超时时要调用的回调函数，函数原型如下：

```c
void vCallbackFunctionExample( TimerHandle_t xTimer );
```


## 修改定时器的周期

函数原型：

```c
BaseType_t xTimerChangePeriod( TimerHandle_t xTimer, 
 TickType_t xNewPeriod,
 TickType_t xTicksToWait );
```

### 1. xTimer

该参数是指要修改那个定时器的周期，类型是定时器句柄（TimerHandle_t）。

### 2. xNewPeriod

该参数是设定新的周期值。

### 3. xTicksToWait

等待时钟可以通过宏(`pdMS_TO_TICKS()`)设定
