# การทดลอง ESP32 FreeRTOS 
##  FreeRTOS Queue

ข้อมูลเพิ่มเติมจากที่นี่ https://www.freertos.org/Embedded-RTOS-Queues.html

### 1. สร้าง project ใหม่ `ESP-IDF-Queue` โดยไม่ต้องใช้ template

### 2. แก้ไข code

2.1 ส่วน include ให้เพิ่ม

```c
#include <stdio.h>
#include <freertos/FreeRTOS.h>
#include <freertos/task.h>
#include "freertos/queue.h"
```


2.2 ส่วน handle นอกจากจะสร้าง handle สำหรับ task สองตัวแล้ว ให้สร้าง handle สำหรับ queue ด้วย

```c
TaskHandle_t myTaskHandle = NULL;
TaskHandle_t myTaskHandle2 = NULL;
QueueHandle_t queue;
```

2.3  code ของ task #1

task1 ใช้เป็น producer task ทำหน้าที่ส่งข้อความผ่าน queue ไปยัง task ที่รอรับข้อมูลผ่าน queue

```c
void Task1(void *arg)
{
    // 1. จองพื้นที่สำหรับสร้างข้อมูลที่จะส่งผ่าน queue จำนวน 50 ตัวอักษร
    char txBuffer[50];

    // 2. สร้าง queue แล้วเก็บไว้ในตัวแปร handle
    queue = xQueueCreate(5, sizeof(txBuffer));
    // 3. ทดสอบว่าสร้าง queue สำเร็จหรือไม่ 
    if (queue == 0)
    {
     printf("Failed to create queue= %p\n", queue);
    }

    // 4. เตรียมข้อมูลและส่งผ่าน queue ทำเป็นจำนวน 3 รอบ
    // queue producerใช้ฟังก์ชัน xQueueSend
    sprintf(txBuffer, "Hello from Demo_Task 1");
    xQueueSend(queue, (void*)txBuffer, (TickType_t)0);

    sprintf(txBuffer, "Hello from Demo_Task 2");
    xQueueSend(queue, (void*)txBuffer, (TickType_t)0);

    sprintf(txBuffer, "Hello from Demo_Task 3");
    xQueueSend(queue, (void*)txBuffer, (TickType_t)0);

    // 5. ส่งครบ 3 ครั้ง ให้วนลูปไม่รู้จบ ไม่ต้องทำอะไรเพิ่ม
    while(1)
    {
        vTaskDelay(1000/ portTICK_RATE_MS);
    }
}
```

2.4 Code สำหรับ task #2

task 2 ใช้สำหรับ

```c
void Task2(void *arg)
{
  // 1. เตรียมพื้นที่รับข้อมูลผ่าน queue
	char rxBuffer[50];
  // 2. วนลูปรอ
	while (1)
	{
    // 3. ถ้ามีการส่งข้อมูลผ่าน queue handle ที่รอรับ ให้เอาข้อมูลไปแสดงผล
    // queue consumer จะใช้ฟังก์ชัน xQueueReceive 
		if (xQueueReceive(queue, &(rxBuffer), (TickType_t) 5))
		{
			printf("Received data from queue == %s\n", rxBuffer);
			vTaskDelay(1000 / portTICK_RATE_MS);

		}
	}
}
```
2.5 Code ของ app_main()

app_main() จะสร้าง task ขึ้นมา 2 task ที่ทำหน้าที่เป็น producer และ consumer ของ queue


```c
void app_main(void)
{
	xTaskCreate(Task1, "Task_1", 4096, NULL, 10, &myTaskHandle);
	xTaskCreatePinnedToCore(Task2, "Task_2", 4096, NULL, 10, &myTaskHandle2, 1);
}
```

### 3. รันและบันทึกผลจากโปรแกรมข้างบน วิเคราะห์ผลที่ได้ว่าเป็นอย่างไร

![](./64030131%20Pattanasak/Pictures/Result/Lab7.png)

* Task1:
จองพื้นที่เพื่อเก็บข้อมูลที่จะส่งผ่าน queue.
สร้าง queue ที่สามารถรองรับ 5 ข้อมูลและเก็บไว้ในตัวแปร queue.
ทดสอบว่าการสร้าง queue สำเร็จหรือไม่.
เตรียมข้อมูลและส่งผ่าน queue 3 ครั้ง.
หลังจากส่งข้อมูลครบ 3 ครั้งแล้ว, จะเข้าสู่ลูปไม่รู้จบโดยรอเพื่อรองรับข้อมูลจาก queue. <br>
Task2:
เตรียมพื้นที่เพื่อรับข้อมูลผ่าน queue.
วนลูปไม่รู้จบโดยรอการรับข้อมูลผ่าน queue.
ถ้ามีข้อมูลถูกส่งผ่าน queue, จะนำข้อมูลนั้นมาแสดงผลและรอเวลา 1 วินาที.
ทั้ง Task1 และ Task2 ทำงานพร้อมกันและสื่อสารผ่าน queue โดยการส่งข้อมูลจาก Task1 ไปยัง Task2 และแสดงข้อมูลที่ถูกรับใน Task2. อันตรายพวกนี้ทำงานอย่างไม่ร้ายแรงและไม่จบการทำงานเลยและรอรับข้อมูลหรือรอเวลาก่อนที่จะทำการแสดงผลข้อมูล.

## [>> ต่อไป >>](./ESP32-FreeRTOS-Labsheet-8.md) 