---
tags:
  - hermes
  - context
  - checkpoint
type: latest-work-checkpoint
updated: 2026-05-28
---
# Latest Work Checkpoint

ใช้หน้านี้เป็น “สถานะงานสดล่าสุด” สำหรับ session ใหม่, gateway restart, model switch หรือหลัง restore เพื่อให้เฮอต่อบริบทได้โดยไม่เดาจาก memory อย่างเดียวค่ะ

## Current active state
- หลัง reinstall Termux/Hermes พี่จอนกู้บริบทช่วง 2026-05-27 ด้วยการไล่ reply ข้อความ Telegram ที่ไม่ได้ถูก backup ครบ
- สรุปกู้คืนถูกรวบรวมไว้ที่ [[Recovered Context Summary - 2026-05-27]]
- Vault ถูกจัดให้เป็น backup knowledge base แล้ว โดยหน้าแรกคือ [[START HERE - Hermes Recovery]]
- ตัวตน/โทน/กฎทำงานหลักอยู่ที่ [[Hermes Identity and Operating Rules]]
- วิธี restore หลัง Termux/Hermes หายอยู่ที่ [[Hermes Restore Playbook]]
- ข้อมูลการเงินส่วนตัวอยู่ที่ [[Finance Wallet Guard]]

## Required re-anchor workflow
เมื่อเริ่ม session ใหม่, หลัง gateway restart, หลัง `/reset`, หลัง `/model`, หรือเมื่อพี่ถาม “สถานะงานล่าสุด / งานล่าสุด / เราทำอะไรค้างอยู่”:
1. อ่าน [[START HERE - Hermes Recovery]]
2. อ่าน [[Hermes Identity and Operating Rules]]
3. อ่าน [[AI Context Bridge]]
4. อ่าน [[Latest Work Checkpoint]] นี้
5. ถ้าถามงานล่าสุด ให้เช็ก `todo` + `session_search` + `cronjob list`
6. ตอบสั้น ๆ เป็น active state + next step

## Current open tasks
- Finance dashboard: backend ยังไม่มี public read-only JSON endpoint สำหรับดึงยอดสดจากภายนอกแบบอัตโนมัติ; ตอนนี้เฮอตรวจได้ว่า frontend เรียก `loadAll()` ผ่าน `google.script.run` แต่ดึง live data ตรงจาก URL เดิมไม่ได้
- ถ้าต้องการให้เฮอสรุปการเงินจากเว็บอัตโนมัติ ต้องเพิ่ม read-only endpoint ใน Apps Script เช่น `?api=loadAll&key=...` หรือ route JSON แยก
- ควรเฝ้าดู backup zip ขนาด ~96MB เพราะ GitHub เตือนเรื่องไฟล์ใหญ่ แม้ push ยังสำเร็จ
- ถ้าต้องการความแน่นขึ้น ให้สร้าง workflow/skill “Model Provider Failover Continuity” แยกจาก recovery note

## Auto-save convention
หลังจบงานสำคัญหรือเปลี่ยน state ที่ควรต่อข้าม session ให้เฮออัปเดตหน้านี้แบบสั้น ๆ:
- Current active state
- Decisions / actions completed
- Open blockers
- Next step

ห้ามบันทึก secrets/token/private data ลงหน้านี้ค่ะ

## Session summary convention
เวลาจะสรุป session ยาว ๆ ให้เหลือแค่แกนสำคัญพอสำหรับค้นหาในอนาคต:
- context / เป้าหมาย
- decision ที่ตกลงแล้ว
- สิ่งที่ทำเสร็จ
- blocker / ปัญหา
- next step
- คำค้นสั้น ๆ ที่น่าจะใช้ค้นต่อ

ถ้า session ยาวมาก ให้ตัดรายละเอียดปลีกย่อย/แชทฟุ้ง ๆ ออก แล้วเก็บเป็น bullet สั้น ๆ เท่านั้นค่ะ
