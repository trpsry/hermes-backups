---
created: 2026-06-13
updated: 2026-06-13
status: approved-plan
tags:
  - hermes
  - llm-wiki
  - knowledge-base
  - plan
type: plan
---

# Plan: Hermes LLM Wiki Upgrade

## Goal
ปรับ Knowledge Base ของเฮอใน Obsidian ให้เป็น **Hybrid LLM Wiki** แบบปลอดภัย

ความหมายแบบง่าย:
- ของเดิมยังอยู่เหมือนเดิม
- เพิ่มพื้นที่ Wiki สำหรับความรู้ที่ต้องโตต่อได้
- ให้ AI ช่วยจัดหมวด สรุป เชื่อมลิงก์ และตรวจสุขภาพความรู้

## Scope
ทำเฉพาะโดเมนแรกก่อน:
- Hermes
- AI workflow
- agent systems
- memory / skills / multi-agent workflow

สิ่งที่จะทำในเฟสแรก:
- ออกแบบโครง `05 Areas/Hermes/Wiki/`
- สร้างกติกา `SCHEMA.md`
- สร้างสารบัญ `index.md`
- สร้างบันทึกการเปลี่ยนแปลง `log.md`
- สร้างหมวด wiki:
  - `raw/`
  - `entities/`
  - `concepts/`
  - `comparisons/`
  - `queries/`

## Out of Scope
รอบแรกยังไม่ทำ:
- ไม่ย้ายทั้ง vault
- ไม่อ่านทั้ง vault
- ไม่ลบ note เดิม
- ไม่ rename note เดิม
- ไม่แก้ canonical recovery / identity notes แบบหนัก ๆ
- ไม่ auto-ingest ทั้ง vault
- ไม่ตั้ง cron
- ไม่ backup ขึ้น git ทุกครั้งที่แก้ vault

## Files / Systems Involved
ไฟล์/พื้นที่ที่จะเกี่ยวข้องเมื่อเริ่มลงมือจริง:
- `/storage/emulated/0/Documents/Hermes Vault/05 Areas/Hermes/Wiki/`
- `05 Areas/Hermes/Wiki/SCHEMA.md`
- `05 Areas/Hermes/Wiki/index.md`
- `05 Areas/Hermes/Wiki/log.md`
- `07 MOCs/MOC - Hermes.md` อาจเพิ่มลิงก์เข้า Wiki ภายหลังเท่านั้น

## Model / Profile Split
### เฮอ / default
โมเดลหลัก: **GPT-5.5**

หน้าที่:
- วางแผนหลัก
- ตัดสินใจโครงสร้าง
- ตรวจความเสี่ยง
- ตรวจ final
- ขออนุมัติพี่จอนก่อนลงมือจริงในจุดเสี่ยง

### ลี่ / secondmodel
โมเดลหลัก: **DeepSeek V4 Pro**

หน้าที่:
- audit note เฉพาะกลุ่มที่เฮอเลือก ไม่อ่านทั้ง vault
- เสนอ candidate pages
- จับผิดแผน
- หา risk เรื่อง link / duplicate / scope creep
- ห้ามแก้ไฟล์จริงเองถ้าเฮอไม่ได้มอบหมายชัด

## Hybrid Boundary
เส้นแบ่งว่าอะไรควรอยู่ตรงไหน:

- **Memory** = preference / facts สั้น ๆ ของพี่จอนและระบบ
- **Skills** = ขั้นตอนทำงานที่ใช้ซ้ำได้
- **Session** = ประวัติการคุยและงานชั่วคราว
- **LLM Wiki** = ความรู้ระยะยาวที่ต้องเชื่อมโยงและโตต่อได้ เช่น concept, comparison, source summary, decision note
- **Canonical notes** = กฎหลัก / recovery / identity / operating rules ห้ามย้ายหรือเขียนทับโดยไม่ขออนุมัติแยก

## Approach
### Phase 0 — Pre-check
1. เช็กว่า target path `05 Areas/Hermes/Wiki/` มีอยู่แล้วไหม
2. ถ้ามีอยู่แล้ว ให้ inventory สั้น ๆ แล้วถามพี่จอนก่อน merge / rename / ใช้ต่อ
3. เช็ก repo backup `hermes-backups` ว่ามีจริงและ push ได้
4. backup ขึ้น git เฉพาะก่อนเริ่มงานใหญ่ หรือเมื่อพี่จอนสั่ง

### Phase 1 — Create Skeleton
1. สร้างโครง `Wiki/`
2. สร้าง `SCHEMA.md`
3. สร้าง `index.md`
4. สร้าง `log.md`
5. ยังไม่ย้าย note เดิม

### Phase 2 — Seed รอบแรกแบบเบา
1. เลือก note เดิม 5–8 หน้าเท่านั้น
2. สร้าง wiki page ใหม่แบบสรุป ไม่ copy ทั้ง note
3. ใส่ `sources:` อ้างกลับไปหา note เดิม
4. wiki page ลิงก์ไป canonical note ได้ แต่ห้ามแก้ canonical note
5. ถ้า note เดิมรกหรือข้อมูลไม่พอ ให้ข้ามก่อน

### Phase 3 — Query-back Loop
1. ถ้าคำตอบในแชทเป็นการวิเคราะห์ลึก / comparison / decision สำคัญ ให้เก็บเข้า `queries/` หรือ `comparisons/`
2. ถ้าเป็นคำตอบสั้นทั่วไป ไม่ต้องเก็บ
3. ถ้าไม่แน่ใจ ให้ถามพี่จอนก่อน

