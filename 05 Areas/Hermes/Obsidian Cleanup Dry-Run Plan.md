---
created: 2026-06-11
status: completed
tags:
- obsidian
- cleanup
- plan
- hermes
type: plan
updated: 2026-06-11
---
## Execution update
เดิมหน้านี้เป็น dry-run plan ค่ะ ตอนนี้หลังได้รับอนุมัติแล้ว:
- phase 1 เสร็จแล้ว: ย้าย `AI Context Bridge` และ `Latest Work Checkpoint` เข้า `00 Core`
- phase 2 เสร็จแล้ว: ย้าย `Daily Learning Loop` และ `Finance Wallet Guard` เข้า `20 Areas/Hermes`, ย้าย recovery/archive notes เข้า `80 Archive/Hermes Recovery`, และย้าย templates ไป `90 Templates`
- ใช้หน้านี้เป็นบันทึกแผนเดิม + audit trail ได้ แต่สถานะล่าสุดให้ดู `[[MOC - Hermes]]` และ `[[Latest Work Checkpoint]]` เพิ่ม


- phase 3 เสร็จแล้ว (2026-06-12): merge orphan root note คำสั่ง termux.md → Hermes Termux Android Context, delete root orphan, update MOC - Hermes ให้ครอบคลุมทุก note, verified zero broken links
# Obsidian Cleanup Dry-Run Plan

## Model
ใช้ **GPT-5.5 Thinking** เป็นตัวหลักสำหรับงานนี้

**เหตุผล:**
- ต้องอ่านโครงสร้าง vault
- ต้องแยกไฟล์ซ้ำ
- ต้องวางแผนย้ายไฟล์อย่างปลอดภัย
- ต้องระวัง link พัง / ไฟล์หาย / secret หลุด

---

## Goal
จัดระเบียบ Obsidian Vault ให้ Hermes Agent ใช้งานง่ายขึ้น โดยยังไม่แตะไฟล์จริง

**Vault:** Hermes Agent/Obsidian

---

## Safety Rules
- **DRY-RUN ONLY**
- ห้ามย้ายไฟล์จริง
- ห้ามลบไฟล์
- ห้าม overwrite
- ห้ามแก้ `.obsidian`
- ห้ามแสดง token / API key / password
- ใช้ MCP เฉพาะ read / list / search
- **ต้องรออนุมัติจากพี่จอนก่อนลงมือจริง**

---

## Current Problem
Hermes-related notes กระจายหลายที่:
- `Obsidian/Hermes`
- `00 Core`
- `20 Areas/Hermes`
- `50 MOCs/MOC - Hermes.md`

ทำให้ Hermes อาจค้นเจอไฟล์ซ้ำ หรือดึงข้อมูลเก่ามาตอบ

---

## Target Structure
```
Obsidian/
├── 00 Inbox/
├── 00 Core/
│   ├── START HERE - Hermes Recovery.md
│   ├── Hermes Identity and Operating Rules.md
│   ├── AI Context Bridge.md
│   └── Latest Work Checkpoint.md
│
├── 10 Projects/
├── 20 Areas/
│   └── Hermes/
│       ├── Hermes Operating Notes.md
│       ├── Hermes Termux Android Context.md
│       ├── Hermes Agent Technical Details.md
│       ├── Firecrawl Setup.md
│       ├── Obsidian MCP Setup.md
│       └── Termux Commands.md
│
├── 30 Resources/
├── 50 MOCs/
│   └── MOC - Hermes.md
│
├── 80 Archive/
│   └── Recovery/
│
└── 90 Templates/
```

---

## Migration Plan
1. อ่าน tree ปัจจุบันของ vault
2. หาไฟล์ซ้ำ / โฟลเดอร์ซ้ำ / note ที่อยู่ผิดที่
3. เสนอ mapping ว่าไฟล์ไหนควรไปอยู่ไหน
4. ห้ามย้ายจริง ให้ทำเป็น **dry-run report** เท่านั้น
5. ตรวจ link สำคัญใน MOC - Hermes / START HERE / Latest Work Checkpoint
6. เสนอไฟล์ที่ควร archive
7. เสนอไฟล์ที่ควรสร้างเพิ่ม เช่น `Firecrawl Setup.md` และ `Obsidian MCP Setup.md`
8. สรุปข้อดี ข้อเสีย ความเสี่ยง
9. รอคำว่า **"อนุมัติ"** จากพี่จอนก่อนลงมือจริง

