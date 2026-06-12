

---
name: hermes-plan-review
description: Hermes skill สำหรับตรวจแผนที่มีอยู่แล้วแบบจับผิด ก่อนลงมือจริง เฮอจะอ่าน PLAN.md หรือไฟล์แผนที่พี่จอนระบุ แล้วส่งให้ลี่ตรวจความเสี่ยงแบบ adversarial review จนได้ APPROVED หรือครบ MAX_ROUNDS ก่อนขออนุมัติจากพี่จอน
---

# Hermes Plan Review

## Purpose

ใช้เมื่อพี่จอนมีแผนอยู่แล้ว และต้องการให้ Hermes ตรวจความเสี่ยงก่อนลงมือจริง

เหมาะกับงานเช่น:

- Obsidian cleanup plan
- Hermes config plan
- GitHub / code change plan
- automation / cronjob plan
- Android / Termux / root plan
- migration / refactor plan
- งานที่มีไฟล์หรือระบบจริงเกี่ยวข้อง

ไม่ใช่ skill สำหรับเริ่มถาม requirement จากศูนย์ ถ้างานยังไม่ชัด ให้ใช้ `hermes-grill-me` ก่อน

## Roles

- พี่จอน = Owner / Final Approver
- เฮอ = Orchestrator / Planner / Final Reviewer
- ลี่ = Adversarial Reviewer / Critic / Risk Checker

## Default Files

- `PLAN_FILE = PLAN.md`
- `LOG_FILE = PLAN-REVIEW-LOG.md`
- `MAX_ROUNDS = 3`

ถ้าพี่จอนระบุไฟล์แผนอื่น เช่น Obsidian note หรือ path `.md` ให้ใช้ไฟล์นั้นแทน `PLAN.md`

## Flow Overview

1. เฮออ่านแผนจาก `PLAN.md` หรือไฟล์ที่พี่จอนระบุ
2. เฮอสรุปแผนให้พี่จอนเข้าใจสั้น ๆ
3. เฮอตรวจเบื้องต้นว่าแผนแตะไฟล์/ระบบอะไรบ้าง
4. เฮอสร้าง task สำหรับส่งให้ลี่ตรวจใน topic `🔎 research`
5. ลี่ตรวจแบบจับผิดและตอบ `VERDICT: APPROVED` หรือ `VERDICT: REVISE`
6. ถ้า `REVISE` เฮอแก้แผนเฉพาะจุดที่สมเหตุสมผล
7. วนซ้ำไม่เกิน `MAX_ROUNDS`
8. เฮอสรุป final plan และขออนุมัติจากพี่จอน
9. ลงมือจริงได้เฉพาะหลังพี่จอนอนุมัติเท่านั้น

## Step 1: Load Plan

เฮอต้องอ่านแผนให้ครบก่อนตอบ

สิ่งที่ต้องหาในแผน:

- เป้าหมายของงาน
- ขอบเขตงาน
- สิ่งที่ห้ามทำ
- ไฟล์/โฟลเดอร์/ระบบที่จะถูกแตะ
- ขั้นตอนที่จะทำ
- ความเสี่ยงที่แผนเขียนไว้แล้ว
- จุดที่ยังไม่ชัด
- จุดที่ต้อง backup หรือ rollback

ถ้าแผนไม่มีข้อมูลพอ ห้ามเดา ให้ถามพี่จอนก่อน

## Step 2: Summarize Plan

หลังอ่านแผน เฮอต้องสรุปให้พี่จอนแบบสั้น ๆ ก่อนส่งให้ลี่ตรวจ

ใช้รูปแบบนี้:

```md
## Plan Summary

งานนี้จะทำอะไร:
- <สรุปสั้น>

ไฟล์/ระบบที่จะถูกแตะ:
- <รายการ>

สิ่งที่เสี่ยง:
- <รายการ>

สิ่งที่ยังไม่ชัด:
- <รายการ หรือ ไม่มี>
```

## Step 3: Send Review Task To ลี่

เฮอต้องส่ง task สำหรับลี่ตรวจใน topic `🔎 research`

ใช้รูปแบบนี้:

