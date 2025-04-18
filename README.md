# pukpostit
# PUKUMPEE Time Capsule
# PUKUMPEE Time Capsule

แอปพลิเคชัน "PUKUMPEE Time Capsule" เป็นระบบบันทึกข้อความที่มีฟีเจอร์ครบครัน คล้ายโซเชียลมีเดียหรือฟอรัม รองรับการโพสต์ข้อความ รูปภาพ และการดูข้อมูลคริปโตเรียลไทม์

---

## 1. CSS (styles.css)

โค้ด CSS นี้กำหนดสไตล์สำหรับหน้าเว็บของแอปพลิเคชัน:

### พื้นฐาน (body)
- ใช้ฟอนต์ Arial
- พื้นหลังสีเทาอ่อน (#f0f2f5) พร้อมรูปภาพพื้นหลัง (bg.png) ที่ยืดเต็มหน้าจอ
- การจัดระยะห่าง (padding) 20px และปรับเป็น 10px บนมือถือ
- ความสูงขั้นต่ำ 100vh และเลื่อนได้อัตโนมัติ

### การตอบสนอง (Responsive)
- ปรับแต่งสำหรับหน้าจอขนาดเล็ก (≤ 768px) เช่น ลด padding และเปลี่ยนตำแหน่งพื้นหลัง

### ส่วนประกอบหลัก
- `.overlay`: ชั้นมืดโปร่งแสงสำหรับพื้นหลังเมื่อมีป๊อปอัพ
- `.container`: กล่องเนื้อหาหลัก ขนาดสูงสุด 800px มีเงาและขอบโค้ง
- `.post`: กล่องโพสต์ขนาด 300px ลากได้ มีเอฟเฟกต์ hover

### ปุ่มและฟอร์ม
- ปุ่มมีสีน้ำเงิน (#007bff) เปลี่ยนสีเมื่อ hover
- ฟอร์ม (input, textarea) มีขอบและเงาเบา ๆ ขนาดฟอนต์ 16px

### ตารางและหน้าเพจ
- ตารางโพสต์ (.post-table) มีการแบ่งหน้า (pagination) และสีสลับแถว
- รองรับการแสดงผลทั้งโพสต์ส่วนตัวและสาธารณะ

---

## 2. JavaScript (scripts.js)

โค้ด JavaScript นี้จัดการ逻辑ของแอปพลิเคชันทั้งฝั่ง客户端 (client-side):

### การตั้งค่าเริ่มต้น
- ใช้ IndexedDB ชื่อ "PostDB" เก็บโพสต์ส่วนตัว
- เก็บข้อมูลผู้ใช้ใน localStorage และจัดการ JWT token สำหรับเซิร์ฟเวอร์

### การจัดการผู้ใช้
- **สมัครสมาชิก/ล็อกอินในเครื่อง**: ใช้ localStorage เก็บ username/password
- **สมัครสมาชิก/ล็อกอินเซิร์ฟเวอร์**: ส่งคำขอไปยัง API พร้อม CSRF token

### การสร้างโพสต์
- รองรับข้อความ รูปภาพ และตัวเลือกโพสต์สาธารณะ
- มีคำสั่งพิเศษ เช่น `/help` แสดงคู่มือ และ `/[crypto]` ดึงข้อมูลคริปโต (เช่น BTC, ETH) จาก Binance
- โพสต์ส่วนตัวเก็บใน IndexedDB, โพสต์สาธารณะส่งไปเซิร์ฟเวอร์

### การแสดงผล
- โพสต์แสดงในหน้าต่างที่ลากได้ มีปุ่มควบคุม (ปิด, ย่อ, ขยาย)
- ตารางโพสต์ส่วนตัวและสาธารณะ มีการแบ่งหน้า (5 โพสต์ต่อหน้า)

### ฟังก์ชันคริปโต
- ดึงราคาเรียลไทม์ผ่าน WebSocket และข้อมูล Kline จาก Binance
- คำนวณ MACD และ Fibonacci Levels

---

## 3. HTML (index.html)

ไฟล์ HTML นี้เป็นโครงสร้างหน้าเว็บ:

### หน้าแรก
- มีส่วนล็อกอินในเครื่องและเซิร์ฟเวอร์ (ป๊อปอัพ)
- หน้าหลัก (#mainPage) มีแท็บ 3 อัน: สร้างโพสต์, โพสต์ส่วนตัว, โพสต์สาธารณะ

### ส่วนประกอบ
- กล่องข้อความและอัปโหลดรูปภาพสำหรับสร้างโพสต์
- ตารางแสดงโพสต์พร้อมปุ่มจัดการ (ลบ, ดู)
- พื้นที่สำหรับแสดงโพสต์แบบหน้าต่าง (#posts)

### ภาษา
- ใช้ภาษาไทยทั้งหมด เช่น "ล็อกอิน", "สมัครสมาชิก", "โพสต์"

---

## 4. Node.js Server (server.js)

โค้ดฝั่งเซิร์ฟเวอร์ใช้ Express จัดการ API:

### การตั้งค่า
- ใช้ SQLite เก็บข้อมูลผู้ใช้, โพสต์, และคอมเมนต์
- รองรับการอัปโหลดไฟล์ด้วย multer (เก็บในโฟลเดอร์ uploads/)

### การป้องกัน
- JWT สำหรับการยืนยันตัวตน
- CSRF Protection ป้องกันการโจมตี
- CORS จำกัดการเข้าถึงจาก localhost:8080 เท่านั้น

### API Endpoints
- **ผู้ใช้**:
  - `/api/register` (POST)
  - `/api/login` (POST)
  - `/api/logout` (POST)
- **โพสต์**:
  - สร้าง: `/api/posts` (POST)
  - ดึงโพสต์ส่วนตัว: `/api/posts/private` (GET)
  - ดึงโพสต์สาธารณะ: `/api/posts/public` (GET)
  - ลบ: `/api/posts/:id` (DELETE)
  - อัปเดตวิว: `/api/posts/:id/views` (PATCH)
  - แก้ไข: `/api/posts/:id` (PUT)
- **คอมเมนต์**:
  - สร้าง: `/api/comments` (POST)
  - ดึง: `/api/comments?postId=...` (GET)

### การจัดการ
- เฉพาะเจ้าของโพสต์ที่แก้ไขหรือลบโพสต์ได้
- คอมเมนต์ใช้ได้เฉพาะโพสต์สาธารณะ

---

## สรุป

แอปพลิเคชัน "PUKUMPEE Time Capsule" นี้เป็นระบบบันทึกข้อความที่มีฟีเจอร์ครบครัน:

- **ฝั่งผู้ใช้**: สามารถสมัคร/ล็อกอิน, โพสต์ข้อความ/รูปภาพ, ดูโพสต์ส่วนตัวและสาธารณะ, คอมเมนต์, และดูข้อมูลคริปโต
- **ฝั่งเซิร์ฟเวอร์**: จัดการข้อมูลอย่างปลอดภัยด้วย SQLite, JWT, และ CSRF
- **ดีไซน์**: อินเทอร์เฟซสวยงาม ลากวางได้ รองรับทั้งเดสก์ท็อปและมือถือ

---