---

## Expected Output
ให้ตอบกลับเป็น .md โดยมีหัวข้อ:

```
# Obsidian Cleanup Report

## Current Tree
## Duplicate / Confusing Areas
## Recommended Target Structure
## File Mapping
## Files To Keep
## Files To Archive
## Link Update Needed
## Risks
## Approval Required
```

---

## Approval Rule
**ห้ามทำจริงจนกว่าผู้ใช้จะพิมพ์:**

> อนุมัติให้เริ่มจัด Obsidian ตามแผน dry-run นี้

## Execution Plan

### Working assumptions
- งานนี้ยังเป็น **DRY-RUN ONLY**
- ยังไม่ย้ายไฟล์จริง
- ยังไม่ลบไฟล์
- ยังไม่แก้ `.obsidian`
- ถ้าจะลงมือจริงค่อยทำหลังได้รับคำว่า **อนุมัติ**

### Proposed working phases
1. **Inventory** — รวบรวมรายชื่อ note Hermes ที่อยู่ใน `00 Core`, `20 Areas/Hermes`, `Hermes/`, `50 MOCs`, `30 Resources`, `80 Archive`
2. **Classification** — แยกแต่ละ note เป็น `Core`, `Active Ops`, `Reference`, `Archive candidate`
3. **Dry-run mapping** — เสนอว่า note ไหนควรอยู่ที่ไหน โดยยังไม่ move จริง
4. **Continuity review** — เช็กผลกระทบกับ `START HERE - Hermes Recovery`, `Hermes Identity and Operating Rules`, `AI Context Bridge`, `Latest Work Checkpoint`, `MOC - Hermes`
5. **Archive proposal** — แยกว่าอะไรควร keep active และอะไรควร archive
6. **Final report** — สรุปเป็น markdown ตาม output format ในแผนเดิม
7. **Approval gate** — รออนุมัติก่อนเริ่มจัดจริง

### Role split
**ให้ลี่ช่วยได้**
- ทำ inventory tree และรวบรวม note ที่เกี่ยวกับ Hermes
- ทำรายการจุดซ้ำ / จุดสับสน / note ที่น่าจะอยู่ผิดที่
- ร่าง mapping แบบ dry-run
- ช่วยเช็กว่า note ไหนควร keep / archive
- ช่วยรวบรวมจุดที่ต้องอัปเดตลิงก์หลังย้ายจริงในอนาคต

**เฮอต้องคุมเอง**
- ตัดสินใจว่าอะไรเป็น source of truth ของ Hermes
- ตัดสินใจ final สำหรับ note backbone ของ continuity/recovery
- ตรวจความเสี่ยงเรื่อง context หลุด, link พัง, หรือดึงข้อมูลเก่ามาตอบ
- รีวิว final report ก่อนเสนอพี่จอน
- ขออนุมัติก่อนมี side effects จริง

### Early findings from initial scan
- ตอนนี้ note Hermes กระจายอย่างน้อยใน `00 Core`, `20 Areas/Hermes`, `Hermes/`, `50 MOCs`, `30 Resources`, `80 Archive`
- จุดที่น่าต้องคุมมากที่สุดคือ `AI Context Bridge` กับ `Latest Work Checkpoint` เพราะยังอยู่นอก `00 Core`
- `MOC - Hermes` เป็นตัวเชื่อมหลัก จึงต้องเช็กผลกระทบของลิงก์ก่อนย้ายจริง
- เบื้องต้นยังไม่เห็น broken wikilinks จากการตรวจรอบแรก แต่ยังต้องตรวจละเอียดอีกครั้งตอนทำ dry-run report เต็ม

### Suggested next move
- ถ้าจะเริ่มงานจริงรอบ dry-run ให้เริ่มจาก inventory + classification ก่อน
- หลังจากนั้นค่อยให้ลี่ร่าง mapping proposal แล้วเฮอตรวจ final