```md
@ลี่ ช่วยตรวจแผนนี้แบบ adversarial review ก่อนลงมือจริง

แผนที่ต้องตรวจ:
<วาง summary หรือ PLAN.md ที่เกี่ยวข้อง>

หน้าที่ของลี่:
1. หา assumption ที่ยังไม่พิสูจน์
2. หา missing edge case
3. หา risk ที่อาจทำให้ไฟล์หาย ระบบพัง หรือ link พัง
4. ตรวจว่า internal link / path / config มีโอกาสผิดไหม
5. ตรวจว่าควร backup อะไรก่อน
6. ตรวจว่ามีขั้นตอนไหน irreversible หรือย้อนกลับยากไหม
7. เสนอ one-line fix สำหรับแต่ละปัญหา
8. ห้ามแก้ไฟล์จริง

ให้ตอบด้วยรูปแบบนี้:

## Review Result

### Issues
- <ปัญหาที่เจอ>

### One-line Fixes
- <วิธีแก้สั้น ๆ>

### Remaining Risks
- <ความเสี่ยงที่ยังเหลือ>

VERDICT: APPROVED
หรือ
VERDICT: REVISE
```

## Step 4: Review Verdict

เมื่อลี่ตอบกลับ เฮอต้องอ่าน verdict บรรทัดสุดท้าย

ถ้าเป็น:

```text
VERDICT: APPROVED
```

ให้ไป Step 6: Final Approval

ถ้าเป็น:

```text
VERDICT: REVISE
```

ให้ไป Step 5: Revise Plan

ถ้าลี่ไม่ใส่ verdict ชัดเจน ให้ถือว่า `REVISE` และถามลี่ให้สรุป verdict ใหม่

## Step 5: Revise Plan

ถ้าแผนต้องแก้ เฮอต้อง:

1. อ่าน feedback ของลี่
2. แยกว่าอะไรควรแก้ / อะไรไม่ควรแก้
3. แก้แผนเฉพาะจุดที่สมเหตุสมผล
4. บันทึกเหตุผลลง `PLAN-REVIEW-LOG.md`
5. ส่งให้ลี่ตรวจซ้ำ ถ้ายังไม่ครบ `MAX_ROUNDS`

รูปแบบ log:

```md
# Plan Review Log: <task>

## Round <n> — ลี่
<feedback ของลี่>

## เฮอตอบกลับ

### Accepted Changes
- <ข้อที่แก้ตาม>

### Rejected Changes
- <ข้อที่ไม่แก้ พร้อมเหตุผล>

### Updated Risks
- <risk หลังแก้>
```

ถ้าครบ `MAX_ROUNDS` แล้วยังไม่ผ่าน ห้าม fake approved ให้ส่ง unresolved issues ให้พี่จอนตัดสิน

## Step 6: Final Approval

ก่อนลงมือจริง เฮอต้องสรุปให้พี่จอนอนุมัติ

ใช้รูปแบบนี้:

```md
## Final Plan Review

สถานะ:
- APPROVED โดยลี่
หรือ
- ยังมี unresolved issues ต้องให้พี่จอนตัดสิน

สิ่งที่จะทำ:
1. <ขั้นตอน>
2. <ขั้นตอน>

ไฟล์/ระบบที่จะถูกแตะ:
- <รายการ>

สิ่งที่ backup ก่อน:
- <รายการ>

ความเสี่ยงที่ยังเหลือ:
- <รายการ>

ขออนุมัติจากพี่จอนก่อนลงมือจริง
```

ถ้าพี่จอนไม่พิมพ์อนุมัติชัดเจน ห้ามลงมือ

## Hard Rules

- ห้ามแก้ไฟล์ก่อนพี่จอนอนุมัติ
- ห้ามลบไฟล์ทันที
- ห้ามให้ลี่แก้ไฟล์จริง
- ห้ามแตะ config/root/token/API key โดยไม่ขออนุมัติ
- ต้อง backup ก่อนงานเสี่ยง
- ถ้าแผนไม่ชัด ให้ถามพี่จอนก่อน
- ถ้าลี่ไม่ให้ verdict ชัดเจน ให้ถือว่ายังไม่ผ่าน
- ถ้าครบ `MAX_ROUNDS` แล้วยังไม่ผ่าน ให้ส่งให้พี่จอนตัดสิน
- ห้าม fake approved ถ้ายังมีปัญหาจริง
- เฮอเป็น final reviewer เสมอ

## When To Use

ใช้ skill นี้เมื่อ:

- มีแผน `.md` อยู่แล้ว
- มี `PLAN.md` อยู่แล้ว
- พี่จอนบอกว่า “ช่วยตรวจแผนก่อนลงมือ”
- งานมีความเสี่ยงต่อไฟล์ config ระบบ หรือข้อมูล
- ต้องการให้ลี่ช่วยจับผิดก่อนทำจริง

## When Not To Use

ไม่ต้องใช้ skill นี้กับ:

- งานที่ยังไม่มีแผนเลย ให้ใช้ `hermes-grill-me` แทน
- งานถามตอบทั่วไป
- งานเขียนข้อความสั้น ๆ
- งานแก้คำผิดเล็กน้อย
- งานที่ไม่แตะไฟล์หรือระบบจริง