# 任务

## 启动调度器

`vTaskStartScheduler();  // 启动调度器`

## xTaskCreate - 创建任务

函数原型：

```c
BaseType_t xTaskCreate( TaskFunction_t pxTaskCode,
                            const char * const pcName, /*lint !e971 Unqualified char types are allowed for strings and single characters only. */
                            const configSTACK_DEPTH_TYPE usStackDepth,
                            void * const pvParameters,
                            UBaseType_t uxPriority,
                            TaskHandle_t * const pxCreatedTask )
```

### 返回类型

返回类型：`BaseType_t ->long`

参数

1. `TaskFunction_t pxTaskCode` 任务函数
2. `const char * const pcName` 任务名称
3. `const configSTACK_DEPTH_TYPE usStackDepth` 堆栈大小，单位（字）
4. `void * const pvParameters` 任务参数
5. `UBaseType_t uxPriority`, 任务优先级
6. `TaskHandle_t * const pxCreatedTask` 任务句柄
