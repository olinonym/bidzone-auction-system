# 🎓 BidZone - Real-Time Full-Stack Auction Platform
> **Term Project** สำหรับรายวิชา 305311 Database  

---

## 📌 เกี่ยวกับโปรเจกต์ (Project Overview)

**BidZone** เป็นโปรเจกต์ประยุกต์ความรู้แบบ Full-Stack Web Application ที่พัฒนาขึ้นเพื่อจำลองระบบประมูลสินค้าออนไลน์แบบเรียลไทม์ ออกแบบตามสถาปัตยกรรม 3-Tier Architecture เชื่อมโยงตั้งแต่การออกแบบฐานข้อมูลเชิงสัมพันธ์ตามหลัก 3NF บน MS SQL Server, การพัฒนาระบบหลังบ้าน (Backend API & Business Logic) ด้วย Node.js/Express ไปจนถึงการแสดงผลหน้าหน้าเว็บ (Frontend Dashboard) ด้วย JavaScript Vanilla ที่มีระบบ Real-time State Synchronization

---

## 🚀 คุณสมบัติเด่น (Key Features)

* **Real-time Live Bidding**: เคาะราคาประมูลพร้อมตรวจสอบเงื่อนไขความถูกต้องแบบ Atomic Transaction ผ่าน Stored Procedure (`sp_PlaceBid`)
* **Real-time Countdown Timers**: ระบบนับเวลาถอยหลังรายชิ้นแบบเรียลไทม์ (ชั่วโมง : นาที : วินาที) คำนวณจาก `end_time` บนฐานข้อมูลโดยตรง
* **Personal Bidding Dashboard (`bids.html`)**: หน้าแสดงสถิติการเสนอราคาของผู้ใช้ แยกสถานะ *"คุณเป็นผู้นำ"* (สีเขียว) และ *"ถูกเสนอแซง"* (สีแดง) พร้อมระบบคำนวณยอดเงินรวม
* **Quick Bidding System**: ปุ่มเคาะราคาด่วน (+฿50) คำนวณราคาขั้นต่ำถัดไปให้อัตโนมัติในตารางประวัติ พร้อมอัปเดตสถานะการเป็นผู้นำทันที
* **Global State Synchronization**: อัปเดตตัวเลขการแจ้งเตือนบน Sidebar และแดชบอร์ดข้ามหน้าเว็บ (`index.html` และ `bids.html`) โดยไม่ต้อง Refresh หน้าจอ
* **Alert Warning Banner**: กล่องแจ้งเตือนสีเหลืองแบบไดนามิก แจ้งรายชื่อสินค้าที่ผู้ใช้ถูกเสนอแซงทันที
* **Database Analytics & Metrics**: คำนวณยอดขายรวม อัตราความสำเร็จ และสถิติต่างๆ ผ่าน Database View (`vw_AuctionPerformanceMetrics`)

---

## 🛠️ เทคโนโลยีที่ใช้ (Tech Stack)

### **Frontend**
* **HTML5 / CSS3**: โครงสร้าง UI แบบ Responsive Layout (Flexbox & Grid) พร้อม Custom Utility Classes
* **JavaScript (ES6+)**: Native `fetch()` API สำหรับการสื่อสารแบบ Asynchronous, Dynamic DOM Rendering และ Event Handling

### **Backend**
* **Node.js**: Runtime Environment สำหรับฝั่ง Server
* **Express.js**: Web Framework จัดการ RESTful API Routing
* **`mssql` Driver**: เชื่อมต่อและส่งคำสั่งจัดการ MS SQL Server
* **`dotenv` & `cors`**: จัดการ Environment Variables และ Cross-Origin Resource Sharing

### **Database**
* **MS SQL Server**: ระบบจัดการฐานข้อมูลความสัมพันธ์ (RDBMS) ออกแบบตามหลัก Normalization (3NF) รวม 10 ตาราง
* **T-SQL Business Logic**: 
  * Stored Procedure: `sp_PlaceBid` (Transaction Management)
  * View: `vw_AuctionPerformanceMetrics` (Complex Aggregation Query)

---

## 📁 โครงสร้างโปรเจกต์ (Project Structure)

```text
bidzone/
├── assets/                  # รูปภาพสินค้าและสื่อต่างๆ
├── index.html               # หน้าหลักแสดงรายการสินค้าประมูลทั้งหมด
├── bids.html                # หน้า Dashboard สรุปประวัติการประมูลส่วนบุคคล
├── style.css                # ไฟล์ Styling หลักของแอปพลิเคชัน
├── script.js                # Logic ฝั่ง Client (Fetch API, Countdown, UI Updates)
├── server.js                # Express REST API Server
├── logic.sql                # Database Schema, Stored Procedures, Views & Mock Data
├── .env.example             # ไฟล์ตัวอย่าง Configuration สำหรับเชื่อมต่อ Database
└── package.json             # ไฟล์จัดการ Node.js Dependencies
