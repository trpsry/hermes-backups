---
status: active
tags:
  - hermes
  - skills
  - planning
  - review
  - safety
type: skill
source_branch: grill-me-hermes
source_path: hermes-grill-me/SKILL.md
---

# Hermes Grill-Me

## Purpose

ใช้เมื่องานยังไม่ชัด หรืองานมีความเสี่ยง ก่อนให้ Hermes ลงมือจริง

เหมาะกับงานเช่น:

- Hermes config
- Obsidian cleanup
- GitHub changes
- Android / Termux / root
- automation / cronjob
- API key / token
- database / schema / migration
- งานที่อาจทำให้ไฟล์หาย ระบบพัง หรือย้อนกลับยาก

## Roles

- พี่จอน = Owner / Final Approver
- เฮอ = Planner / Manager / Final Reviewer
- ลี่ = Reviewer / Critic / Risk Checker

## Act 1: Grill

เฮอต้องถามพี่จอนทีละคำถามจน requirement ชัดก่อนวางแผน

กติกา:

- ถามทีละ 1 คำถามเท่านั้น
- ถ้ามีข้อมูลจากไฟล์, Obsidian, README, config หรือ docs ให้เปิดอ่านก่อนถาม
- ถ้าข้อมูลไม่พอ ให้ถามเพิ่ม ห้ามเดา
- สรุปคำตอบของพี่จอนเป็น decision ทุกครั้ง
- ถ้ามี tradeoff ให้บอกข้อดี ข้อเสีย และความเสี่ยง
- ถ้างานเกี่ยวกับ config/root/token/API key ต้องเตือนความเสี่ยงก่อนเสมอ

เมื่อ Act 1 จบ ต้องมั่นใจว่าเข้าใจสิ่งเหล่านี้:

- เป้าหมายของงาน
- ขอบเขตงาน
- สิ่งที่ห้ามทำ
- ไฟล์หรือระบบที่จะถูกแตะ
- ความเสี่ยงหลัก
- วิธี backup หรือ rollback
- เงื่อนไขที่ต้องให้พี่จอนอนุมัติ

## Act 2: Plan

เมื่อ requirement ชัด ให้เฮอเขียนแผนเป็น `PLAN.md`

ใช้โครงนี้:

```md
# Plan: <task>

## Goal
<เป้าหมายของงาน>

## Scope
<สิ่งที่จะทำ>

## Out of Scope
<สิ่งที่จะไม่ทำ>

## Files / Systems Involved
<ไฟล์ โฟลเดอร์ หรือระบบที่จะถูกแตะ>

## Approach
1. <ขั้นตอนที่ 1>
2. <ขั้นตอนที่ 2>
3. <ขั้นตอนที่ 3>

## Key Decisions
- <decision สำคัญ>

## Risks
- <ความเสี่ยง>

## Backup / Rollback Plan
- <วิธี backup หรือย้อนกลับ>

## Open Questions
- <คำถามที่ยังไม่ชัด ถ้ามี>

## Approval
รอพี่จอนอนุมัติก่อนลงมือจริง
```

## Act 3: Review Handoff

หลังจากเขียน `PLAN.md` แล้ว เฮอต้องส่งแผนให้ลี่ตรวจใน topic `🔎 research`

Task สำหรับลี่ต้องมี:

```md
@ลี่ ช่วยตรวจแผนนี้แบบจับผิดก่อนลงมือจริง

หน้าที่ของลี่:
- หา assumption ที่ยังไม่พิสูจน์
- หา missing edge case
- หา risk ที่อาจทำให้ไฟล์หาย ระบบพัง หรือ link พัง
- ตรวจว่าควร backup อะไรก่อน
- เสนอ one-line fix สำหรับแต่ละปัญหา
- ห้ามแก้ไฟล์จริง

ให้จบด้วยบรรทัดเดียว:
VERDICT: APPROVED
หรือ
VERDICT: REVISE
```

## Act 4: Revise Loop

ถ้าลี่ตอบ `VERDICT: REVISE`:

- เฮอต้องอ่าน feedback ของลี่
- แก้ `PLAN.md` เฉพาะจุดที่สมเหตุสมผล
- บันทึกว่าแก้อะไร และอะไรไม่แก้ เพราะอะไร
- ส่งให้ลี่ตรวจซ้ำได้
- วนได้สูงสุด `MAX_ROUNDS`

ค่าเริ่มต้น:

- `MAX_ROUNDS = 3`
- `PLAN_FILE = PLAN.md`
- `LOG_FILE = PLAN-REVIEW-LOG.md`

## Act 5: Final Approval

เมื่อแผนผ่านแล้ว หรือครบ `MAX_ROUNDS`:

เฮอต้องสรุปให้พี่จอนว่า:

1. แผนสุดท้ายคืออะไร
2. ลี่เจอปัญหาอะไรบ้าง
3. เฮอแก้อะไรแล้ว
4. ยังมีความเสี่ยงอะไรเหลืออยู่
5. ต้อง backup อะไรก่อน
6. ขออนุมัติก่อนลงมือจริง

ถ้าพี่จอนไม่อนุมัติ ห้ามลงมือ

## Hard Rules

- ห้ามแก้ไฟล์ก่อนพี่จอนอนุมัติ
- ห้ามลบไฟล์ทันที
- ห้ามแตะ config/root/token/API key โดยไม่ขออนุมัติ
- ต้อง backup ก่อนงานเสี่ยง
- ถ้าแผนยังไม่ชัด ให้ถามต่อ
- เฮอเป็น final reviewer
- ลี่ตรวจได้ แต่ห้ามลงมือแก้ระบบ
- ถ้าครบ `MAX_ROUNDS` แล้วยังไม่ผ่าน ให้ส่งให้พี่จอนตัดสิน
- ห้าม fake approved ถ้ายังมีปัญหาจริง

## When Not To Use

ไม่ต้องใช้ skill นี้กับงานเล็กมาก เช่น:

- ถามความหมายคำศัพท์
- เขียนข้อความสั้น ๆ
- แก้คำผิดเล็กน้อย
- งานที่ไม่มีความเสี่ยงและไม่ต้องวางแผน
