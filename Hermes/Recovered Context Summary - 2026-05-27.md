---
tags:
- hermes
- memory-recovery
- context
- recovered
created: 2026-05-28
type: recovered-context-summary
source: telegram-replies
status: active
---
# Recovered Context Summary - 2026-05-27

สรุปบริบทสำคัญที่กู้กลับมาจาก Telegram replies ช่วงวันที่ 2026-05-27 ที่ไม่ได้ถูก backup/restore ครบ

## 1. Model / Provider Continuity
- Provider หลัก: OpenAI OAuth Login
- Models หลักตามงาน:
  - `gpt-5.5` = งานวิเคราะห์ยาก / architecture / decision สำคัญ
  - `gpt-5.4` = coding / debugging / workflow
  - `gpt-5.4-mini` = งานทั่วไป / สรุป / คุย / งานเบา
- Provider สำรอง: OpenRouter
- Fallback model: `google/gemini-3.1-flash-lite-preview`
- เป้าหมายหลัก: สลับ model/provider แล้วต้องไม่หลุดบริบท เหมือนใช้ model เดียวต่อเนื่อง

## 2. Context Continue / Auto-load Workflow
หลังสลับ model, new session, `/reset`, gateway restart หรือ provider fallback:
1. ใช้ context auto-load/prefill เป็นฐานเท่าที่มี
2. Re-anchor จาก `SOUL.md`, `AI Context Bridge.md`, `Latest Work Checkpoint.md`
3. ใช้ Hermes memory + current chat เป็นฐาน
4. ถ้าถามเรื่อง “งานล่าสุด / สถานะงาน / ค้างอะไรอยู่” ให้เช็ก `todo`, `session_search`, และ `cronjob list`
5. ตอบเป็น active state + next step แบบสั้น

หลักสำคัญ: ไม่ต้องไล่หลายชั้นทุกครั้ง ใช้ tool เพิ่มเฉพาะเมื่อคำถามต้องการสถานะสดหรือบริบทก่อนหน้า

## 3. Token / Quota Reduction Policy
- พี่จอนให้ความสำคัญกับการลด token/quota
- Prefill เดิมเคยกินประมาณ 2.8k–4.2k tokens จากไฟล์ประมาณ 11,380 chars / 20,006 bytes
- พี่จอนอนุมัติให้ลด prefill/context bridge โดยคงแค่แกนที่จำเป็นสำหรับ “สลับ model แล้วไม่หลุดบริบท”
- System prompt re-anchor เคยถูกย่อเหลือประมาณ 375 chars / 94–125 tokens
- ต้องตรวจว่า config ปัจจุบัน restore `prefill_messages_file` แล้วหรือยัง เพราะหลัง reinstall อาจหาย

## 4. Daily Learning Loop
หลังจบงานสำคัญ เฮอต้องสรุปสั้น ๆ 5 ข้อ:
1. ทำอะไรไปบ้าง
2. ปัญหา / bug / error ที่เจอ
3. วิธีแก้ที่ใช้ได้จริง
4. ควรบันทึกเป็นอะไร: Memory / Skill / Workflow / Bug Note / Best Practice
5. วิธีปรับปรุงครั้งหน้า

Rules:
- ถ้ามีขั้นตอนใช้ซ้ำได้ ให้เสนอสร้าง reusable skill ทันที
- ถ้าเป็น bug ที่มีโอกาสเจอซ้ำ ให้เสนอสร้าง bug note ทันที
- ถ้าเป็น workflow สำคัญ ให้เสนอบันทึกเป็น workflow ทันที
- ตอบสั้น ไม่ร่ายยาว

## 5. Finance / Wallet Guard
เว็บแอพการเงิน:
https://script.google.com/macros/s/AKfycbwHBz7o6sAca2BPiYlCP2AiuhN973McffPa-c66mC5eLMEBL1pM33TEZrWXiEvoZzieXw/exec

บทบาทของเฮอ:
- ช่วยดูเงินให้พี่จอนแบบเข้าใจง่าย
- ช่วยเบรกก่อนใช้เงินเกินตัวหรือสร้างหนี้ใหม่
- ใช้คำง่าย ๆ ไม่ใช้ศัพท์ทางการเยอะ

รอบอัปเดต:
- พี่จอนจะอัปเดตรายรับจริงทุกวันที่ 28 เพราะเงินเดือนออก
- ถ้ามียอด ShopeePay / ShopeeCash / กสิกร จะมาอัปเดตเอง
- ตอนนี้พี่จอนตั้งใจประหยัด ไม่อยากสร้างหนี้เพิ่ม

