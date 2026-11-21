# 🎓 BidZone - ระบบประมูลสินค้าออนไลน์ (Online Real-Time Auction System)

> 🌐 **Live Demo Website:** [https://bidzone-demo-folk.lucidolive.chatgpt.site/](https://bidzone-demo-folk.lucidolive.chatgpt.site/) <br>
> **Term Project:** รายวิชา 305311 ระบบฐานข้อมูล (Database Systems)

---

## 📌 เกี่ยวกับโปรเจกต์ (Project Overview)

**BidZone (ระบบประมูลสินค้าออนไลน์)** เป็นเว็บแอปพลิเคชันแบบ Full-Stack ที่พัฒนาขึ้นเพื่อจำลองระบบประมูลสินค้าแบบเรียลไทม์ตามสถาปัตยกรรม 3-Tier Architecture ครอบคลุมตั้งแต่การออกแบบฐานข้อมูลเชิงสัมพันธ์ (Relational Database) ระดับ 3NF/BCNF จำนวน 9–10 ตาราง[cite: 20, 23], การพัฒนา Business Logic ขั้นสูงในระดับฐานข้อมูล (Stored Procedure, Trigger, Reporting View, Complex Analytical Queries), การเชื่อมต่อ RESTful API ด้วย Node.js/Express ไปจนถึงการแสดงผลหน้าเว็บแบบ Real-time State Synchronization

---

## 👨‍💻 บทบาทและความรับผิดชอบ (My Role: Backend & DB Logic Engineer)

โปรเจกต์นี้พัฒนาในรูปแบบทีม 3 คน (Data Architect & DBA, Backend & DB Logic Engineer, Full-stack & Frontend Developer) โดยในส่วนของ **Backend & DB Logic Engineer** รับผิดชอบการออกแบบและพัฒนาตรรกะทางธุรกิจในระดับฐานข้อมูลและระบบหลังบ้านทั้งหมด:

* **ACID Transaction Stored Procedure (`sp_PlaceBid`):** พัฒนากระบวนการตรวจสอบเงื่อนไขราคาเสนอใหม่ (ต้องสูงกว่าราคาปัจจุบัน + Bid Increment) บันทึกประวัติการประมูล และอัปเดตราคาสูงสุดของสินค้าแบบ Atomic Transaction (`START TRANSACTION`, `COMMIT`, `ROLLBACK`) เพื่อป้องกันปัญหา Race Condition
* **Automated Soft-Close Trigger (`trg_ExtendAuctionOnLateBid`):** พัฒนา Trigger ทำงานอัตโนมัติเมื่อมีการ `INSERT` ข้อมูลการประมูลใหม่ เพื่อตรวจสอบและขยายเวลาปิดประมูลออกไปอีก 5 นาที (Soft Close) หากมีการเคาะราคาในช่วง 3 นาทีสุดท้ายก่อนหมดเวลา
* **Executive Reporting View (`vw_AuctionPerformanceMetrics`):** ออกแบบ View สำหรับสรุปภาพรวมทางธุรกิจ ทั้งอัตราการปิดประมูลสำเร็จ (Win Rate) ส่วนต่างราคาเปิดเทียบราคาปิด และมูลค่าการประมูลรวมแยกตามหมวดหมู่สินค้า
* **10 Complex Analytical Queries & Mock Data (`queries.sql`, `data.sql`):** ออกแบบและทดสอบชุดคำสั่งคิวรีเชิงวิเคราะห์ 10 ข้อ ครอบคลุม Multi-table JOIN (3 ตาราง), `GROUP BY + HAVING`, Subqueries/Correlated Subqueries และ Window Functions (`RANK`, `ROW_NUMBER`, `OVER`) พร้อมจัดเตรียมชุดข้อมูลจำลอง (Mock Data) กว่า 500+ Records
* **RESTful API Integration (`server.js`):** พัฒนา API Endpoints ด้วย Node.js/Express เชื่อมต่อระหว่างหน้าเว็บกับฐานข้อมูล

---

## 🚀 คุณสมบัติเด่นของระบบ (Key Features)

- **Real-time Live Bidding (ACID Compliant):** เคาะราคาประมูลพร้อมตรวจสอบความถูกต้องและอัปเดตสถานะหลายตารางพร้อมกันผ่าน `sp_PlaceBid`
- **Anti-Sniping Soft Close System:** ระบบต่อเวลาประมูลอัตโนมัติ 5 นาทีเมื่อมีการเคาะราคาช่วงท้ายผ่าน Database Trigger `trg_ExtendAuctionOnLateBid`
- **Real-time Countdown Timers:** ระบบนับเวลาถอยหลังรายชิ้นแบบเรียลไทม์ (ชั่วโมง : นาที : วินาที) คำนวณจาก `end_time` บนฐานข้อมูลโดยตรง
- **Personal Bidding Dashboard (`bids.html`):** หน้าแสดงสถิติการเสนอราคาของผู้ใช้ แยกสถานะ *"คุณเป็นผู้นำ"* (สีเขียว) และ *"ถูกเสนอแซง"* (สีแดง) พร้อมระบบแจ้งเตือน Warning Banner แบบไดนามิก
- **Executive Analytics Dashboard:** แสดงข้อมูลสถิติยอดขายรวมและประสิทธิภาพการประมูลรายหมวดหมู่ผ่าน `vw_AuctionPerformanceMetrics`

---

## 🛠️ เทคโนโลยีที่ใช้ (Tech Stack)

### **Database & Advanced SQL**
- **Relational Database (3NF/BCNF):** 9–10 ตารางหลัก (`Auction_Items`, `Sellers`, `Bidders`, `Bids`, `Winning_Bids`, `Payments`, `Item_Categories`, `Shipping_Details`, `Watchlists`)
- **Database Objects:** Stored Procedure (`sp_PlaceBid`), Trigger (`trg_ExtendAuctionOnLateBid`), View (`vw_AuctionPerformanceMetrics`)
- **Analytical SQL:** Multi-table JOINs, Aggregations (`GROUP BY`/`HAVING`), Subqueries, Window Functions

### **Backend & Frontend**
- **Backend:** Node.js, Express.js, Database Driver (`mssql` / SQL Client), `dotenv`, `cors`
- **Frontend:** HTML5, CSS3 (Responsive Flexbox & Grid), JavaScript (ES6+ Native `fetch()` API & Real-time State Sync)

---

## 📁 โครงสร้างโปรเจกต์ (Project Structure)

```text
bidzone-auction-system/
├── database/
│   ├── schema.sql           # DDL สร้างตาราง, Constraints, Foreign Keys และ Indexes
│   ├── data.sql             # DML ข้อมูลจำลอง Mock Data (>= 500 records ในตารางหลัก)
│   ├── logic.sql            # Stored Procedure (sp_PlaceBid), Trigger และ Reporting View
│   └── queries.sql          # ชุดคำสั่ง Complex Analytical Queries 10 ข้อ
├── assets/                  # รูปภาพสินค้าและสื่อประกอบ
├── index.html               # หน้าหลักแสดงรายการสินค้าประมูลทั้งหมด
├── bids.html                # หน้า Dashboard สรุปประวัติการประมูลส่วนบุคคล
├── style.css                # ไฟล์ Styling หลักของแอปพลิเคชัน
├── script.js                # Logic ฝั่ง Client (Fetch API, Countdown, UI Updates)
├── server.js                # Express REST API Server
├── .env.example             # ไฟล์ตัวอย่าง Configuration สำหรับเชื่อมต่อ Database
└── package.json             # ไฟล์จัดการ Node.js Dependencies
