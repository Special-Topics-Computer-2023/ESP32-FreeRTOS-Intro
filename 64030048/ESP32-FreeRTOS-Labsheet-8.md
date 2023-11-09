# การทดลอง ESP32 FreeRTOS 
##  FreeRTOS xQueueSendFromISR


### 1. สร้าง project ใหม่ `ESP-IDF-QueueFromISR` โดยไม่ต้องใช้ template

### 2. แก้ไข code

2.1 ส่วน include ให้เพิ่ม

```c
#include <freertos/FreeRTOS.h>
#include <freertos/task.h>
#include "freertos/queue.h"
#include "driver/gpio.h"
```

2.2 ส่วน #define 

```c
#define LED_PIN 27
#define PUSH_BUTTON_PIN 33
```

2.3 ส่วน handle นอกจากจะสร้าง handle สำหรับ task สองตัวแล้ว ให้สร้าง handle สำหรับ queue ด้วย

```c
TaskHandle_t myTaskHandle = NULL;
QueueHandle_t queue;
```

2.4  code ของ task 

task  ใช้เป็น consumer task  รอรับ queue ที่ส่งมาจาก ISR ที่รองรับเหตุการณ์จากการกดปุ่ม

```c
void Task(void *arg)
{
	// 1. เตรียมพื้นที่รับข้อมูลผ่าน queue
	char rxBuffer;
	// 2. วนลูปรอ
	while (1)
	{
		// 3. ถ้ามีการส่งข้อมูลผ่าน queue handle ที่รอรับ ให้เอาข้อมูลไปแสดงผล
		// queue consumer จะใช้ฟังก์ชัน xQueueReceive
		if (xQueueReceive(queue, &(rxBuffer), (TickType_t) 5))
		{
			printf("Button pressed!\n");
			vTaskDelay(1000 / portTICK_RATE_MS);
		}
	}
}
```

2.5 Code สำหรับบริการอินเตอรรัพต์จากการกดปุ่ม

```c
void IRAM_ATTR button_isr_handler(void *arg)
{
	// code จาก https://www.freertos.org/a00119.html
	char cIn;
	BaseType_t xHigherPriorityTaskWoken;
	/* We have not woken a task at the start of the ISR. */
	xHigherPriorityTaskWoken = pdFALSE;
	cIn = '1';
	xQueueSendFromISR(queue, &cIn, &xHigherPriorityTaskWoken);
}
```

2.6 Code ของ app_main()

app_main() ทำหน้าที่กำหนด I/O, ติดตั้งอินเทอร์รัพต์, สร้าง queue และสร้าง Task


```c
void app_main(void)
{
	gpio_pad_select_gpio(PUSH_BUTTON_PIN);
	gpio_pad_select_gpio(LED_PIN);

	gpio_set_direction(PUSH_BUTTON_PIN, GPIO_MODE_INPUT);
	gpio_set_direction(LED_PIN, GPIO_MODE_OUTPUT);

	gpio_set_intr_type(PUSH_BUTTON_PIN, GPIO_INTR_POSEDGE);

	gpio_install_isr_service(0);

	gpio_isr_handler_add(PUSH_BUTTON_PIN, button_isr_handler, NULL);

	// สร้าง queue ที่บรรจุตัวแปร char จำนวน 1 ตัว
	queue = xQueueCreate(1, sizeof(char));

	// ทดสอบว่าสร้าง queue สำเร็จ?
	if (queue == 0)
	{
		printf("Failed to create queue= %p\n", queue);
	}

	xTaskCreatePinnedToCore(Task, "My_Task", 4096, NULL, 10, &myTaskHandle, 1);
}

```

### 3. รันและบันทึกผลจากโปรแกรมข้างบน วิเคราะห์ผลที่ได้ว่าเป็นอย่างไร

![ภาพ](https://github.com/kammam19/ESP32-FreeRTOS-Intro/assets/112167732/057cdfa6-acf2-45ed-92f9-7808e5235c68)

## กดรันโปรแกรม หากกดปุ่มโปรแกรมจะแสดงผลว่า Button pressed!

## [>> ต่อไป สัปดาห์หน้า>>](README.md) 
