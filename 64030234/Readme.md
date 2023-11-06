# นางสาวกุลธิดา  เกษร

# Lab 1

```css
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>

#include <freertos/FreeRTOS.h>
#include <freertos/task.h>

TaskHandle_t MyFirstTaskHandle = NULL;

void My_First_Task(void * arg)
{
    int  i = 0;
    while(1)
    {
        printf("Hello My First Task %d\n", i);
        vTaskDelay(1000 / portTICK_PERIOD_MS);
        i++;
    }
}

void app_main(void)
{
    xTaskCreate(My_First_Task, "First_Task", 4096, NULL, 10, &MyFirstTaskHandle);
}
```
รันโปรแกรมและอธิบายผลที่ได้

![image](https://github.com/KUNTIDA234/ESP32-FreeRTOS-Intro/assets/115066215/1e032407-eb0c-40db-aded-d086506fec20)

- ผลลัพธ์ที่ได้เมื่อทุก 1 วินาทีโปรแกรมจะบวกค่าตัวแปร i ทีละ 1 และจะแสดงข้อความ Hello My First Task ตามด้วยค่าของตัวแปร i

# Lab 2
```css
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>

#include <freertos/FreeRTOS.h>
#include <freertos/task.h>

TaskHandle_t MyFirstTaskHandle = NULL;

void My_First_Task(void * arg)
{
    int  i = 0;
    while(1)
    {
        printf("Hello My First Task %d\n", i);
        vTaskDelay(1000 / portTICK_PERIOD_MS);
        i++;
    }
}

void app_main(void)
{
    xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);
    xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);
    xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);
    xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);
    xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);
}

```

รันโปรแกรมและอธิบายผลที่ได้

![image](https://github.com/KUNTIDA234/ESP32-FreeRTOS-Intro/assets/115066215/c2ba2c8f-0078-4771-b258-e4d8ba6b1fcd)

- โปรแกรมจะเรียก My_First_Task 5 ครั้งและ terminal จะแสดงข้อความ Hello My First Task ตามด้วย เลขเดียวกัน 5 ครั้ง 

# Lab 3
แก้ไขโปรแกรม ดังนี้
```css
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
	int i = 0;
	while(1)
	{
		printf("Hello My First Task %d\n",i);
		vTaskDelay(1000/portTICK_PERIOD_MS);
		i++;
	}
}

// 2. เพิ่ม task function
void My_Second_Task(void * arg)
{
	int i = 0;
	while(1)
	{
		printf("Hello My Second Task %d\n",i);
		vTaskDelay(1000/portTICK_PERIOD_MS);
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
รันโปรแกรมและอธิบายผลที่ได้

![image](https://github.com/KUNTIDA234/ESP32-FreeRTOS-Intro/assets/115066215/adfcc88b-b811-45e0-a8d8-dd7503990a27)

แก้ไข code ในส่วนของการสร้าง task 2 (ตามหมายเหตุหมายเลข 3) เป็นดังนี้
```c
void app_main(void)
{
	xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);
	// สร้างและเรียกใช้ task ด้วยฟังก์ชัน xTaskCreatePinnedToCore
	xTaskCreatePinnedToCore(My_Second_Task, "Second_Task", 4096, NULL, 10, &MySeconeTaskHandle, 1);
}
```
รันและบันทึกผลจากโปรแกรมข้างบน ได้ผลเหมือนหรือต่างกันอย่างไร

![image](https://github.com/KUNTIDA234/ESP32-FreeRTOS-Intro/assets/115066215/ee7cf4d8-7194-45c4-8101-925f4efafe41)

- โปรแกรมจะเรียก My_First_Task และ My_Second_Task method เพื่อแสดงคำว่า Hello My First Task และ Hello My Second Task ตามด้วยเลขครั้งที่แสดง โดย First และ Second จะแสดงพร้อมกัน 

# Lab 4

รันและบันทึกผลจากโปรแกรมข้างบน ได้ผลเหมือนหรือต่างกันอย่างไร

![image](https://github.com/KUNTIDA234/ESP32-FreeRTOS-Intro/assets/115066215/51fb2799-6b70-4b13-9a8a-66f65adcad4e)

- โปรแกรมจะทำงาน 5 ครั้ง และลบ MySecondTaskHandle จากนั้นจะทำงานต่อเฉพาะ MyFirstTaskHandle

# Lab 5

แก้ไขโปรแกรมให้เป็นดังต่อไปนี้

```css
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
	int i = 0;
	while(1)
	{
		printf("Hello My First Task %d\n",i);
		vTaskDelay(1000/portTICK_PERIOD_MS);
		i++;

		if(i == 5)
		{
			vTaskSuspend(MySeconeTaskHandle);
			printf("Second Task suspended\n");
		}

		if(i == 10)
		{
			vTaskResume(MySeconeTaskHandle);
			printf("Second Task Resumed\n");
		}

		if(i == 15)
		{
			vTaskDelete(MySeconeTaskHandle);
			printf("Second Task deleted\n");
		}

		if(i == 20)
		{
			printf("MyFirstTaskHandle will suspend itself\n");
			vTaskSuspend(NULL);
		}
	}
}
// 2. เพิ่ม task function
void My_Second_Task(void * arg)
{
	int i = 0;
	while(1)
	{
		printf("Hello My Second Task %d\n",i);
		vTaskDelay(1000/portTICK_PERIOD_MS);
		i++;
	}
}


