# 64030165 Ratchanon
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

![image](https://github.com/RatchanonBusaracome/ESP32-FreeRTOS-Intro/assets/115066405/fb44e1b0-eb7b-42f1-8803-b5e58d22a743)

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

![image](https://github.com/RatchanonBusaracome/ESP32-FreeRTOS-Intro/assets/115066405/d9f95a9a-6588-4bfb-81c1-40acc05d2140)


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

![image](https://github.com/RatchanonBusaracome/ESP32-FreeRTOS-Intro/assets/115066405/c35b670e-72a2-4ac1-ada6-817542764ffd)

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

![image](https://github.com/RatchanonBusaracome/ESP32-FreeRTOS-Intro/assets/115066405/7f7dfcd0-1edf-4b26-a547-49cc45b4e10d)

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

![image](https://github.com/RatchanonBusaracome/ESP32-FreeRTOS-Intro/assets/115066405/ff092b6c-428a-4058-a459-9ebe7f195560)


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

![image](https://github.com/RatchanonBusaracome/ESP32-FreeRTOS-Intro/assets/115066405/da51587f-da8b-4681-b317-235ab3a59671)


## Lab 7

โปรแกรมะแสดงผลข้อความ Received data from queue == จาก Task2 และตามด้วยข้อความ Hello from Demo_Task 1 2 3 ตามลำดับจนครบจากนั้นจะวนลูปไม่รู้จบ

![image](https://github.com/RatchanonBusaracome/ESP32-FreeRTOS-Intro/assets/115066405/232f7700-eff2-47c3-9e5b-c415b4f5a5c3)


## Lab 8

โปรแกรมจะรอรับการกดปุ่ม หากกดปุ่มโปรแกรมจะแสดงผลว่าปุ่มถูกกดโดยจะเรียก method Task เพื่อตรวจสอบการกดปุ่ม

![image](https://github.com/RatchanonBusaracome/ESP32-FreeRTOS-Intro/assets/115066405/0ca92a2d-04b6-4e54-910d-4fcd7dda8555)

