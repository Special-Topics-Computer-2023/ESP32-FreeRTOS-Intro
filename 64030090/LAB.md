# 64030090 NANAPON

แสดงข้อความ Hello My First Task หน่วงเวลา 1 วินาทีและเพิ่มค่า i ทีละ 1
## LAB1
![image](https://github.com/Nanapon2002/ESP32-FreeRTOS-Intro/assets/115066356/5b473c01-310f-4ea1-ace3-5fcdabdfdfcb)


## LAB2
## คำถาม 
1. ถ้าแก้ไข code เป็นดังต่อไปนี้ จะได้ผลลัพธ์เป็นอย่างไร

```c
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
โปรแกรมจะเรียก My_First_Task 5 ครั้งและ terminal จะแสดงข้อความ Hello My First Task ตามด้วย เลขเดียวกัน 5 ครั้ง
![image](https://github.com/Nanapon2002/ESP32-FreeRTOS-Intro/assets/115066356/f376ef37-ce01-4827-bbeb-e11edb4b6fed)

## LAB3
1. แก่ไข Code จากโปรแกรมที่แล้ว โดยการเพิ่ม task เข้าไปอีก 1 task (ดู comment ใน code)
```c
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>

#include "freertos/FreeRTOS.h"
#include "freertos/task.h"

TaskHandle_t MyFirstTaskHandle = NULL;
TaskHandle_t MySecondTaskHandle = NULL; // 1. Fix typo

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

void My_Second_Task(void *arg) // 2. Add task function
{
    uint32_t i = 0;
    while (1)
    {
        printf("Hello My Second Task %d\n", i);
        vTaskDelay(1000 / portTICK_RATE_MS);
        i++;
    }
}

void app_main(void)
{
    xTaskCreate(My_First_Task, "First_Task", 4096, NULL, 10, &MyFirstTaskHandle);
    xTaskCreate(My_Second_Task, "Second_Task", 4096, NULL, 10, &MySecondTaskHandle); // 3. Create task for My_Second_Task
}

```
โปรแกรมจะเรียก My_First_Task และ My_Second_Task ตามด้วยแสดงตัวเลขจำนวนครั้ง

![image](https://github.com/Nanapon2002/ESP32-FreeRTOS-Intro/assets/115066356/f283195a-e3d5-4da7-9fc9-b60dafba78f4)

3.  แก้ไข code ในส่วนของการสร้าง task 2 (ตามหมายเหตุหมายเลข 3) เป็นดังนี้

```c
void app_main(void)
{
	xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);
	// สร้างและเรียกใช้ task ด้วยฟังก์ชัน xTaskCreatePinnedToCore
	xTaskCreatePinnedToCore(My_Second_Task, "Second_Task", 4096, NULL, 10, &MySeconeTaskHandle, 1);
}
```
โปรแกรมจะเรียก My_First_Task และ My_Second_Task method เพื่อแสดงคำว่า Hello My First Task และ Hello My Second Task ตามด้วยเลขครั้งที่แสดง
แต่จะ My First Task และ My Second Task พร้อมกัน
![image](https://github.com/Nanapon2002/ESP32-FreeRTOS-Intro/assets/115066356/b30f84b4-f9f9-4da1-b374-5a1777f230ed)



## LAB4
1. แก่ไข Code จากโปรแกรมที่แล้ว โดยการเพิ่ม task เข้าไปอีก 1 task

```c
...

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

...
```

จากโปรแกรมด้านบน เมื่อมีการนับจน i == 5 จะทำให้เงื่อนไขในประโยค if เป็นจริง

คำสั่ง `vTaskDelete(MySecondTaskHandle);` จะลบ `MySecondTaskHandle` ออกจาก list ของ task ที่จะทำงาน
เมื่อโปรแกรมทำงานครบ 5 ครั้ง จะทำการลบ MySecondTaskHandle

![image](https://github.com/Nanapon2002/ESP32-FreeRTOS-Intro/assets/115066356/9371949b-1663-4a98-9a63-c84b3a63e69b)

## LAB5
โปรแกรมจะเริ่มทำงาน เมื่อเข้าครั้งที่ 5 จะทำการเรียกใช้ vTaskSuspend() ใช้หยุดทำงาน MySecondTask เมื่อเข้ารอบที่ 10 จะทำการเรียกใช้ vTaskResume() ทำให้ MySecondTask กลับมาทำงาน 
เมื่อเข้ารอบที่ 15 จะทำการเรียกใช้ vTaskDelete() ทำให้ MySecondTask โดนลบ เมื่อเข้ารอบที่ 20 จะหยุดการทำงานทั้ง MyfirstTask และ MySecondTask

![image](https://github.com/Nanapon2002/ESP32-FreeRTOS-Intro/assets/115066356/50ad6a2a-1400-4da4-84ba-6cdf221f286f)

## LAB6
เมื่อกดปุ่ม LED จะติดและแสดงผลว่าปุ่มถูกกดโดยจะเรียก method interrupt_task ที่เป็นตัวกำหนดว่า LED จะติดหรือดับ
![image](https://github.com/Nanapon2002/ESP32-FreeRTOS-Intro/assets/115066356/a48315a7-001e-48d3-895b-90adf6c1ef22)

โปรแกรมะแสดงผลข้อความ Received data from queue == จาก Task2 และตามด้วยข้อความ Hello from Demo_Task 1 2 3 ตามลำดับจนครบจากนั้นจะวนลูปไม่รู้จบ

![image](https://github.com/Nanapon2002/ESP32-FreeRTOS-Intro/assets/115066356/b1fbd7c4-fa26-4e4a-97cb-efce8e60473e)

## LAB8
โปรแกรมจะรอรับการกดปุ่ม หากกดปุ่มโปรแกรมจะแสดงผลว่าปุ่มถูกกดโดยจะเรียก method Task เพื่อตรวจสอบการกดปุ่ม

![image](https://github.com/Nanapon2002/ESP32-FreeRTOS-Intro/assets/115066356/87c5d8c2-fef3-4bba-8969-c70d62e49bbb)

