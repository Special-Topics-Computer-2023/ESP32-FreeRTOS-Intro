# 64030318 Suwit 
## Free RTOS LAB1
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

![image](https://github.com/Suthera213/ESP32-FreeRTOS-Intro/assets/115066359/5f975748-dab3-43e4-a1a4-90bf0ea44796)


## Free RTOS LAB2
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

ผลการรัน  

![image](https://github.com/Suthera213/ESP32-FreeRTOS-Intro/assets/115066359/f78f2244-57f1-477a-a654-425932ff8594)


## Free RTOS LAB3
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

ผลการรัน  

![image](https://github.com/Suthera213/ESP32-FreeRTOS-Intro/assets/115066359/2dd85659-ccff-4281-a9ad-b05f5675e65d)


## Free RTOS LAB4
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

ผลการรัน  

![image](https://github.com/Suthera213/ESP32-FreeRTOS-Intro/assets/115066359/873323bf-ee54-4639-bc42-a5e1839f5384)


## Free RTOS LAB5
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

ผลการรัน  

![image](https://github.com/Suthera213/ESP32-FreeRTOS-Intro/assets/115066359/9457224b-5438-4c9b-b44f-0b024d95d8d6)


## Free RTOS LAB6
ถ้ากดปุ่ม LED จะติดและแสดงผลว่าปุ่มถูกกด  

![image](https://github.com/Suthera213/ESP32-FreeRTOS-Intro/assets/115066359/c7e273fe-f996-4a6f-8c51-573be9597015)

## Free RTOS LAB7
โปรแกรมจะทำงาน loop แสดงผลข้อความ Received data from queue == จาก Task2 และตามด้วยข้อความ Hello from Demo_Task 1 2 3  

![image](https://github.com/Suthera213/ESP32-FreeRTOS-Intro/assets/115066359/e9e6d08b-50d1-465e-854d-edb79e139c0b)
