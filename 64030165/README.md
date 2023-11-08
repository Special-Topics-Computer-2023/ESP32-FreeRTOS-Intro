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

## Lab 5 
โปรแกรมจะทำงาน MyFirstTaskHandle และ MySecondTaskHandle 5 ครั้ง จากนั้นจะหยุดการทำงานของ MySecondTaskHandle และเมื่อ MyFirstTaskHandle ทำงานครบ 10 ครั้ง MySecondTaskHandle จะกลับมาทำงานต่อ เมื่อ MyFirstTaskHandle ทำงานครบ 20 ครั้งจะหยุดทำงานทั้ง MyFirstTaskHandle MySecondTaskHandle

## Lab 6

เมื่อกดปุ่ม LED จะติดและแสดงผลว่าปุ่มถูกกดโดยจะเรียก method interrupt_task ที่เป็นตัวกำหนดว่า LED จะติดหรือดับ

## Lab 7

โปรแกรมะแสดงผลข้อความ Received data from queue == จาก Task2 และตามด้วยข้อความ Hello from Demo_Task 1 2 3 ตามลำดับจนครบจากนั้นจะวนลูปไม่รู้จบ

## Lab 8

โปรแกรมจะรอรับการกดปุ่ม หากกดปุ่มโปรแกรมจะแสดงผลว่าปุ่มถูกกดโดยจะเรียก method Task เพื่อตรวจสอบการกดปุ่ม
