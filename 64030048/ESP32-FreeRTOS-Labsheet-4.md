# การทดลอง ESP32 FreeRTOS 
##  ฟังก์ชัน vTaskDelete()

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


4. รันและบันทึกผลจากโปรแกรมข้างบน วิเคราะห์ผลที่ได้ว่าเป็นอย่างไร
![ภาพ](https://github.com/kammam19/ESP32-FreeRTOS-Intro/assets/112167732/0648c3cb-8661-4c9c-87a6-c61fd07a1865)
## รันโปรแกรม โปรแกรมทำงาน 5 ครั้ง และลบจะ MySecondTaskHandle

## [>> ต่อไป >>](./ESP32-FreeRTOS-Labsheet-5.md) 
