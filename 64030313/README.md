# 64030313 นางสาวสุดารัตน์ ฆ้องอินตะ

# Lab1
![image](https://github.com/NamaoySudarat/ESP32-FreeRTOS-Intro/assets/115037574/ba1aab28-af7d-4252-b09d-4f4826935b33)
## รันโปรแกรมและอธิบายผลที่ได้
```
โปรแกรมจะแสดงข้อความ Hello My First Task 1, 2, 3, ..., n หน่วงเวลา 1 วินาที
```

# Lab2
![image](https://github.com/NamaoySudarat/ESP32-FreeRTOS-Intro/assets/115037574/50f54365-30b9-442f-9139-7f87a0aabf1b)

## รันโปรแกรมและอธิบายผลที่ได้
```
โปรแกรมจะแสดงข้อความครั้งละ 5 บรรทัด และเพิ่มตัวเลขเพิ่มขึ้นเรื่อยๆ หน่วงเวลา 1 วินาที
Hello My First Task 0
Hello My First Task 0
Hello My First Task 0
Hello My First Task 0
Hello My First Task 0
```

# Lab3

## รันและบันทึกผลจากโปรแกรม
![image](https://github.com/NamaoySudarat/ESP32-FreeRTOS-Intro/assets/115037574/8c36016c-a057-4074-8b8a-bcaf6c4ec969)

## รันและบันทึกผลจากโปรแกรม
![image](https://github.com/NamaoySudarat/ESP32-FreeRTOS-Intro/assets/115037574/b146f2d5-f4ca-4dd5-b230-33b754875ef7)

## ได้ผลเหมือนหรือต่างกันอย่างไร
- xTaskCreate สร้างงานและรันบนคอร์เริ่มต้น (คอร์ 0) ของ FreeRTOS โดยปกติ
- xTaskCreatePinnedToCore ช่วยให้คุณระบุคอร์ที่งานจะรันอยู่ และงานจะรันบนคอร์ที่ระบุ

# Lab4

## รันและบันทึกผลจากโปรแกรม
![image](https://github.com/NamaoySudarat/ESP32-FreeRTOS-Intro/assets/115037574/3b5b39ed-3866-4838-ad4f-60e164c73ce3)

## ได้ผลเหมือนหรือต่างกันอย่างไร
```
My_Second_Task จะหยุดทำงานและถูกลบจากระบบ FreeRTOS เมื่อ i ครบ 5 รอบ และข้อความ Second Task deleted จะแสดงใน Terminal
```

# Lab5

## รันและบันทึกผลจากโปรแกรม
![image](https://github.com/NamaoySudarat/ESP32-FreeRTOS-Intro/assets/115037574/b1205646-0095-4190-bc5b-46497982b90f)

## วิเคราะห์ผลที่ได้ว่าเป็นอย่างไร
```
เมื่อ i เป็น 5 รอบ My_Second_Task จะถูก suspended
เมื่อ i เป็น 10 รอบ จะทำงานต่อ
เมื่อ i เป็น 15 รอบ My_Second_Task จะถูก deleted
เมื่อ i เป็น 20 รอบ My_First_Task จะถูก suspended
```

# Lab6 

## รันและบันทึกผลจากโปรแกรมข้างบน
![image](https://github.com/NamaoySudarat/ESP32-FreeRTOS-Intro/assets/115037574/6b84ce1b-9d1b-4cc4-a58c-bf288a4d5483)
![image](https://github.com/NamaoySudarat/ESP32-FreeRTOS-Intro/assets/115037574/af98ae22-41ea-4975-a099-70f19e3aed65)
![image](https://github.com/NamaoySudarat/ESP32-FreeRTOS-Intro/assets/115037574/6b5d670c-8b6d-458d-b3d5-6255eedf0db7)

## วิเคราะห์ผลที่ได้ว่าเป็นอย่างไร
```
ตรวจจับการกดปุ่ม และสลับสถานะ LED ทุกครั้งที่ปุ่มถูกกด และแสดงข้อความ Button pressed! ใน Terminal
```

# Lab7

## รันและบันทึกผลจากโปรแกรมข้างบน
![image](https://github.com/NamaoySudarat/ESP32-FreeRTOS-Intro/assets/115037574/950c6e93-085e-47d6-bbfc-2e1452f27c7e)

## วิเคราะห์ผลที่ได้ว่าเป็นอย่างไร
```
Task1 สร้าง Queue และส่งข้อมูล Hello from Demo_Task X ผ่าน Queue ไปยัง Task2
Task2 รอรับข้อมูลจาก Queue และแสดงข้อความใน Terminal
```

# Lab 8

## รันและบันทึกผลจากโปรแกรมข้างบน
![image](https://github.com/NamaoySudarat/ESP32-FreeRTOS-Intro/assets/115037574/b33bf099-0127-453c-8f8c-363b817b137b)

## วิเคราะห์ผลที่ได้ว่าเป็นอย่างไร
```
เมื่อคุณกดปุ่ม โปรแกรมจะตรวจจับการกดปุ่มและแสดงข้อความ Button pressed! ใน Ternimal
ควบคุม LED ได้โดยเปลี่ยนสถานะของ LED ผ่านคำสั่ง GPIO
```
