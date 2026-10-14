# ANSWER — นาย รัฐภูมิ ผุดผ่อง 671540005028-9

- Platform: Vercel
- Production URL: https://ksu-coffee-671540005028-9.vercel.app

## คำตอบ

1. เลือก Vercel ซึ่งรันโค้ดแบบ Serverless คือสร้างฟังก์ชันขึ้นมาตอบเมื่อมี request เข้ามา ไม่ได้มี process รันค้างตลอด ไฟล์ public/index.html ถูกเสิร์ฟเป็นไฟล์ static ส่วน /api/info ทำงานผ่านฟังก์ชันของ Vercel ดังนั้น server.js ไม่ได้ถูกรันบน production (server.js ใช้เฉพาะทดสอบในเครื่อง) <ตรวจกับโครงสร้าง repo จริงแล้วปรับถ้าต่าง>
   [Render: เป็น Container/Web Service รัน `node server.js` จริงตลอดเวลา จึงใช้ server.js บน production]

2. ตั้ง STUDENT_ID ในหน้า Settings → Environment Variables ของโปรเจกต์บน Vercel ไม่ commit .env เพราะเป็นไฟล์ค่าตั้งค่าเฉพาะเครื่อง/ความลับ ถ้าขึ้น repo สาธารณะทุกคนเห็นและอยู่ใน git history ถาวร NOT SET เกิดได้จาก: ยังไม่ได้ตั้งตัวแปร, ชื่อตัวแปรสะกดผิด, เลือก environment ไม่ครอบคลุม Production, หรือตั้งหลัง deploy แล้วยังไม่ได้ deploy ใหม่ (ตัวแปรมีผลเฉพาะ deployment ที่สร้างหลังตั้งค่า)

3. ครั้งแรก 2.062 วินาที / ครั้งที่สอง 0.757 วินาที — ต่างกัน ครั้งแรกช้ากว่า เพราะฟังก์ชัน Serverless ไม่ได้รันค้างไว้ ครั้งแรกต้องเริ่มฟังก์ชันใหม่ (cold start) และสร้างการเชื่อมต่อ TLS ส่วนครั้งที่สอง instance ยังอุ่นอยู่ จึงตอบเร็วกว่า

4. เมื่อ git push GitHub แจ้งแพลตฟอร์มผ่านการเชื่อมต่อ repo (webhook) แพลตฟอร์มดึงโค้ด commit ล่าสุดมา build และ deploy เป็นเวอร์ชันใหม่ แล้วสลับให้ production ชี้เวอร์ชันใหม่ จึงเห็น Menu v2 ค่า commit ใน /api/info มาจาก environment ของแพลตฟอร์มซึ่งบันทึก SHA ของ commit ที่ deploy จึงตรงกับ git log -1 --format=%H <ยืนยันหลังเทียบจริง>

5. deploy ตรง ๆ ไม่ได้ เพราะ Vercel ไม่รันคอนเทนเนอร์ และไม่อ่าน docker-compose.yml ส่วนระบบข้อ 1 มี 2 service (build เอง + image จาก Docker Hub) ต้องแยก deploy ทีละ service หรือใช้บริการที่รัน container ได้ (เช่น Render แยกเป็น Web Service ต่อ service) และ port mapping แบบ 8081:3000 ก็ใช้ไม่ได้บนแพลตฟอร์ม
   [Render: Web Service รับ Dockerfile ได้ทีละ service ไม่อ่าน compose โดยตรง ต้องแยกเป็นหลาย service]