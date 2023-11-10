# 64030207
## Lab 1

```css
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
TaskHandle_t MyFirstTaskHandle = NULL;
void My_First_Task(void *arg)
{
    uint32_t i = 0;
    while (1)
    {
        printf("Hello My First Task %d\n", i);
        vTaskDelay(1000 / portTICK_RATE_MS);
        i++;
    }
}
void app_main(void)
{
    xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);
}
```
ผลการรัน
โปรแกรมจะเรียก My_First_Task method เพื่อแสดงคำว่า Hello My First Task ตามด้วยเลขครั้ง

![ภาพ](https://github.com/Sittinon-Sawatdemongkol/ESP32-FreeRTOS-Intro/assets/115066278/d4298c16-c7ad-4163-a52e-2068e18fa336)

## Lab 2

เมื่อแก้ไขโค้ดโปรแกรมเป็นดังนี้
```css
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>

#include "freertos/FreeRTOS.h"
#include "freertos/task.h"

TaskHandle_t MyFirstTaskHandle = NULL;

void My_First_Task(void *arg)
{
    uint32_t i = 0;
    while (1)
    {
        printf("Hello My First Task %d\n", i);
        vTaskDelay(1000 / portTICK_RATE_MS);
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
โปรแกรมจะเรียก My_First_Task 5 ครั้งและ terminal จะแสดงข้อความ Hello My First Task ตามด้วย เลขเดียวกัน 5 ครั้ง

![ภาพ](https://github.com/Sittinon-Sawatdemongkol/ESP32-FreeRTOS-Intro/assets/115066278/84693467-7f26-455d-aadf-92053cb8fa5d)

## Lab 3

แก้ไขโปรแกรมเป้นดังนี้
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
โปรแกรมจะเรียก My_First_Task และ My_Second_Task method เพื่อแสดงคำว่า Hello My First Task และ Hello My Second Task ตามด้วยเลขครั้ง

![ภาพ](https://github.com/Sittinon-Sawatdemongkol/ESP32-FreeRTOS-Intro/assets/115066278/c00764f2-03f4-4e0b-bf4b-8e131839c15a)

แก้ไข app_main 
```css
void app_main(void)
{
	xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);
	// สร้างและเรียกใช้ task ด้วยฟังก์ชัน xTaskCreatePinnedToCore
	xTaskCreatePinnedToCore(My_Second_Task, "Second_Task", 4096, NULL, 10, &MySeconeTaskHandle, 1);
}
```
โปรแกรมจะเรียก My_First_Task และ My_Second_Task method เพื่อแสดงคำว่า Hello My First Task และ Hello My Second Task ตามด้วยเลขครั้งที่แสดง โดย First และ Second จะแสดงพร้อมกัน

![ภาพ](https://github.com/Sittinon-Sawatdemongkol/ESP32-FreeRTOS-Intro/assets/115066278/d054794f-7dba-47ea-8b18-6d7765a68105)

## Lab 4

เมื่อแก้ไขโปรแกรมคาดว่าโปรแกรมจะทำงาน 5 ครั้งจะแสดงข้อความว่า Second Task deleted และแสดงผลเแพาะข้อความที่ของ My_First_Task
```css
void My_First_Task(void * arg)
{
	uint32_t i = 0;
	while(1)
	{
		printf("Hello My First Task %d\n",i);
		vTaskDelay(1000/portTICK_RATE_MS);
		i++;

		if(i == 5)
		{
			vTaskDelete(MySecondTaskHandle);
			printf("Second Task deleted\n");
		}
	}
}
```
โปรแกรมจะทำงาน 5 ครั้ง และลบ MySecondTaskHandle จากนั้นจะทำงานต่อเฉพาะ MyFirstTaskHandle

![ภาพ](https://github.com/Sittinon-Sawatdemongkol/ESP32-FreeRTOS-Intro/assets/115066278/be9f0854-b834-4c1d-acad-c03d5bd52a7e)

## Lab 5 
โปรแกรมจะทำงาน MyFirstTaskHandle และ MySecondTaskHandle 5 ครั้ง จากนั้นจะหยุดการทำงานของ MySecondTaskHandle และเมื่อ MyFirstTaskHandle ทำงานครบ 10 ครั้ง MySecondTaskHandle จะกลับมาทำงานต่อ เมื่อ MyFirstTaskHandle ทำงานครบ 20 ครั้งจะหยุดทำงานทั้ง MyFirstTaskHandle MySecondTaskHandle
```css
Hello My First Task 0
I (332) main_task: Returned from app_main()
Hello My Second Task 0
Hello My First Task 1
Hello My Second Task 1
Hello My First Task 2
Hello My Second Task 2
Hello My First Task 3
Hello My Second Task 3
Hello My First Task 4
Hello My Second Task 4
Second Task suspended
Hello My First Task 5
Hello My First Task 6
Hello My First Task 7
Hello My First Task 8
Hello My First Task 9
Second Task Resumed
Hello My First Task 10
Hello My Second Task 5
Hello My First Task 11
Hello My Second Task 6
Hello My First Task 12
Hello My Second Task 7
Hello My First Task 13
Hello My Second Task 8
Hello My First Task 14
Hello My Second Task 9
Second Task deleted
Hello My First Task 15
Hello My First Task 16
Hello My First Task 17
Hello My First Task 18
Hello My First Task 19
MyFirstTaskHandle will suspend itself
```

## Lab 6

เมื่อกดปุ่ม LED จะติดและแสดงผลว่าปุ่มถูกกดโดยจะเรียก method interrupt_task ที่เป็นตัวกำหนดว่า LED จะติดหรือดับ

![ภาพ](https://github.com/Sittinon-Sawatdemongkol/ESP32-FreeRTOS-Intro/assets/115066278/0c822292-71a2-4d41-9d7a-0003b08f1aa9)

## Lab 7

โปรแกรมะแสดงผลข้อความ Received data from queue == จาก Task2 และตามด้วยข้อความ Hello from Demo_Task 1 2 3 ตามลำดับจนครบจากนั้นจะวนลูปไม่รู้จบ

![ภาพ](https://github.com/Sittinon-Sawatdemongkol/ESP32-FreeRTOS-Intro/assets/115066278/f9e22544-8d42-46da-890f-989d052aefc7)

## Lab 8

โปรแกรมจะรอรับการกดปุ่ม หากกดปุ่มโปรแกรมจะแสดงผลว่าปุ่มถูกกดโดยจะเรียก method Task เพื่อตรวจสอบการกดปุ่ม

![ภาพ](https://github.com/Sittinon-Sawatdemongkol/ESP32-FreeRTOS-Intro/assets/115066278/fb573365-77dd-4bc0-8c25-ad89a29f6b92)