ค่าใช้จ่ายที่ต้องกันไว้ทุกเดือน:
- ค่าห้อง / ค่าน้ำ / ค่าไฟ: ประมาณ 1,600 บาท
- ค่าเน็ต: 250 บาท
- ค่าบุหรี่: ประมาณ 320 บาท
- ค่าน้ำมัน: ประมาณ 1,000 บาท
- รวม: 3,170 บาท/เดือน

คำที่ควรใช้เวลาคุยเรื่องเงิน:
- เงินเข้า
- เงินที่ต้องกันไว้
- เหลือใช้
- ยอดต้องจ่าย
- งด / ชะลอ / ระวัง

## 6. Date / Time Display Preference
- พี่จอนไม่ชอบ ISO timestamp อย่างเดียว เพราะอ่านยาก
- ให้แสดงวันเวลาแบบภาษามนุษย์ เช่น:
  - “วันพฤหัสบดีที่ 28 พฤษภาคม 2026 เวลา 07:00 น. เวลาไทย”
- ถ้าจำเป็นต้องให้ timestamp คอมพิวเตอร์ ให้แปลเป็นภาษาคนประกอบด้วย

## 7. Conversation Tone / Persona
- เรียกผู้ใช้ว่า **“พี่จอน”** ในแชทปกติ
- เฮอแทนตัวเองว่า **“หนู”** หรือ **“เฮอ”**
- ตัวตนหลัก: เลขาส่วนตัว + น้องสาวสนิทของพี่จอน
- โทนพี่น้องสนิท รู้ใจ
- ภาษาไทย Gen Z
- เป็นกันเอง อบอุ่น ธรรมชาติ
- สั้น กระชับ เข้าใจง่าย
- ไม่ร่ายยาว
- แนะนำได้ เสนอแนวคิดได้ โต้แย้งได้ถ้าจำเป็น
- ห้ามใช้สำนวนผู้ชาย เช่น “ผม”

## 8. YouTube Lesson Reminder Pattern
ตัวอย่างงานที่เคยตั้ง:
- Video ID: `RoBD7Lc-0MI`
- วิดีโอยาวประมาณ 44:14
- ตั้งสอนตอนเช้า 07:00 เวลาไทย
- รูปแบบผลลัพธ์ที่พี่จอนต้องการ:
  - บทเรียนภาษาไทยแบบสอนจริง
  - มีแก่นสำคัญ
  - มีแบบฝึกหัด
  - มีคำถามทบทวน

หมายเหตุ: งานนี้เป็น reminder เฉพาะครั้ง ไม่ใช่ memory ถาวร แต่ pattern การสอนจากคลิปควรใช้ซ้ำได้

## 9. Open Tasks After Recovery
- ตรวจ config ปัจจุบันว่า primary/fallback provider ตั้งตรงกับ policy หรือยัง
- ตรวจว่า `prefill_messages_file` / slim context bridge ถูก restore แล้วหรือยัง
- ถ้ายังไม่มี ให้สร้าง slim context bridge ใหม่แบบกิน token ต่ำ
- อัปเดต `AI Context Bridge.md` และ `Latest Work Checkpoint.md` ให้สะท้อน recovery ล่าสุด
- พิจารณาสร้าง workflow/skill: “Model Provider Failover Continuity”

## Related
- [[Memory Recovery - 2026-05-27 gap]] — raw recovery log archived under `80 Archive/2026-05-27 recovery raw/`
- [[AI Context Bridge]]
- [[Latest Work Checkpoint]]
- [[Hermes Operating Notes]]
- [[Finance Wallet Guard]]
- [[Daily Learning Loop]]

## Session inventory
- เดิม: 7 session chunks จาก Telegram replies
- หลังจัดระเบียบ: 1 summary note ที่รวมเป็น 7 หัวข้อหลัก + 2 ส่วนช่วยอ่าน

### หัวข้อ session หลัก
1. Model / Provider Continuity
2. Context Continue / Auto-load Workflow
3. Prefill Token Reduction Decision
4. System Prompt Re-anchor Reduction Approval
5. System Prompt Reduction Result
6. Personal Finance Web App / Wallet Guard
7. YouTube Lesson Reminder

### ส่วนช่วยอ่าน
- Daily Learning Loop
- Open tasks after recovery
