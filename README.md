# คณิตศาสตร์ ม.3 (เว็บประกอบการเรียน)

จัดทำโดย ครูคุ้ม อชิตพล บุณรัตน์ — เพื่อการศึกษา ใช้ร่วมกับหนังสือเรียน

## นำขึ้น GitHub Pages
1. repository ชื่อ `math-m3` (Public)
2. Add file → Upload files → ลาก index.html และโฟลเดอร์ worksheets (config.json อัปโหลดเฉพาะครั้งแรก) → Commit changes
3. Settings → Pages → Branch: main, /(root) → Save
4. เว็บอยู่ที่ https://ชื่อผู้ใช้.github.io/math-m3/

## เปิด/ปิดเฉลย (แก้ config.json แล้ว Commit)
เล่ม 1: "B1-1.2", "B1-1.3", "B1-1.4" (อสมการ) · "B1-2" (แยกตัวประกอบ) · "B1-3.1", "B1-3.2", "B1-3.3" (สมการกำลังสอง) · "B1-4.1", "B1-4.2", "B1-4.3" (ความคล้าย) · "B1-5.1", "B1-5.2", "B1-5.3" (ฟังก์ชันกำลังสอง) · "6.1", "6.2" (สถิติ)
เล่ม 2: "B2-1.1"–"B2-1.3" (ระบบสมการ) · "B2-2.1"–"B2-2.4" (วงกลม) · "B2-3.1"–"B2-3.3" (พื้นที่ผิวและปริมาตร) · "B2-4.1"–"B2-4.3" (ความน่าจะเป็น) · "B2-5.1", "B2-5.2" (ตรีโกณมิติ)
ตัวอย่าง: `"openAnswers": ["B1-1.2", "6.1"]`

## เปิดระบบล็อกอิน (ใช้ Firebase โปรเจกต์เดียวกับเว็บ ม.1)
1. **config.json ของ ม.3**: คัดลอกค่า `"firebase": {...}` จาก config.json ของเว็บ ม.1 มาวางแทน `null` แล้ว Commit
2. **Authorized domains**: ถ้าเว็บ ม.3 อยู่โดเมนเดียวกับ ม.1 (ชื่อผู้ใช้.github.io) ไม่ต้องทำอะไร
3. **Firestore → Rules**: วางส่วนนี้เพิ่ม *ก่อน* ปีกกาปิดสองตัวสุดท้าย แล้วกด Publish

```
    match /m3students/{uid} {
      allow read: if signedIn() && (request.auth.uid == uid || isTeacher());
      allow create, delete: if isTeacher();
      allow update: if isTeacher() || (signedIn() && request.auth.uid == uid
        && request.resource.data.diff(resource.data).affectedKeys().hasOnly(['topics','lastSeen','visits']));
    }
    match /m3tests/{id} {
      allow create: if signedIn() && request.resource.data.uid == request.auth.uid;
      allow read, delete: if isTeacher();
    }
```
4. เข้าเว็บ ม.3 → เข้าสู่ระบบด้วยอีเมลครู (บัญชีเดียวกับ ม.1) → แท็บ “สร้างบัญชี” วางรายชื่อ ม.3 → พิมพ์ใบแจ้งรหัส
5. นักเรียน ม.3 ล็อกอินด้วยเลขประจำตัว + รหัส (บัญชีแยกจาก ม.1)
6. ลืมรหัส: Authentication → Users ค้นหา `เลขประจำตัว@students.m3math.app` ลบ แล้วลบเอกสารใน m3students แล้วสร้างใหม่

## หน้า O-NET (#onet)
- แนวข้อสอบเขียนใหม่ 93 ข้อ อิงข้อสอบจริงปีการศึกษา 2561–2568 (เปลี่ยนตัวเลข/สถานการณ์ ไม่คัดลอกฉบับจริง) แต่ละหัวข้อมีส่วน “05 แนวข้อสอบ O-NET” ท้ายหน้า
- ข้อสอบเสมือนจริง 20 ข้อ 100 คะแนน ถ้านักเรียนล็อกอินอยู่ คะแนนบันทึกใน m3tests (type = "onet") ดูได้ในแดชบอร์ดครู แท็บคะแนนทดสอบ

## เปิด/ปิดเฉลย O-NET
ใน config.json แก้ `"onetSolutions": false` (ปิด: นักเรียนเห็นแค่ว่าถูกหรือผิด และคะแนน) เป็น `true` (เปิด: เห็นคำตอบที่ถูกและวิธีคิด) แล้ว Commit
ถ้า config.json บน GitHub ยังไม่มีบรรทัดนี้ ให้เพิ่มเอง ระวังจุลภาคคั่นระหว่างบรรทัด
