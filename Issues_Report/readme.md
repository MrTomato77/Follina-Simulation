## 06/07/2025 - 04/08/2025
- เลือกหัวข้อ (CVE-2022-30190)
- เขียน Storyboard
- ใช้ VirtualBox (7.1.12) ในการ Demonstration
- เลือก Version ของ OS ที่จะจำลอง (Windows, Kali Linux)

## 05/08/2025 - 08/08/2025
- ปัญหา `MSDT Protocol` ไม่ถูกเรียกใช้โดย Microsoft Word (Fixed)
    - ทดลองแล้วว่าเป็นปัญหาที่ Version ของ `Microsoft Office` ที่ต้องโหลดด้วย `ODT` เท่านั้น
    - ทดลองบน Windows เวอร์ชั่นต่างๆ เช่น `Windows Server 2019`, `Windows 10 19H1`, `Windows 11 24H2`
    - ผลลัพธ์คือเมื่อใช้ `Microsoft Office` เวอร์ชั่น 2019 ที่โหลดด้วย `ODT` และทดลองใน Windows ทั้งสามจากข้างต้น พบว่า `Microsoft Word` มีการเรียกใช้ `MSDT Protocol` ที่เรียกหา `Index.html` ที่ `IP Address` ของ Kali Linux สำเร็จแล้ว

- ปัญหา `Error code 501, message Unsupported method ('OPTIONS')` คำสั่งที่ส่งผ่าน `MSDT Protocol` ยังไม่สามารถทำงานได้
    - คาดว่าต้องหา office version ที่เหมาะสมกับการจำลองให้เจอก่อน

- พบว่า `Microsoft 2016 Professional Retail x64 (64-bit)` สามารถใช้งานได้และเหมาะสมต่อการจำลองสถานการณ์

## 09/08/2025 - 26/08/2025
- ปัญหา `Reverse Shell` ของ `JohnHammond` สามารถสร้าง `session` ได้เพียงครั้งเดียว หลังจากถูกปิด `Troubleshooter`
    - `MSDT Protocol` ใช้งานได้ตามปกติ มีการเรียก `Internet Explorer` ออกมาเหมือนทุกครั้ง
    - `Troubleshooter` ไม่ถูกเรียกใช้งานในครั้งที่ 2 ทำให้การ `Reverse Shell` ไม่สำเร็จ

- แก้ไขโดยการใช้ `reverse tcp payload` ในการรักษา `session` หลังจาก `Troubleshooter` ถูกปิด
    - สร้าง `reverse tcp payload` ขึ้นมาเองด้วย `msfvenom`
    - ใช้ `set AutoRunScript getsystem` ใน Listener เพื่อขอใช้สิทธิสูงสุดในการเข้าถึง Victim
