# การทดลอง ESP32 FreeRTOS 
##  ฟังก์ชันบริการ Interrupt ทำงานร่วมกับ FreeRTOS

ในใบงานก่อนหน้านี้ เราได้ทดลองใช้งาน interrupt จากการกดปุ่ม button ไปบ้างแล้ว

ใบงานนี้จะเรียนรู้รายละเอียดการทำงานกับ interrupt บน FreeRTOS

### 1. การเชื่อมต่อทาง Hardware

1.1 LED pin  =  27

1.2 Push button pin = 33 

### 2. Software code

2.1 Include

เพื่อที่จะใช้งาน GPIO ให้ include ไฟล์ `"driver/gpio.h"`

```c
#include "driver/gpio.h"
```

2.1 Task handle

สร้าง task handle เพื่อเรียกใช้ใน `xTaskCreate(...)`

```c
TaskHandle_t ISR = NULL;
```

2.2 Interrupt task


เมื่อถูกสร้างและเรียกมาทำงาน task นี้จะส่งตัวเองเข้าสู่สภาวะ suspend ด้วยคำสั่ง `vTaskSuspend(NULL);` จึงไม่มีการทำงานใด ๆ จนกว่า OS หรือ task อื่น ๆ เรียกให้ขึ้นมาทำงานด้วยคำสั่ง `vTaskResume()`

```c
void interrupt_task(void *arg)
{
  bool led_status = false;
  while(1)
  {
    vTaskSuspend(NULL);
    led_status = !led_status;
    gpio_set_level(LED_PIN, led_status);
    printf("Button pressed!\n");
  }
}
```

2.3 Interrupt handler

Interrupt handler นี้จะถูกเรียกจากการกดปุ่มที่ลงทะเบียนไว้

เมื่อถูกเรียก มันจะไปทำการปลุก (resume) ให้ interrupt_task (ในข้อ 2.2) ทำงานต่อเป็นจำนวน 1 รอบ แล้ว suspend ต่อ

คำสั่ง   `xTaskResumeFromISR(ISR);` เป็นการปลุก (resume) task จากภายใน interrupt handler  (ถ้าเป็นการเรียกปกติก็ใช้ `xTaskResume(...)`)

```c
void IRAM_ATTR button_isr_handler(void *arg)
{
  xTaskResumeFromISR(ISR);
}
```

2.4 Code ของ app_main()

```c
void app_main(void)
{
  gpio_pad_select_gpio(PUSH_BUTTON_PIN);
  gpio_pad_select_gpio(LED_PIN);

  gpio_set_direction(PUSH_BUTTON_PIN, GPIO_MODE_INPUT);
  gpio_set_direction(LED_PIN ,GPIO_MODE_OUTPUT);

  gpio_set_intr_type(PUSH_BUTTON_PIN, GPIO_INTR_POSEDGE);

  gpio_install_isr_service(0);

  gpio_isr_handler_add(PUSH_BUTTON_PIN, button_isr_handler, NULL);

  xTaskCreate(interrupt_task, "interrupt_task", 4096, NULL, 10, &ISR);
}
```

### 3. รันและบันทึกผลจากโปรแกรมข้างบน วิเคราะห์ผลที่ได้ว่าเป็นอย่างไร
เมื่อกดปุ่ม LED จะติดและแสดงผลว่าปุ่มถูกกดโดยจะเรียก method interrupt_task ที่เป็นตัวกำหนดว่า LED จะติดหรือดับ
![Screenshot 2023-10-23 151703](https://github.com/Fixckpx/ESP32-FreeRTOS-Intro/assets/115066186/4adab347-7341-40e9-a384-8a782eab4b5b)

## [>> ต่อไป >>](./ESP32-FreeRTOS-Labsheet-7.md) 
