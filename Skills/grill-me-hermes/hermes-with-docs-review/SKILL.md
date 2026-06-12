

---
name: hermes-with-docs-review
description: Hermes skill สำหรับตรวจแผนโดยอิงเฉพาะเอกสารที่เกี่ยวข้องกับงานนั้นเท่านั้น เช่น CONTEXT.md, README.md, ADR, SOUL.md หรือ Obsidian note ที่พี่จอนระบุ ห้ามอ่านทั้ง vault หรือดึงงานเก่ามาปนเอง
---

# Hermes With Docs Review

## Purpose

ใช้เมื่อแผนต้องอิงเอกสารหรือบริบทเดิมของโปรเจกต์นั้นโดยตรง

เหมาะกับงานเช่น:

- ปรับโครงสร้าง Obsidian เฉพาะโปรเจกต์ที่ระบุ
- ตรวจแผนที่ต้องอิง `CONTEXT.md`
- ตรวจแผนที่ต้องอิง `README.md`
- ตรวจแผนที่ต้องอิง `SOUL.md`
- ตรวจแผนที่ต้องอิง `docs/adr/`
- ตรวจแผนที่ต้องอิง note หรือ folder ที่พี่จอนระบุ path ชัดเจน

ไม่ใช่ skill สำหรับกวาด Obsidian ทั้ง vault หรือดึง note งานเก่ามาปนเอง

ถ้ามีแผนอยู่แล้วแต่ไม่ต้องอิง docs ให้ใช้ `hermes-plan-review` แทน
ถ้างานยังไม่ชัดและต้องถาม requirement ก่อน ให้ใช้ `hermes-grill-me` แทน

## Roles

- พี่จอน = Owner / Final Approver / Scope Setter
- เฮอ = Docs-aware Planner / Manager / Final Reviewer
- ลี่ = Docs Reviewer / Critic / Risk Checker

## Default Files

- `PLAN_FILE = PLAN.md`
- `LOG_FILE = PLAN-REVIEW-LOG.md`
- `MAX_ROUNDS = 3`

ไฟล์ docs ที่อ่านได้ ต้องมาจากหนึ่งในเงื่อนไขนี้เท่านั้น:

1. พี่จอนระบุ path ชัดเจน
2. อยู่ใน folder เดียวกับแผน
3. เป็นไฟล์ context ที่แผนระบุถึงโดยตรง
4. เฮอขออนุมัติพี่จอนก่อนอ่านเพิ่มแล้วพี่จอนอนุมัติ

## Scope Rules

กฎสำคัญที่สุดของ skill นี้:

- ห้ามอ่าน Obsidian ทั้ง vault
- ห้ามดึง note เก่ามาปนเอง
- ห้ามค้นข้ามโปรเจกต์โดยไม่ขออนุมัติ
- ห้ามเดา path เอง
- อ่านเฉพาะไฟล์ที่เกี่ยวข้องกับแผนนี้เท่านั้น
- ถ้าต้องอ่านไฟล์เพิ่ม ให้ถามพี่จอนก่อน
- ถ้าไม่แน่ใจว่าไฟล์เกี่ยวข้องไหม ให้ถือว่า “ยังไม่เกี่ยวข้อง” จนกว่าจะได้รับอนุมัติ

## Flow Overview

1. เฮอรับ path ของแผนหรือ note จากพี่จอน
2. เฮออ่านแผนให้ครบ
3. เฮอระบุรายการ docs ที่จำเป็นต้องอ่าน
4. ถ้า docs ยังไม่ได้รับอนุมัติ ให้ถามพี่จอนก่อน
5. เฮออ่านเฉพาะ docs ที่อยู่ใน scope
6. เฮอสรุปแผน + docs context
7. เฮอส่ง task ให้ลี่ตรวจใน topic `🔎 research`
8. ลี่ตรวจว่าแผนขัดกับ docs หรือมี risk ไหม
9. ถ้า `VERDICT: REVISE` เฮอแก้แผน
10. วนไม่เกิน `MAX_ROUNDS`
11. เฮอสรุป final และขอพี่จอนอนุมัติก่อนลงมือจริง

## Step 1: Load Plan

เฮอต้องอ่านแผนที่พี่จอนระบุให้ครบก่อน

สิ่งที่ต้องหาในแผน:

- เป้าหมายของงาน
- ขอบเขตงาน
- ไฟล์หรือ folder ที่เกี่ยวข้อง
- docs ที่แผนอ้างถึง
- คำศัพท์เฉพาะที่ต้องนิยาม
- decision สำคัญ
- จุดที่เสี่ยงต่อไฟล์/link/context
- จุดที่ยังไม่ชัด

ถ้าแผนไม่ระบุ docs ที่ต้องอ่าน ให้ถามพี่จอนก่อนว่าจะให้อิงไฟล์ไหน

## Step 2: Identify Allowed Docs

ก่อนอ่าน docs เพิ่ม เฮอต้องสรุปรายการไฟล์ที่จะอ่านก่อน

