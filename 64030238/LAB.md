# 64030238 Jasda Singhapooti

แสดงข้อความ Hello My First Task หน่วงเวลา 1 วินาทีและเพิ่มค่า i ทีละ 1
## LAB1
```
I (36) boot.esp32: SPI Speed      : 40MHz
I (41) boot.esp32: SPI Mode       : DIO
I (45) boot.esp32: SPI Flash Size : 4MB
I (50) boot: Enabling RNG early entropy source...
I (55) boot: Partition Table:
I (59) boot: ## Label            Usage          Type ST Offset   Length
I (66) boot:  0 nvs              WiFi data        01 02 00009000 00006000
I (74) boot:  1 phy_init         RF data          01 01 0000f000 00001000
I (81) boot:  2 factory          factory app      00 00 00010000 00100000
I (89) boot: End of partition table
I (93) esp_image: segment 0: paddr=00010020 vaddr=3f400020 size=08814h ( 34836) map
I (114) esp_image: segment 1: paddr=0001883c vaddr=3ffb0000 size=01b78h (  7032) load
I (117) esp_image: segment 2: paddr=0001a3bc vaddr=40080000 size=05c5ch ( 23644) load
I (130) esp_image: segment 3: paddr=00020020 vaddr=400d0020 size=14f90h ( 85904) map
I (162) esp_image: segment 4: paddr=00034fb8 vaddr=40085c5c size=059b8h ( 22968) load
I (177) boot: Loaded app from partition at offset 0x10000
I (177) boot: Disabling RNG early entropy source...
I (188) cpu_start: Multicore app
I (188) cpu_start: Pro cpu up.
I (189) cpu_start: Starting app cpu, entry point is 0x40080fec
0x40080fec: call_start_cpu1 at C:/Users/gamem/esp/esp-idf/components/esp_system/port/cpu_start.c:151

I (0) cpu_start: App cpu up.
I (207) cpu_start: Pro cpu start user code
I (207) cpu_start: cpu freq: 160000000
I (207) cpu_start: Application information:
I (211) cpu_start: Project name:     LAB1
I (216) cpu_start: App version:      1
I (220) cpu_start: Compile time:     Oct 16 2023 11:32:39
I (226) cpu_start: ELF file SHA256:  0d234ea710095472...
I (232) cpu_start: ESP-IDF:          v4.4.6
I (237) cpu_start: Min chip rev:     v0.0
I (242) cpu_start: Max chip rev:     v3.99
I (247) cpu_start: Chip rev:         v3.0
I (252) heap_init: Initializing. RAM available for dynamic allocation:
I (259) heap_init: At 3FFAE6E0 len 00001920 (6 KiB): DRAM
I (265) heap_init: At 3FFB2468 len 0002DB98 (182 KiB): DRAM
I (271) heap_init: At 3FFE0440 len 00003AE0 (14 KiB): D/IRAM
I (277) heap_init: At 3FFE4350 len 0001BCB0 (111 KiB): D/IRAM
I (284) heap_init: At 4008B614 len 000149EC (82 KiB): IRAM
I (291) spi_flash: detected chip: generic
I (295) spi_flash: flash io: dio
I (300) cpu_start: Starting scheduler on PRO CPU.
I (0) cpu_start: Starting scheduler on APP CPU.
Hello My First Task 0
Hello My First Task 1
Hello My First Task 2
Hello My First Task 3
Hello My First Task 4
Hello My First Task 5
Hello My First Task 6
Hello My First Task 7
Hello My First Task 8
Hello My First Task 9
Hello My First Task 10
Hello My First Task 11
```
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
![image](https://github.com/JASDA0000/ESP32-FreeRTOS-Intro/assets/103983336/bb4944a3-1d63-4c94-8f4c-f07728b712f9)
## LAB3
1. แก่ไข Code จากโปรแกรมที่แล้ว โดยการเพิ่ม task เข้าไปอีก 1 task (ดู comment ใน code)
```c
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
```
โปรแกรมจะเรียก My_First_Task และ My_Second_Task ตามด้วยแสดงตัวเลขจำนวนครั้ง
![image](https://github.com/JASDA0000/ESP32-FreeRTOS-Intro/assets/103983336/bfbc9527-b3c0-4b61-b98c-65645507a3b6)
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
![image](https://github.com/JASDA0000/ESP32-FreeRTOS-Intro/assets/103983336/35a72ced-2eea-42f5-91fd-9d97aeafd6cd)

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
![image](https://github.com/JASDA0000/ESP32-FreeRTOS-Intro/assets/103983336/1fb995c6-1c20-463c-aa2c-69a0a76e38fb)
## LAB5
โปรแกรมจะเริ่มทำงาน เมื่อเข้าครั้งที่ 5 จะทำการเรียกใช้ vTaskSuspend() ใช้หยุดทำงาน MySecondTask เมื่อเข้ารอบที่ 10 จะทำการเรียกใช้ vTaskResume() ทำให้ MySecondTask กลับมาทำงาน 
เมื่อเข้ารอบที่ 15 จะทำการเรียกใช้ vTaskDelete() ทำให้ MySecondTask โดนลบ เมื่อเข้ารอบที่ 20 จะหยุดการทำงานทั้ง MyfirstTask และ MySecondTask
![image](https://github.com/JASDA0000/ESP32-FreeRTOS-Intro/assets/103983336/94db75de-3ebf-414b-bc93-ecd15714c8ed)
## LAB6
เมื่อกดปุ่ม LED จะติดและแสดงผลว่าปุ่มถูกกดโดยจะเรียก method interrupt_task ที่เป็นตัวกำหนดว่า LED จะติดหรือดับ
![image](https://github.com/JASDA0000/ESP32-FreeRTOS-Intro/assets/103983336/6ffe2ea8-d084-4a0e-8706-2c4e8729c484)
## LAB7
โปรแกรมะแสดงผลข้อความ Received data from queue == จาก Task2 และตามด้วยข้อความ Hello from Demo_Task 1 2 3 ตามลำดับจนครบจากนั้นจะวนลูปไม่รู้จบ
![image](https://github.com/JASDA0000/ESP32-FreeRTOS-Intro/assets/103983336/c75c0b98-fa9e-4511-b0ef-f7a8a74d2505)
## LAB8
โปรแกรมจะรอรับการกดปุ่ม หากกดปุ่มโปรแกรมจะแสดงผลว่าปุ่มถูกกดโดยจะเรียก method Task เพื่อตรวจสอบการกดปุ่ม
![image](https://github.com/JASDA0000/ESP32-FreeRTOS-Intro/assets/103983336/bc9f7acd-30e4-43b9-b3a9-86c5468a6647)

