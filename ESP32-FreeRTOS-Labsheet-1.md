# การทดลอง ESP32 FreeRTOS 
## เรื่อง การสร้าง Task เบื้องต้น

## ลำดับการทดลอง

### 1. สร้าง project ใหม่ใน ESP-IDF เพื่อทำงานกับ FreeRTOS

1.1 ชื่อ  `FreeRTOS_Task_1`

1.2 ไม่ต้องเลือก Template (ใช้เทมเพลตพื้นฐาน)

1.3 เมื่อสร้างแล้ว ให้แก้ไข Code ดังนี้

1. เพิ่ม include files

2. ลบ code ใน app_main() แล้วเพิ่มcode ตามรูปต่อไปนี้

![Alt text](./Pictures/Labs/FreeRTOS-Lab-Picture-03.PNG)

3. รันโปรแกรมและอธิบายผลที่ได้
- ผลลัพธ์ที่ได้เมื่อทุก 1 วินาทีโปรแกรมจะบวกค่าตัวแปร i ทีละ 1 และจะแสดงข้อความ Hello My First Task ตามด้วยค่าของตัวแปร i

## [>> ต่อไป >>](./ESP32-FreeRTOS-Labsheet-2.md) 