ใช้รูปแบบนี้:

```md
## Docs Scope Check

แผนหลัก:
- <path/to/plan.md>

เอกสารที่จำเป็นต้องอ่าน:
- <path/to/CONTEXT.md> — เหตุผลที่ต้องอ่าน
- <path/to/README.md> — เหตุผลที่ต้องอ่าน

เอกสารที่จะไม่อ่าน:
- ไฟล์นอก scope
- note เก่าใน Obsidian ที่ไม่ได้ระบุ
- vault ทั้งหมด

ขออนุมัติพี่จอนก่อนอ่าน docs เพิ่ม
```

ถ้าพี่จอนไม่อนุมัติ ห้ามอ่านเพิ่ม

## Step 3: Read Docs Context

เมื่อได้รับอนุมัติแล้ว ให้อ่านเฉพาะไฟล์ใน scope

ให้สรุป docs context แบบนี้:

```md
## Docs Context Summary

คำศัพท์สำคัญ:
- <term> = <meaning>

กติกา/ข้อจำกัดจาก docs:
- <rule>

Decision เดิมที่เกี่ยวข้อง:
- <decision>

สิ่งที่แผนต้องห้ามขัด:
- <constraint>
```

ถ้า docs ขัดกันเอง ให้หยุดและถามพี่จอน ไม่ต้องเดาเอง

## Step 4: Challenge The Plan Against Docs

เฮอต้องตรวจแผนเทียบกับ docs ที่อ่านมา

ตรวจเรื่องเหล่านี้:

- แผนใช้คำศัพท์ตรงกับ `CONTEXT.md` ไหม
- แผนขัดกับ `README.md` หรือ architecture เดิมไหม
- แผนขัดกับ ADR เดิมไหม
- แผนแตะไฟล์นอก scope ไหม
- แผนอาจทำให้ Obsidian internal link `[[...]]` พังไหม
- แผนอาจทำให้ context สำคัญหายไหม
- แผนมีการ merge / rename / move / delete ที่เสี่ยงไหม
- แผนมีขั้นตอนที่ rollback ยากไหม

## Step 5: Send Review Task To ลี่

ส่ง task ให้ลี่ตรวจใน topic `🔎 research`

ใช้รูปแบบนี้:

```md
@ลี่ ช่วยตรวจแผนนี้แบบ docs-aware adversarial review

แผนหลัก:
<วาง summary ของแผน>

Docs context ที่อนุญาตให้อ้างอิง:
<วาง summary ของ docs ที่อ่านแล้ว>

Scope ที่ห้ามออกนอก:
- <รายการ scope>

หน้าที่ของลี่:
1. ตรวจว่าแผนขัดกับ docs/context ไหม
2. ตรวจว่ามี note หรือไฟล์นอก scope ปนเข้ามาไหม
3. ตรวจว่า internal link [[...]] อาจพังไหม
4. ตรวจว่า path / folder / filename มีความเสี่ยงไหม
5. ตรวจว่า terminology ใช้ตรงกับ CONTEXT.md ไหม
6. ตรวจว่า decision ขัดกับ ADR เดิมไหม
7. ตรวจว่าควร backup อะไรก่อน
8. เสนอ one-line fix สำหรับแต่ละปัญหา
9. ห้ามแก้ไฟล์จริง

ให้ตอบด้วยรูปแบบนี้:

## Docs-aware Review Result

### Scope Violations
- <มีไฟล์/note นอก scope ปนไหม>

### Docs Conflicts
- <แผนขัดกับ docs จุดไหน>

### Link / Path Risks
- <risk เกี่ยวกับ link หรือ path>

### One-line Fixes
- <วิธีแก้สั้น ๆ>

### Remaining Risks
- <ความเสี่ยงที่ยังเหลือ>

VERDICT: APPROVED
หรือ
VERDICT: REVISE
```

## Step 6: Revise Plan

ถ้าลี่ตอบ `VERDICT: REVISE` เฮอต้อง:

1. อ่าน feedback ของลี่
2. แยกข้อที่ควรแก้ / ไม่ควรแก้
3. แก้แผนเฉพาะจุดที่อยู่ใน scope
4. ห้ามเพิ่ม docs/note ใหม่เข้ามาเอง
5. บันทึกเหตุผลใน `PLAN-REVIEW-LOG.md`
6. ส่งให้ลี่ตรวจซ้ำถ้ายังไม่ครบ `MAX_ROUNDS`

รูปแบบ log:

```md
# Docs-aware Plan Review Log: <task>

## Scope
- <ไฟล์และ docs ที่อนุญาตให้อ่าน>

## Round <n> — ลี่
<feedback ของลี่>

## เฮอตอบกลับ

### Accepted Changes
- <ข้อที่แก้ตาม>

### Rejected Changes
- <ข้อที่ไม่แก้ พร้อมเหตุผล>

### Scope Guard
- <ยืนยันว่าไม่ได้อ่าน/ดึง note นอก scope>

### Updated Risks
- <risk หลังแก้>
```

