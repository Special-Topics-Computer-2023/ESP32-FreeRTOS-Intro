# ESP32-FreeRTOS-Intro
## LAB1

```css
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
}
```

ผลการรันโปรแกรม

![image](https://github.com/sucha312/ESP32-FreeRTOS-Intro/assets/115066208/f0f930b6-1c8c-4f1b-b3ae-efc3d2244187)

## LAB2

```css
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

ผลการรันโปรแกรมคือ จะแสดงเลขตัวเดิมซ้ำ 5 ครั้ง

![image](https://github.com/sucha312/ESP32-FreeRTOS-Intro/assets/115066208/5336d071-4960-4067-a563-5b46368047b7)

## LAB3

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

ผลจากการรันโปรแกรม

![image](https://github.com/sucha312/ESP32-FreeRTOS-Intro/assets/115066208/f0e38653-c32c-41e9-aa1e-b66a54dc1021)


แก้ไขโค้ด

![image](https://github.com/sucha312/ESP32-FreeRTOS-Intro/assets/115066208/324bf715-478b-4609-9cc9-038c5fc9b6e1)

## LAB4

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

![image](https://github.com/sucha312/ESP32-FreeRTOS-Intro/assets/115066208/c84fc868-4515-4c76-826d-f9d095767fa9)


## LAB5

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
