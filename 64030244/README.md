# ESP32-FreeRTOS-Intro

###Lab 1


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

![Screenshot 2023-11-09 204504](https://github.com/CHAIYAPRUK/ESP32-FreeRTOS-Intro/assets/115066395/7a495928-9f83-4de8-b7db-ad3e0f994bee)

###Lab 2

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

ผลการรัน โปรแกรมจะเรียก My_First_Task 5 ครั้งและ terminal จะแสดงข้อความ Hello My First Task ตามด้วย เลขเดียวกัน 5 ครั้ง

![Screenshot 2023-11-09 204854](https://github.com/CHAIYAPRUK/ESP32-FreeRTOS-Intro/assets/115066395/1d714180-6e63-4aa2-bb5b-1d7f7dbec8f8)

###Lab 3

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
ผลการรัน โปรแกรมจะเรียก My_First_Task และ My_Second_Task method เพื่อแสดงคำว่า Hello My First Task และ Hello My Second Task ตามด้วยเลข

![Screenshot 2023-11-09 205032](https://github.com/CHAIYAPRUK/ESP32-FreeRTOS-Intro/assets/115066395/f7799376-2053-4dfb-bf3a-ecab1387f03a)

แก้ไข code ในส่วนของการสร้าง task 2

```css
void app_main(void)
{
	xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);
	// สร้างและเรียกใช้ task ด้วยฟังก์ชัน xTaskCreatePinnedToCore
	xTaskCreatePinnedToCore(My_Second_Task, "Second_Task", 4096, NULL, 10, &MySeconeTaskHandle, 1);
}
```

โดย First และ Second จะแสดงพร้อมกัน

![Screenshot 2023-11-09 205154](https://github.com/CHAIYAPRUK/ESP32-FreeRTOS-Intro/assets/115066395/7f72451e-963f-4239-93b5-023f163ff3c0)

###Lab 4

แก่ไข Code จากโปรแกรมที่แล้ว โดยการเพิ่ม task เข้าไปอีก 1 task

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
ผลการรัน โปรแกรมจะทำงาน 5 ครั้ง และลบ MySecondTaskHandle


![Screenshot 2023-11-09 210002](https://github.com/CHAIYAPRUK/ESP32-FreeRTOS-Intro/assets/115066395/6eb736f0-98a3-4559-b4cd-45798efc54c8)

###Lab5

แก่ไข Code จากโปรแกรมที่แล้ว โดยการเพิ่ม task เข้าไปอีก 1 task

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
			printf("MyFirstTaskHandle will suspend itself\n");
			vTaskSuspend(NULL);
		}
	}
}
```

ผลการรัน

![Screenshot 2023-11-09 210229](https://github.com/CHAIYAPRUK/ESP32-FreeRTOS-Intro/assets/115066395/2503e0c9-cfd8-45f9-b17e-d6019301290d)

จะทำงาน MyFirstTaskHandle และ MySecondTaskHandle 5 ครั้ง จากนั้นจะหยุดการทำงานของ MySecondTaskHandle และเมื่อ MyFirstTaskHandle ทำงานครบ 10 ครั้ง MySecondTaskHandle จะกลับมาทำงานต่อ เมื่อ MyFirstTaskHandle ทำงานครบ 20 ครั้งจะหยุดทำงานทั้ง MyFirstTaskHandle MySecondTaskHandle

###Lab6

```css
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>

#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "driver/gpio.h"

#define PUSH_BUTTON_PIN GPIO_NUM_33
#define LED_PIN GPIO_NUM_27

TaskHandle_t ISR = NULL;

void interrupt_task(void *arg)
{
  bool led_status = false;
  while (1)
  {
    vTaskSuspend(NULL);
    led_status = !led_status;
    gpio_set_level(LED_PIN, led_status);
    printf("Button pressed!\n");
  }
}

void IRAM_ATTR button_isr_handler(void *arg)
{
  xTaskResumeFromISR(ISR);
}

void app_main(void)
{
  gpio_pad_select_gpio(PUSH_BUTTON_PIN);
  gpio_pad_select_gpio(LED_PIN);

  gpio_set_direction(PUSH_BUTTON_PIN, GPIO_MODE_INPUT);
  gpio_set_direction(LED_PIN, GPIO_MODE_OUTPUT);

  gpio_set_intr_type(PUSH_BUTTON_PIN, GPIO_INTR_POSEDGE);

  gpio_install_isr_service(0);

  gpio_isr_handler_add(PUSH_BUTTON_PIN, button_isr_handler, NULL);

  xTaskCreate(interrupt_task, "interrupt_task", 4096, NULL, 10, &ISR);
}
```

ผลการรัน กดปุ่ม LED ไฟติดติด/ดับ จะขึ้นคำว่า Button pressed!

![Screenshot 2023-11-09 213638](https://github.com/CHAIYAPRUK/ESP32-FreeRTOS-Intro/assets/115066395/d6715445-0300-47a9-8982-81360c8a6a16)

###Lab7

```css
#include <stdio.h>
#include <freertos/FreeRTOS.h>
#include <freertos/task.h>
#include "freertos/queue.h"

TaskHandle_t myTaskHandle = NULL;
TaskHandle_t myTaskHandle2 = NULL;
QueueHandle_t queue;

void Task1(void *arg)
{
    // 1. จองพื้นที่สำหรับสร้างข้อมูลที่จะส่งผ่าน queue จำนวน 50 ตัวอักษร
    char txBuffer[50];

    // 2. สร้าง queue แล้วเก็บไว้ในตัวแปร handle
    queue = xQueueCreate(5, sizeof(txBuffer));
    // 3. ทดสอบว่าสร้าง queue สำเร็จหรือไม่
    if (queue == 0)
    {
     printf("Failed to create queue= %p\n", queue);
    }

    // 4. เตรียมข้อมูลและส่งผ่าน queue ทำเป็นจำนวน 3 รอบ
    // queue producerใช้ฟังก์ชัน xQueueSend
    sprintf(txBuffer, "Hello from Demo_Task 1");
    xQueueSend(queue, (void*)txBuffer, (TickType_t)0);

    sprintf(txBuffer, "Hello from Demo_Task 2");
    xQueueSend(queue, (void*)txBuffer, (TickType_t)0);

    sprintf(txBuffer, "Hello from Demo_Task 3");
    xQueueSend(queue, (void*)txBuffer, (TickType_t)0);

    // 5. ส่งครบ 3 ครั้ง ให้วนลูปไม่รู้จบ ไม่ต้องทำอะไรเพิ่ม
    while(1)
    {
        vTaskDelay(1000/ portTICK_RATE_MS);
    }
}

void Task2(void *arg)
{
  // 1. เตรียมพื้นที่รับข้อมูลผ่าน queue
	char rxBuffer[50];
  // 2. วนลูปรอ
	while (1)
	{
    // 3. ถ้ามีการส่งข้อมูลผ่าน queue handle ที่รอรับ ให้เอาข้อมูลไปแสดงผล
    // queue consumer จะใช้ฟังก์ชัน xQueueReceive
		if (xQueueReceive(queue, &(rxBuffer), (TickType_t) 5))
		{
			printf("Received data from queue == %s\n", rxBuffer);
			vTaskDelay(1000 / portTICK_RATE_MS);

		}
	}
}
void app_main(void)
{
	xTaskCreate(Task1, "Task_1", 4096, NULL, 10, &myTaskHandle);
	xTaskCreatePinnedToCore(Task2, "Task_2", 4096, NULL, 10, &myTaskHandle2, 1);
}
```

ผลการรัน จะแสดง Received data from queue == จาก Task2 และตามด้วยข้อความ Hello from Demo_Task 1 2 3

![Screenshot 2023-11-09 214159](https://github.com/CHAIYAPRUK/ESP32-FreeRTOS-Intro/assets/115066395/9f9ac360-d270-4b9c-a7c3-b49e4dbff43e)

###Lab8


```css
#include <freertos/FreeRTOS.h>
#include <freertos/task.h>
#include "freertos/queue.h"
#include "driver/gpio.h"

#define LED_PIN 27
#define PUSH_BUTTON_PIN 33

TaskHandle_t myTaskHandle = NULL;
QueueHandle_t queue;

void Task(void *arg)
{
	// 1. เตรียมพื้นที่รับข้อมูลผ่าน queue
	char rxBuffer;
	// 2. วนลูปรอ
	while (1)
	{
		// 3. ถ้ามีการส่งข้อมูลผ่าน queue handle ที่รอรับ ให้เอาข้อมูลไปแสดงผล
		// queue consumer จะใช้ฟังก์ชัน xQueueReceive
		if (xQueueReceive(queue, &(rxBuffer), (TickType_t) 5))
		{
			printf("Button pressed!\n");
			vTaskDelay(1000 / portTICK_RATE_MS);
		}
	}
}
void IRAM_ATTR button_isr_handler(void *arg)
{
	// code จาก https://www.freertos.org/a00119.html
	char cIn;
	BaseType_t xHigherPriorityTaskWoken;
	/* We have not woken a task at the start of the ISR. */
	xHigherPriorityTaskWoken = pdFALSE;
	cIn = '1';
	xQueueSendFromISR(queue, &cIn, &xHigherPriorityTaskWoken);
}

void app_main(void)
{
	gpio_pad_select_gpio(PUSH_BUTTON_PIN);
	gpio_pad_select_gpio(LED_PIN);

	gpio_set_direction(PUSH_BUTTON_PIN, GPIO_MODE_INPUT);
	gpio_set_direction(LED_PIN, GPIO_MODE_OUTPUT);

	gpio_set_intr_type(PUSH_BUTTON_PIN, GPIO_INTR_POSEDGE);

	gpio_install_isr_service(0);

	gpio_isr_handler_add(PUSH_BUTTON_PIN, button_isr_handler, NULL);

	// สร้าง queue ที่บรรจุตัวแปร char จำนวน 1 ตัว
	queue = xQueueCreate(1, sizeof(char));

	// ทดสอบว่าสร้าง queue สำเร็จ?
	if (queue == 0)
	{
		printf("Failed to create queue= %p\n", queue);
	}

	xTaskCreatePinnedToCore(Task, "My_Task", 4096, NULL, 10, &myTaskHandle, 1);
}
```

ผลการรัน โปรแกรมจะรอรับการกดปุ่ม หากกดปุ่มโปรแกรมจะแสดงผลว่า Button pressed!

![Screenshot 2023-11-09 214738](https://github.com/CHAIYAPRUK/ESP32-FreeRTOS-Intro/assets/115066395/6341de44-0637-4525-aa8c-1733147eac28)