### Phase 4 — Lint / Health Check
ตรวจเป็นรอบ ๆ:
- broken wikilinks
- orphan pages
- duplicate pages
- tag นอก schema
- `index.md` ไม่ตรงกับไฟล์จริง
- queries ที่เก่าเกิน 90 วัน
- page ยาวเกิน 200 lines

## Key Decisions
- ใช้ Hybrid LLM Wiki ไม่รื้อของเดิม
- เริ่มเฉพาะ Hermes / AI workflow ก่อน
- Wiki อยู่ใต้ `05 Areas/Hermes/Wiki/`
- รอบแรกใช้ derive + link ไม่ copy note ดิบ
- canonical notes เป็น read-only source ในเฟสแรก
- backup ขึ้น git เฉพาะก่อนเริ่มงานใหญ่หรือเมื่อพี่จอนสั่ง
- เฮอคุม final, ลี่ช่วย audit/review

## Risks
- Wiki รกถ้า schema ไม่ชัด
- สร้าง note ซ้ำถ้าไม่เช็ก index ก่อน
- link พังถ้าไปย้าย note เดิม
- queries โตไม่หยุดถ้าไม่มี cleanup
- canonical note เสียความหมายถ้าเผลอเขียนทับ
- Obsidian sync conflict ถ้าแก้หลายเครื่องพร้อมกัน

## Backup / Rollback Plan
### Backup Policy
- backup ขึ้น git ก่อนเริ่มงานใหญ่
- backup เพิ่มเมื่อพี่จอนสั่ง
- ไม่ backup ทุกครั้งที่แก้ vault ย่อย ๆ

### Before Real Changes
- เช็ก `git status` ใน `hermes-backups`
- push backup ให้เรียบร้อยก่อนเริ่ม phase ที่แตะไฟล์จริง

### Rollback
- ถ้าเป็นไฟล์ใน vault: ใช้ git restore / git checkout จาก backup repo
- ถ้าเป็นโครง wiki ใหม่: ลบเฉพาะไฟล์ที่สร้างใน `Wiki/` หลังขออนุมัติ
- ห้าม rollback กว้าง ๆ ทั้ง vault โดยไม่ถามพี่จอน

## วิธีใช้งานหลังทำเสร็จ
### เวลา ingest source ใหม่
1. เก็บ source เข้า `raw/`
2. อ่าน `SCHEMA.md`
3. เช็ก `index.md`
4. สร้าง/อัปเดต page ที่เกี่ยวข้อง
5. อัปเดต `index.md` และ `log.md`

### เวลา query ความรู้
1. อ่าน `index.md`
2. อ่าน page ที่เกี่ยวข้อง
3. ตอบโดยอ้างอิง wiki pages
4. ถ้าคำตอบมีคุณค่าเก็บยาว ให้บันทึกกลับเข้า `queries/` หรือ `comparisons/`

### เวลา health check
ให้ AI ตรวจ:
- link พัง
- note โดดเดี่ยว
- note ซ้ำ
- index ไม่ตรงไฟล์จริง
- query เก่าเกิน 90 วัน

## Before / After
### Before
- ความรู้กระจายอยู่ใน memory / session / skills / Obsidian
- output ดี ๆ บางอย่างหายไปในแชท
- Obsidian มี note สำคัญ แต่ยังไม่ใช่ wiki ที่โตแบบเป็นระบบ
- เวลาเริ่มงานใหม่ ต้องค้นหลายที่

### After
- มีพื้นที่ LLM Wiki แยกชัด
- ความรู้เชิงลึกมีที่ลงถาวร
- AI อ่าน `index.md` แล้วเข้าใจภาพรวมเร็วขึ้น
- มี `log.md` บอกว่า wiki เปลี่ยนอะไรไปบ้าง
- ลดการสร้าง note ซ้ำด้วย schema + index

### ดีขึ้น
- continuity ดีขึ้น
- ความรู้โตแบบทบต้น
- ใช้ Obsidian เป็นสมองระยะยาวได้ชัดขึ้น
- ลี่ช่วย audit ได้เป็นระบบขึ้น

### แย่ลง / tradeoff
- มีโครงสร้างเพิ่ม ต้องดูแลเพิ่ม
- ถ้าไม่ lint เป็นรอบ ๆ wiki จะรก
- ถ้า schema ไม่ชัด จะกลายเป็น note กองใหม่
- ใช้ token เพิ่มตอน ingest / lint

## Open Questions
- Seed รอบแรกจะใช้ note ชุดไหน 5–8 หน้า
- ต้องการให้ queries เก็บอัตโนมัติแค่ไหน หรือถามก่อนทุกครั้ง
- จะตั้ง `WIKI_PATH` ใน `.env` ไหม หรือใช้ path จากแผนเท่านั้นก่อน

## Approval
พี่จอนอนุมัติให้เขียน PLAN.md และส่งงานเข้า Header แล้วเมื่อ 13 มิ.ย. 2026

ขั้นถัดไปหลังจากนี้:
1. ส่งแผนเข้า Header
2. รอพี่จอนสั่งเริ่ม Phase 0 / Phase 1
3. ก่อนแตะไฟล์จริงเพิ่มเติม ให้ทำ pre-check และ backup ตาม policy
