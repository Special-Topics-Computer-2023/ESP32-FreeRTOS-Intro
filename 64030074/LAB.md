# 64030074 Thani Paknam

## LAB1

แสดงข้อความ Hello My First Task หน่วงเวลา 1 วินาทีและเพิ่มค่า i ทีละ 1

![image](https://github.com/tnpn2545/ESP32-FreeRTOS-Intro/assets/115066414/fa203d16-733a-4c49-8a83-510d44b30609)

## LAB2
```c++
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>

#include "freertos/FreeRTOS.h"
#include "freertos/task.h"

TaskHandle_t MyFirstTaskHandle = NULL;

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

void app_main(void)
{
	xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);
	xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);
	xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);
	xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);
	xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);

}
```
ผลลัพธ์
โปรแกรมจะเรียก My_First_Task 5 ครั้งและ terminal จะแสดงข้อความ Hello My First Task ตามด้วย เลขเดียวกัน 5 ครั้ง

![image](https://github.com/tnpn2545/ESP32-FreeRTOS-Intro/assets/115066414/0465bec5-224d-4fb8-8b5a-213adcfa59e0)
## LAB3
```c++
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
ผลลัพธ์
โปรแกรมจะเรียก My_First_Task และ My_Second_Task method เพื่อแสดงคำว่า Hello My First Task และ Hello My Second Task ตามด้วยเลขครั้งที่แสดง

![image](https://github.com/tnpn2545/ESP32-FreeRTOS-Intro/assets/115066414/a96a2be2-2b27-4b08-8f91-c873c4bf42b6)
แก้ไข app_main
```c++
void app_main(void)
{
	xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);
	// สร้างและเรียกใช้ task ด้วยฟังก์ชัน xTaskCreatePinnedToCore
	xTaskCreatePinnedToCore(My_Second_Task, "Second_Task", 4096, NULL, 10, &MySeconeTaskHandle, 1);
}
```
ผลลัพธ์
โปรแกรมจะเรียก My_First_Task และ My_Second_Task method เพื่อแสดงคำว่า Hello My First Task และ Hello My Second Task ตามด้วยเลขครั้งที่แสดง โดย First และ Second จะแสดงพร้อมกัน

![image](https://github.com/tnpn2545/ESP32-FreeRTOS-Intro/assets/115066414/b3e40f86-df03-4d7d-accf-3d3979dfbb56)
## LAB 4
แก่ไข Code จากโปรแกรมที่แล้ว โดยการเพิ่ม task เข้าไปอีก 1 task
```c++
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>

#include "freertos/FreeRTOS.h"
#include "freertos/task.h"

TaskHandle_t MyFirstTaskHandle = NULL;
// 1. เพิ่ม MySeconeTaskHandle
TaskHandle_t MySecondTaskHandle = NULL;

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
	// สร้างและเรียกใช้ task ด้วยฟังก์ชัน xTaskCreatePinnedToCore
	xTaskCreatePinnedToCore(My_Second_Task, "Second_Task", 4096, NULL, 10, &MySecondTaskHandle, 1);
}
```
ผลลัพธ์
จากโปรแกรมด้านบน เมื่อมีการนับจน i == 5 จะทำให้เงื่อนไขในประโยค if เป็นจริง
คำสั่ง vTaskDelete(MySecondTaskHandle); จะลบ MySecondTaskHandle ออกจาก list ของ task ที่จะทำงาน เมื่อโปรแกรมทำงานครบ 5 ครั้ง จะทำการลบ MySecondTaskHandle

![image](https://github.com/tnpn2545/ESP32-FreeRTOS-Intro/assets/115066414/7f639e3e-586e-47d6-b5ec-5b14fd914657)
## LAB5
โปรแกรมจะเริ่มทำงาน เมื่อเข้าครั้งที่ 5 จะทำการเรียกใช้ vTaskSuspend() ใช้หยุดทำงาน MySecondTask เมื่อเข้ารอบที่ 10 จะทำการเรียกใช้ vTaskResume() ทำให้ MySecondTask กลับมาทำงาน เมื่อเข้ารอบที่ 15 จะทำการเรียกใช้ vTaskDelete() ทำให้ MySecondTask โดนลบ เมื่อเข้ารอบ

![image](https://github.com/tnpn2545/ESP32-FreeRTOS-Intro/assets/115066414/fe87d5e3-243e-4486-ae72-34789266779f)
## LAB6
เมื่อกดปุ่ม LED จะติดและแสดงผลว่าปุ่มถูกกด

![image](https://github.com/tnpn2545/ESP32-FreeRTOS-Intro/assets/115066414/cf24c102-adfc-4826-a21d-9d8016c52553)
## LAB7
![image](https://github.com/tnpn2545/ESP32-FreeRTOS-Intro/assets/115066414/9ea44e24-8bc7-4753-8231-f90792b9e1ca)

โปรแกรมะแสดงผลข้อความ Received data from queue == จาก Task2 และตามด้วยข้อความ Hello from Demo_Task 1 2 3 ตามลำดับจนครบจากนั้นจะวนลูปไม่รู้จบ
## LAB8
![image](https://github.com/tnpn2545/ESP32-FreeRTOS-Intro/assets/115066414/a9a931b7-d39b-4bfe-9814-47701321c3a2)

โปรแกรมจะรอรับการกดปุ่ม หากกดปุ่มโปรแกรมจะแสดงผลว่าปุ่มถูกกดโดยจะเรียก method Task เพื่อตรวจสอบการกดปุ่ม
