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
    	printf("Hello My First Task %lu\n", i);
    	vTaskDelay(1000 / portTICK_PERIOD_MS);
        i++;
    }
}
void app_main(void)
{
    xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);
}
```
## ผลลัพธ์
![image](https://github.com/TanapatPluemchai/ESP32-FreeRTOS-Intro/assets/115067806/80e6b800-63ee-4962-bf36-599dea3710b8)


## Free RTOS LAB2
```css
#include <unistd.h>

#include "freertos/FreeRTOS.h"
#include "freertos/task.h"

TaskHandle_t MyFirstTaskHandle = NULL;

void My_First_Task(void *arg)
{
    uint32_t i = 0;
    while (1)
    {
    	printf("Hello My First Task %lu\n", i);
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
## ผลลัพธ์
![image](https://github.com/TanapatPluemchai/ESP32-FreeRTOS-Intro/assets/115067806/bd7a4669-ecdf-4a00-b50c-78a1d5ec7dc6)



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
		printf("Hello My First Task %lu\n", i);
		vTaskDelay(1000 / portTICK_PERIOD_MS);
		i++;
	}
}

// 2. เพิ่ม task function
void My_Second_Task(void * arg)
{
	uint32_t i = 0;
	while(1)
	{
		 printf("Hello My Second Task %lu\n", i);
		 vTaskDelay(1000 / portTICK_PERIOD_MS);
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
## ผลลัพธ์
![image](https://github.com/TanapatPluemchai/ESP32-FreeRTOS-Intro/assets/115067806/b9ffe466-89b6-474c-97e8-967a76f5b5d2)

## Free RTOS LAB4

```css
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>

#include "freertos/FreeRTOS.h"
#include "freertos/task.h"

TaskHandle_t MyFirstTaskHandle = NULL;
TaskHandle_t MySecondTaskHandle = NULL; // Corrected typo in variable name

void My_First_Task(void *arg)
{
    uint32_t i = 0;
    while (1)
    {
        printf("Hello My First Task %lu\n", i);
        vTaskDelay(1000 / portTICK_PERIOD_MS);
        i++;

        if (i == 5)
        {
            vTaskDelete(MySecondTaskHandle);
            printf("Second Task deleted\n");
        }
    }
}

void My_Second_Task(void *arg) // Corrected function name
{
    uint32_t i = 0;
    while (1)
    {
        printf("Hello My Second Task %lu\n", i);
        vTaskDelay(1000 / portTICK_PERIOD_MS);
        i++;
    }
}

void app_main(void)
{
    xTaskCreate(My_First_Task, "First_Task", 4096, NULL, 10, &MyFirstTaskHandle);
    xTaskCreate(My_Second_Task, "Second_Task", 4096, NULL, 10, &MySecondTaskHandle); // Corrected typo in task name
}

```

## ผลลัพธ์
![image](https://github.com/TanapatPluemchai/ESP32-FreeRTOS-Intro/assets/115067806/0152e587-cf0c-4b0b-8483-17b095afd5c8)

## Free RTOS LAB5
```css
#include <stdio.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "freertos/semphr.h"

TaskHandle_t MySecondTaskHandle;
TaskHandle_t MyThirdTaskHandle;  // เพิ่มตัวแปรสำหรับ MyThirdTask

void My_Second_Task(void * arg)
{
    uint32_t j = 0;
    while(1)
    {
    	printf("Hello My Second Task %lu\n", j);
    	vTaskDelay(1000 / portTICK_PERIOD_MS);
        j++;
    }
}

void My_Third_Task(void * arg)
{
    uint32_t k = 0;
    while(1)
    {
    	printf("Hello My Second Task %lu\n",kA);
    	vTaskDelay(1000 / portTICK_PERIOD_MS);
        k++;
    }
}

void My_First_Task(void * arg)
{
    uint32_t i = 0;

    // สร้าง MySecondTask
    xTaskCreate(&My_Second_Task, "MySecondTask", 2048, NULL, 1, &MySecondTaskHandle);

    // สร้าง MyThirdTask
    xTaskCreate(&My_Third_Task, "MyThirdTask", 2048, NULL, 1, &MyThirdTaskHandle);

    while(1)
    {
    	printf("Hello My Second Task %lu\n", i);
    	vTaskDelay(1000 / portTICK_PERIOD_MS);
        i++;

        if(i == 5)
        {
            vTaskSuspend(MySecondTaskHandle);
            printf("Second Task suspended\n");
        }

        if(i == 10)
        {
            vTaskResume(MySecondTaskHandle);
            printf("Second Task Resumed\n");
        }

        if(i == 15)
        {
            vTaskDelete(MySecondTaskHandle);
            printf("Second Task deleted\n");
        }

        if(i == 20)
        {
            xTaskCreate(&My_Second_Task, "MySecondTask", 2048, NULL, 1, &MySecondTaskHandle);
            printf("Second Task recreated\n");
        }

        if(i == 25)
        {
            printf("MyFirstTaskHandle will suspend itself\n");
            vTaskSuspend(NULL);
        }
    }
}

void app_main()
{
    xTaskCreate(&My_First_Task, "MyFirstTask", 2048, NULL, 1, NULL);
}

```
## ผลลัพธ์
![image](https://github.com/TanapatPluemchai/ESP32-FreeRTOS-Intro/assets/115067806/1987433c-702b-4640-9bf8-0db7de6cb4f3)

## Free RTOS LAB6
ผลลัพธ์
เมื่อกดปุ่ม LED จะติด และ เมื่อกดซ้ำจะดับ

![image](https://github.com/TanapatPluemchai/ESP32-FreeRTOS-Intro/assets/115067806/0e9ca6c1-f3ed-4664-a476-44fbe6eb1d16)

## Free RTOS LAB7
ผลลัพธ์
![image](https://github.com/TanapatPluemchai/ESP32-FreeRTOS-Intro/assets/115067806/469dfea5-c4d8-4fd1-bf54-b9e46fc8b414)



## Free RTOS LAB8
ผลลัพธ์
![image](https://github.com/TanapatPluemchai/ESP32-FreeRTOS-Intro/assets/115067806/f1af3056-d0de-4c3b-8946-5b262ee4fb2d)






