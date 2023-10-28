# การทดลอง ESP32 FreeRTOS 
##  ฟังก์ชัน xTaskCreatePinnedToCore()

1. แก่ไข Code จากโปรแกรมที่แล้ว โดยการเพิ่ม task เข้าไปอีก 1 task (ดู comment ใน code)



```c
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>

#include "freertos/FreeRTOS.h"
#include "freertos/task.h"

TaskHandle_t MyFirstTaskHandle = NULL;
// 1. เพิ่ม MySeconeTaskHandle
TaskHandle_t MySeconeTaskHandle = NULL;

void My_First_Task(void * arg)
{
	uint32_t i = 0;
	while(1)
	{
		printf("Hello My First Task %d\n",i);
		vTaskDelay(1000/portTICK_RATE_MS);
		i++;
	}
}

// 2. เพิ่ม task function
void My_Second_Task(void * arg)
{
	uint32_t i = 0;
	while(1)
	{
		printf("Hello My Second Task %d\n",i);
		vTaskDelay(1000/portTICK_RATE_MS);
		i++;
	}
}


void app_main(void)
{
	xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);
	// 3. สร้างและเรียกใช้ task
	xTaskCreate(My_Second_Task, "Second_Task", 4096, NULL, 10, &MySeconeTaskHandle);
}
```

2. รันและบันทึกผลจากโปรแกรมข้างบน

![](./Pictures/Result/Lab3.png)

* ทุก 1 วินาทีจะเรียกใช้งานฟังก์ชั่น My_First_Task และ My_Second_Task ไปพร้อมกัน

3.  แก้ไข code ในส่วนของการสร้าง task 2 (ตามหมายเหตุหมายเลข 3) เป็นดังนี้

```c
void app_main(void)
{
	xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);
	// สร้างและเรียกใช้ task ด้วยฟังก์ชัน xTaskCreatePinnedToCore
	xTaskCreatePinnedToCore(My_Second_Task, "Second_Task", 4096, NULL, 10, &MySeconeTaskHandle, 1);
}
```

4. รันและบันทึกผลจากโปรแกรมข้างบน ได้ผลเหมือนหรือต่างกันอย่างไร

![](./Pictures/Result/Lab3_2.png)

* ผลลัพธ์ที่ได้เหมือนกันทุก 1 วินาทีมีการเรียกใช้งานฟังก์ชั่น My_First_Task และ My_Second_Task เหมือนกัน

## [>> ต่อไป >>](./ESP32-FreeRTOS-Labsheet-4.md) 