ถ้าครบ `MAX_ROUNDS` แล้วยังไม่ผ่าน ห้าม fake approved ให้ส่ง unresolved issues ให้พี่จอนตัดสิน

## Step 7: Final Approval

ก่อนลงมือจริง เฮอต้องสรุปให้พี่จอนอนุมัติ

ใช้รูปแบบนี้:

```md
## Final Docs-aware Plan Review

สถานะ:
- APPROVED โดยลี่
หรือ
- ยังมี unresolved issues ต้องให้พี่จอนตัดสิน

แผนสุดท้าย:
1. <ขั้นตอน>
2. <ขั้นตอน>

ไฟล์/ระบบที่จะถูกแตะ:
- <รายการ>

Docs ที่ใช้อ้างอิงจริง:
- <รายการ docs ที่อ่าน>

ยืนยัน scope:
- ไม่ได้อ่านทั้ง vault
- ไม่ได้ดึง note เก่ามาปนเอง
- ใช้เฉพาะไฟล์ที่ได้รับอนุญาต

สิ่งที่ backup ก่อน:
- <รายการ>

ความเสี่ยงที่ยังเหลือ:
- <รายการ>

ขออนุมัติจากพี่จอนก่อนลงมือจริง
```

ถ้าพี่จอนไม่พิมพ์อนุมัติชัดเจน ห้ามลงมือ

## CONTEXT.md Rules

ถ้าใช้ `CONTEXT.md`:

- ใช้เป็น glossary เท่านั้น
- ห้ามใส่ implementation detail
- ห้ามใช้เป็น scratchpad
- ห้ามใช้แทน spec
- เพิ่มคำศัพท์ใหม่ได้เฉพาะเมื่อพี่จอนอนุมัติ

## ADR Rules

ADR = Architecture Decision Record หรือบันทึกเหตุผลของ decision สำคัญ

เสนอ ADR เฉพาะเมื่อครบ 3 เงื่อนไข:

1. Decision นั้นย้อนกลับยาก
2. คนอ่านในอนาคตอาจสงสัยว่าทำไมเลือกแบบนี้
3. มี tradeoff จริง ไม่ใช่เรื่อง obvious

ถ้าจะสร้าง ADR ต้องขออนุมัติพี่จอนก่อน

## Obsidian Rules

ถ้างานเกี่ยวกับ Obsidian:

- ห้ามอ่านทั้ง vault
- ห้ามลบ note ทันที
- ห้าม rename/move note จำนวนมากโดยไม่ backup
- ห้าม merge note โดยไม่สรุปว่าจะรวมอะไรเข้ากับอะไร
- ต้องตรวจ internal link `[[...]]` ก่อน move/rename
- ถ้าไม่แน่ใจ ให้ใช้ `99_Archive` แทนการลบ
- ถ้าจะอ่าน note เพิ่ม ต้องขออนุมัติก่อน

## Hard Rules

- ห้ามแก้ไฟล์ก่อนพี่จอนอนุมัติ
- ห้ามอ่านไฟล์นอก scope โดยไม่ขออนุมัติ
- ห้ามอ่าน Obsidian ทั้ง vault
- ห้ามดึง note เก่ามาปนเอง
- ห้ามเดา path เอง
- ห้ามลบไฟล์ทันที
- ห้ามให้ลี่แก้ไฟล์จริง
- ห้ามแตะ config/root/token/API key โดยไม่ขออนุมัติ
- ต้อง backup ก่อนงานเสี่ยง
- ถ้า docs ขัดกันเอง ให้ถามพี่จอน
- ถ้าลี่ไม่ให้ verdict ชัดเจน ให้ถือว่ายังไม่ผ่าน
- ถ้าครบ `MAX_ROUNDS` แล้วยังไม่ผ่าน ให้ส่งให้พี่จอนตัดสิน
- ห้าม fake approved ถ้ายังมีปัญหาจริง
- เฮอเป็น final reviewer เสมอ

## When To Use

ใช้ skill นี้เมื่อ:

- แผนต้องอิง docs เดิม
- พี่จอนระบุ path ของ Obsidian note หรือ docs ชัดเจน
- ต้องตรวจว่าแผนขัดกับ `CONTEXT.md`, `README.md`, `SOUL.md`, ADR หรือไม่
- งานมี risk เรื่อง link/path/context
- ต้องการตรวจ scope อย่างเข้ม

## When Not To Use

ไม่ต้องใช้ skill นี้กับ:

- งานที่ไม่ต้องอิง docs เดิม
- งานที่มีแผนแล้วแต่ตรวจทั่วไป ใช้ `hermes-plan-review` แทน
- งานที่ยังไม่มีแผนเลย ใช้ `hermes-grill-me` แทน
- งานถามตอบทั่วไป
- งานเขียนข้อความสั้น ๆ
- งานที่ไม่แตะไฟล์หรือระบบจริง