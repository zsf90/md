# 按键程序

## 1. 延时消抖例子

```c
void key_scan_task(void *pvParameters)
{
    while(1) {
        FlagStatus key_result = gpio_input_bit_get(BOARD_BTN_PORT, BOARD_BTN_PIN);
        vTaskDelay(10);
        uint8_t flag = 0; // 标志位
      
        if(key_result == RESET)
        {
            vTaskDelay(10);
            while(RESET == gpio_input_bit_get(BOARD_BTN_PORT, BOARD_BTN_PIN));
            {
                flag = 1;
            }
            if(flag == 1)
            {
                flag = 0;
                gd_eval_led_toggle(LED1);
            }
        }
    }
} // key_scan_task
```

在该例子中会对按键先进行一次读取状态，如果按钮按下则延时 `10ms` ，然后再次判断按键状态，如果一直为按下状态则一直在循环体中不会退出，并且给标志位 `flag` 赋值 1。当按钮松开时则退出循环继续执行下面的流程，接下来判断标志位，如果为1则翻转 `LED` 的状态并且清除标志位。
