# การทดลอง ESP32 FreeRTOS 
##  ฟังก์ชัน xTaskCreate()

การสร้างงานใหม่ขึ้นใน `app_main()` ทำได้โดยใช้ `xTaskCreate()` ซึ่งจะรับ อาร์กิวเมนต์หลายตัวด้วยกันได้แก่

1. ชื่อของฟังก์ชันสำหรับ task ซึ่งของเราตั้งชื่อเป็น `My_First_Task`

ฟังก์ชันที่เป็น task จะต้องเขียนตามรูปแบบต่อไปนี้

```c
void TaskName(void * arg)
{

}
```

2. ชื่อของ task เพื่อวัตถุประสงค์ในการจดจำได้ง่าย โดยปกติจะใช้ในการ debug  ซึ่งโปรแกรมจะมองเป็น string ดังนั้นเราต้องเขียนอยู่ภายใต้เครื่องหมายคำพูด `“Fitst_Task”`

3. ขนาดหน่วยความจำที่จะมอบให้แก่ task ในที่นี่้กำหนดเป็น `4096` ถ้าให้น้อยไป อาจจะไม่เพียงพอแก่การใช้งาน ถ้าให้มากไป อาจจะเหลือหน่วยความจำระบบน้อยกว่าที่จะรันระบบได้ 

4. พารามิเตอร์ที่จะส่งเป็นค่าเริ่มต้นให้กับ task ในที่นี้เราส่งเป็น `NULL` เนื่องจากไม่ต้องการใช้ 

5. Priority ของ task ในที่นี้กำหนดเป็น `10`

6. Handle ที่จะให้โปรแกรมใช้สั่งการต่างๆ ไปยัง task เช่นการ  suspend, resume, delete หรือทำการต่างๆ เกี่ยวกับ task ในที่นี้เราประกาศเป็น 

```c
TaskHandle_t MyFirstTaskHandle = NULL;
```
และใช้ `&MyFirstTaskHandle` เป็นอาร์กิวเมนต์ตัวที่ 6 ของ `xTaskCreate`


ดังนั้น ใน `app_main()`  จึงเรียกใช้เป็นดังนี้ 

```c
xTaskCreate(My_First_Task, "Fitst_Task", 4096, NULL, 10, &MyFirstTaskHandle);
```

##  ฟังก์ชัน void My_First_Task(void * arg)

```c
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
```


ฟังก์ชัน `void My_First_Task(void * arg)` จะมีลักษณะการใช้งานเหมือนฟังก์ชัน main() ในภาษาซีโดยทั่วไป นั้นคือมีอาร์กิวเมนต์ 1 ตัว เพื่อป้อนค่าให้กับฟังก์ชัน ซึ่งในตัวอย่างนี้เรายังไม่ได้นำมาใช้

ภายใน `void My_First_Task(void * arg)` จะมีการประกาศตัวแปรสำหรับนับค่า และมีลูป while เพื่อควบคุมให้ task นี้ทำงานแบบวนรอบไม่รู้จบ ซึ่งจะทำการแสดงข้อความพร้อมตัวเลขจำนวนนับ 

บรรทัด `vTaskDelay(1000/portTICK_RATE_MS);`  เป็นการหน่วงเวลาแบบ non blocking นั่นคือ task จะบอก OS ว่าต้องการหยุดพักเป็นเวลา `1000/portTICK_RATE_MS` ดังนั้นเมื่อทำคำสั่งนี้ OS จะคำนวนเวลาเป้าหมายที่จะปลุก task และเปลี่ยนสถานะของ Task เป็น suspend 



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


## >> ต่อไป >> 