void app_main(void)
{
	xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);
	// สร้างและเรียกใช้ task ด้วยฟังก์ชัน xTaskCreatePinnedToCore
	xTaskCreatePinnedToCore(My_Second_Task, "Second_Task", 4096, NULL, 10, &MySeconeTaskHandle, 1);
}

```
รันและบันทึกผลจากโปรแกรมข้างบน วิเคราะห์ผลที่ได้ว่าเป็นอย่างไร

![image](https://github.com/KUNTIDA234/ESP32-FreeRTOS-Intro/assets/115066215/e154892c-7ec7-4450-870b-8f9eab287abe)

- เมื่อโปรแกรมทำงานจะเรียกใช้ฟังก์ชั่น My_First_Task และ My_Second_Task เพิ่มค่าตัวแปร i ทีละ 1 ค่าทุก 1 วินาที เมื่อ i มีค่าเท่ากับ 5 ฟังก์ชั่น My_First_Task จะหยุดการทำงานของฟังก์ชั่น My_Second_Task และเมื่อ i มีค่าเท่ากับ 10 ฟังก์ชั่น My_Second_Task จะกลับมาทำงานต่อ และเมื่อ i เท่ากับ 15 ฟังก์ชั่น My_Second_Task จะถูกลบ และเมื่อ i เท่ากับ 20 ฟังก์ชั่น My_First_Task จะถูกหยุดการทำงาน

# Lab 6
รันและบันทึกผลจากโปรแกรมข้างบน 

![image](https://github.com/KUNTIDA234/ESP32-FreeRTOS-Intro/assets/115066215/c0500803-04a0-4947-969c-97f869b03ad5)

วิเคราะห์ผลที่ได้ว่าเป็นอย่างไร
- เมื่อกดปุ่ม LED จะติดและแสดงผลว่าปุ่มถูกกดโดยจะเรียก method interrupt_task ที่เป็นตัวกำหนดว่า LED จะติดหรือดับ

# Lab 7 
รันและบันทึกผลจากโปรแกรมข้างบน 
![image](https://github.com/KUNTIDA234/ESP32-FreeRTOS-Intro/assets/115066215/9f04c6db-d066-4c19-82b7-4a41e82a01a1)

วิเคราะห์ผลที่ได้ว่าเป็นอย่างไร

- โปรแกรมะแสดงผลข้อความ Received data from queue == จาก Task2 และตามด้วยข้อความ Hello from Demo_Task 1 2 3 ตามลำดับจนครบจากนั้นจะทำการวนลูปไปเรื่อยๆ

# Lab 8
รันและบันทึกผลจากโปรแกรมข้างบน 

![image](https://github.com/KUNTIDA234/ESP32-FreeRTOS-Intro/assets/115066215/77587738-04ad-43ec-84c1-81e7455d1b2b)

วิเคราะห์ผลที่ได้ว่าเป็นอย่างไร
- เมื่อปุ่มถูกกด จะเกิดการเรียก button_isr_handler ซึ่งจะส่งข้อมูล "1" ผ่าน queue และทำให้งาน Task ที่รอรับข้อมูลแสดงข้อความ "Button pressed!"
