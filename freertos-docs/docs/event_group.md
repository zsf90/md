# 事件组

## 创建事件组

xEventGroupCreate()

```c
#include “FreeRTOS.h”
#include “event_groups.h”
EventGroupHandle_t xEventGroupCreate( void );
```

## 事件删除函数 vEventGroupDelete()

函数定义

```c
void vEventGroupDelete( EventGroupHandle_t xEventGroup )
{
    EventGroup_t * pxEventBits = xEventGroup;
    const List_t * pxTasksWaitingForBits = &( pxEventBits->xTasksWaitingForBits );

    vTaskSuspendAll();
    {
        traceEVENT_GROUP_DELETE( xEventGroup );

        while( listCURRENT_LIST_LENGTH( pxTasksWaitingForBits ) > ( UBaseType_t ) 0 )
        {
            /* Unblock the task, returning 0 as the event list is being deleted
             * and cannot therefore have any bits set. */
            configASSERT( pxTasksWaitingForBits->xListEnd.pxNext != ( const ListItem_t * ) &( pxTasksWaitingForBits->xListEnd ) );
            vTaskRemoveFromUnorderedEventList( pxTasksWaitingForBits->xListEnd.pxNext, eventUNBLOCKED_DUE_TO_BIT_SET );
        }

        #if ( ( configSUPPORT_DYNAMIC_ALLOCATION == 1 ) && ( configSUPPORT_STATIC_ALLOCATION == 0 ) )
            {
                /* The event group can only have been allocated dynamically - free
                 * it again. */
                vPortFree( pxEventBits );
            }
        #elif ( ( configSUPPORT_DYNAMIC_ALLOCATION == 1 ) && ( configSUPPORT_STATIC_ALLOCATION == 1 ) )
            {
                /* The event group could have been allocated statically or
                 * dynamically, so check before attempting to free the memory. */
                if( pxEventBits->ucStaticallyAllocated == ( uint8_t ) pdFALSE )
                {
                    vPortFree( pxEventBits );
                }
                else
                {
                    mtCOVERAGE_TEST_MARKER();
                }
            }
        #endif /* configSUPPORT_DYNAMIC_ALLOCATION */
    }
    ( void ) xTaskResumeAll();
}
```

## 事件组置位函数 xEventGroupSetBitsFromISR()

该函数是中断专用的，功能和xEventGroupSetBits类似。

事件等待

```c
EventBits_t xEventGroupWaitBits( const EventGroupHandle_t xEventGroup, /* 事件标志组句柄 */
 const EventBits_t uxBitsToWaitFor, /* 等待被设置的事件标志位 */
 const BaseType_t xClearOnExit, /* 选择是否清零被置位的事件标志位 */
 const BaseType_t xWaitForAllBits,  /* 选择是否等待所有标志位都被设置 */
 TickType_t xTicksToWait /* 设置等待时间 */
);
```

## 例子 - 在中断中置位事件组

```c
// 创建一个事件组handle变量
EventGroupHandle_t event_group_1;

#define BIT_0 (1<<0)
#define BIT_4 (1<<4)

/*
* 外部中断 0,1 处理函数
*/
void EXTI0_1_IRQHandler(void)
{
    if(RESET != exti_interrupt_flag_get(BOARD_BTN_EXTI_LINE)) {
        BaseType_t xResult;
        BaseType_t xHigherPriorityTaskWoken = pdFALSE;
  
        xResult = xEventGroupSetBitsFromISR(event_group_1, BIT_0, &xHigherPriorityTaskWoken);

        if(xResult != pdFAIL)
        {
            portYIELD_FROM_ISR(xHigherPriorityTaskWoken);
        }
        exti_interrupt_flag_clear(BOARD_BTN_EXTI_LINE); // 清除中断标志
    }
}

// 任务阻塞等待事件发生
void led_task(void *pvParameters)
{
    EventBits_t uxBits;
    const TickType_t xTicksToWait = 100;
    while(1) {
        uxBits = xEventGroupWaitBits(event_group_1, BIT_0, pdTRUE, pdTRUE, xTicksToWait);
  
        if(uxBits & BIT_0)
        {
            gd_eval_led_toggle(LED2);
        }
    }
}
```
