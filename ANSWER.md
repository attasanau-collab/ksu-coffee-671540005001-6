# ANSWER — นายอัธฑสัณ เอาะตะเมาะ 671540005001-6
 
- Platform: Vercel
- Production URL: https://ksu-coffee-671540005001-6.vercel.app
 
## คำตอบ
 
1. เลือกแพลตฟอร์ม Vercel ซึ่งรันโค้ดด้วยโมเดล Serverless Functions บน Production ไฟล์ `server.js` ไม่ได้ถูกใช้งานรัน Express Server โดยตรง เพราะ Vercel ตรวจจับไฟล์ในโฟลเดอร์ `/api` แล้วแปลงเป็น Serverless Endpoints แยกราย Route อัตโนมัติ และเสิร์ฟไฟล์ใน `/public` เป็น Static Files
2. ตั้งค่า `STUDENT_ID` ที่หน้า Settings > Environment Variables บน Dashboard ของ Vercel, ห้าม commit ไฟล์ `.env` เพราะเสี่ยงต่อการรั่วไหลของข้อมูลความลับสู่ Public Repo, ส่วนกรณีที่ `/api/info` แสดง NOT SET เกิดจากยังไม่ได้ตั้งค่า Key/Value บน Vercel หรือยังไม่ได้สั่ง Rebuild/Redeploy ใหม่หลังจากตั้งค่า
3. ครั้งแรก 5.36 วินาที / ครั้งที่สอง 0.75 วินาที — ครั้งแรกช้ากว่าอย่างเห็นได้ชัดเนื่องจากเกิดภาวะ Cold Start ซึ่งระบบต้อง Spin-up/Boot Serverless Instance ขึ้นใหม่ ส่วนครั้งที่สองและครั้งถัดมาเร็วกว่ามากเนื่องจากเป็นการเรียกใช้งานผ่าน Warm Instance ที่เปิดรันอยู่อย่างต่อเนื่อง
4. เมื่อสั่ง `git push` ทาง GitHub จะส่ง Webhook ไปแจ้ง Vercel เพื่อดึงโค้ดล่าสุดมา Build และ Deploy ให้อัตโนมัติ โดย Vercel จะดึง Git Commit Hash ล่าสุดบันทึกเป็น Metadata ระบบ ทำให้ค่า commit ใน `/api/info` ตรงกับผลลัพธ์จาก `git log -1 --format=%H` ชัดเจน
5. นำมา Deploy แบบตรง ๆ ไม่ได้ เนื่องจาก Vercel รองรับการรันแอปพลิเคชัน/Serverless Function แบบเดี่ยวเท่านั้น ไม่รองรับการอ่านและรัน Docker Engine รวมถึงไฟล์ `docker-compose.yml` ที่ต้องจัดการรันแบบ Multi-container พร้อมกัน