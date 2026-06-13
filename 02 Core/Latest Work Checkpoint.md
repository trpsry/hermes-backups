---
tags:
  - hermes
  - context
  - checkpoint
type: latest-work-checkpoint
updated: 2026-06-11
---
# Latest Work Checkpoint

ใช้หน้านี้เป็น “สถานะงานสดล่าสุด” สำหรับ session ใหม่, gateway restart, model switch หรือหลัง restore เพื่อให้เฮอต่อบริบทได้โดยไม่เดาจาก memory อย่างเดียวค่ะ

## Current active state
- ตอนนี้ continuity backbone อยู่ที่ `02 Core` ชัดแล้ว: [[START HERE - Hermes Recovery]], [[Hermes Identity and Operating Rules]], [[AI Context Bridge]], [[Latest Work Checkpoint]]
- recovery/context reference ที่ไม่ต้องโหลดทุกครั้งถูกย้ายไป archive แล้ว: [[Recovered Context Summary - 2026-05-27]]
- workflow/domain note ถูกจัดเข้า `05 Areas/Hermes` แล้ว เช่น [[Daily Learning Loop]] และ [[Finance Wallet Guard]]
- templates ถูกย้ายออกมาไว้ที่ `09 Templates`
- หลักการ re-anchor ล่าสุดคือ **เริ่มจาก memory + current chat ก่อน** แล้วค่อยอ่าน recovery notes ตามลำดับเท่าที่จำเป็น

## Required re-anchor workflow
เมื่อเริ่ม session ใหม่, หลัง gateway restart, หลัง `/reset`, หลัง `/model`, หรือเมื่อพี่ถาม “สถานะงานล่าสุด / งานล่าสุด / เราทำอะไรค้างอยู่”:
1. ใช้ Hermes memory + current chat เป็นฐานก่อน
2. ถ้าบริบทบาง ค่อยอ่าน [[START HERE - Hermes Recovery]]
3. อ่าน [[Hermes Identity and Operating Rules]]
4. อ่าน [[AI Context Bridge]]
5. อ่าน [[Latest Work Checkpoint]] นี้
6. ถ้าถามงานล่าสุด ให้เช็ก `todo` + `session_search` + `cronjob list`
7. ถ้ายังไม่พอจริง ๆ ค่อยอ่าน [[Recovered Context Summary - 2026-05-27]]
8. ตอบสั้น ๆ เป็น active state + next step

## Current open tasks
- Finance dashboard: backend ยังไม่มี public read-only JSON endpoint สำหรับดึงยอดสดจากภายนอกแบบอัตโนมัติ; ตอนนี้เฮอตรวจได้ว่า frontend เรียก `loadAll()` ผ่าน `google.script.run` แต่ดึง live data ตรงจาก URL เดิมไม่ได้
- ถ้าต้องการให้เฮอสรุปการเงินจากเว็บอัตโนมัติ ต้องเพิ่ม read-only endpoint ใน Apps Script เช่น `?api=loadAll&key=...` หรือ route JSON แยก
- ควรเฝ้าดู backup zip ขนาด ~96MB เพราะ GitHub เตือนเรื่องไฟล์ใหญ่ แม้ push ยังสำเร็จ
- ถ้าต้องการความแน่นขึ้น ให้สร้าง workflow/skill “Model Provider Failover Continuity” แยกจาก recovery note
- Obsidian cleanup phase 3 ยังไม่จำเป็นทันที; ค่อยทำเมื่อมี note ใหม่กระจายหรือมีจุดสับสนเพิ่ม

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
