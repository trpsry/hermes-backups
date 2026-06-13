---
status: active
tags:
- hermes
- wiki
- schema
type: schema
created: 2026-06-13
updated: 2026-06-13
---
# SCHEMA

## Domain
Hermes / AI workflow / agent systems / memory / skills / multi-agent workflow

## Karpathy LLM Wiki fit
แนวนี้ตามไอเดียของ Andrej Karpathy: ไม่ใช่แค่ค้น note ตอนถาม แต่ให้ AI ค่อย ๆ สร้าง wiki ที่เชื่อมโยงกันและโตต่อได้

มี 3 ชั้น:
- `raw/` = แหล่งข้อมูลต้นทาง อ่านได้ ห้ามแก้มั่ว
- wiki pages = หน้าสรุป/เชื่อมโยงที่ AI ดูแล เช่น entity, concept, comparison, query
- `SCHEMA.md` = กติกาที่ทำให้ AI เขียน wiki แบบมีวินัย

## หมวดหลัก
- `raw/` = source / excerpt / note ต้นทาง
- `entities/` = คน ระบบ เครื่องมือ โปรไฟล์ หรือองค์ประกอบที่เป็นตัวตนชัด
- `concepts/` = แนวคิด กฎ วิธีคิด ลำดับการทำงาน
- `comparisons/` = เทียบ A vs B / ทางเลือก / tradeoff
- `queries/` = คำตอบเชิงวิเคราะห์ที่ควรเก็บกลับมาใช้

## Frontmatter ขั้นต่ำ
```yaml
status: draft|active
wiki_type: raw|entity|concept|comparison|query
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources:
  - source note or wikilink
confidence: high|medium|low
```

## กติกาเขียนหน้าใหม่
1. ห้าม copy note เดิมทั้งหน้า
2. ให้สรุปใหม่แบบสั้น แล้วใส่ `sources:` กลับไปหา note ต้นทาง
3. ทุกหน้า wiki ต้องมีลิงก์ออกอย่างน้อย 2 จุด ถ้าทำได้
4. ถ้าเกี่ยวกับ recovery / identity / operating rules ให้ลิงก์หา note หลัก แทนการเขียนทับ
5. หน้าใหม่ต้องถูกเพิ่มใน `index.md`
6. ทุกงานต้องลง `log.md`
7. ถ้าเป็นข้อมูลเร็ว/เปลี่ยนบ่อย ให้ใส่ `confidence: medium` หรือ `low`
8. ถ้าเจอข้อมูลขัดกัน ให้เขียนไว้ตรง ๆ ไม่เขียนทับเงียบ ๆ

## Tag Taxonomy
ต้องเพิ่ม tag ใหม่ที่นี่ก่อนใช้จริง ห้ามใช้ tag ที่ไม่อยู่ใน taxonomy เพื่อป้องกัน tag sprawl

### Structural (ทุกหน้า wiki ต้องมี)
- `hermes` — ทุกหน้าต้องมี tag นี้
- `wiki` — ใช้กับทุกหน้าที่อยู่ใน Wiki
- `entity`, `concept`, `comparison`, `query` — ตาม wiki_type ของหน้า
- `raw` — สำหรับ source notes ใน raw/
- `schema`, `index`, `log`, `roadmap` — core files

### Domain / Technical
- `agent` — เกี่ยวกับ agent architecture, runtime
- `model` — เกี่ยวกับ model/provider selection
- `routing` — เกี่ยวกับ task routing, profile split
- `workflow` — เกี่ยวกับ workflow, automation
- `multi-agent` — เกี่ยวกับ multi-agent, team collaboration
- `android`, `termux` — แพลตฟอร์ม / environment

### Operations
- `safety` — กฎความปลอดภัย, boundary
- `recovery` — restore, playbook, continuity
- `memory` — memory architecture, context
- `skills` — skill management, skill health
- `config` — configuration, setup
- `backup` — backup procedures

### Quality signals
- `stable` — หน้าที่ผ่านการใช้งานและตรวจสอบแล้ว
- `seed` — หน้าที่สร้างจาก seed candidate
- `draft` — หน้ายังไม่สมบูรณ์

### Rule
ทุก tag บนหน้า wiki ต้องปรากฏใน taxonomy นี้ ถ้าต้องการ tag ใหม่ให้เพิ่มที่นี่ก่อน

## Core file policy
- `SCHEMA`, `index`, `log`, `_roadmap` เป็น core files ของ wiki
- core files ใช้ `type:` ได้ตามบทบาทจริง (`schema`, `index`, `log`, `roadmap`) และไม่จำเป็นต้องมี `wiki_type`, `sources`, `confidence`
- หน้า README ของแต่ละหมวดเป็นหน้าชี้ทางภายในหมวด ใช้ `wiki_type` ตามหมวดได้ และไม่จำเป็นต้องมี `sources` หรือ `confidence`

## Ingest / query / lint protocol
- Ingest = เพิ่ม raw source ก่อน แล้วค่อยสร้างหรืออัปเดต wiki page ที่เกี่ยว พร้อมอัปเดต `index.md` และ `log.md`
- Query = ตอบจาก wiki page ที่มีอยู่ก่อน ถ้าคำตอบใหม่มีคุณค่าระยะยาว ค่อยเก็บกลับเข้า `queries/` หรือ `comparisons/`
- Lint = ทุกครั้งที่ทำเฟสตรวจสุขภาพ ให้เช็กอย่างน้อย 6 อย่างนี้
  1. broken wikilinks
  2. ลิงก์ข้ามหมวดใช้ path/alias ที่ resolve ได้จริง
  3. `index.md` นับจำนวนหน้าและสรุปตรงกับไฟล์จริง
  4. `log.md` ลงเหตุการณ์ล่าสุดครบ
  5. หน้าใหม่มี `sources:` และ `updated:` ครบ
  6. ไม่มี token, route, credential, หรือรายละเอียดเสี่ยงหลุดเข้ามา

## Page threshold
สร้างหน้าใหม่เมื่อ:
- เป็นแกนหลักของโดเมน Hermes/agent workflow หรือ
- มีประโยชน์กับการตัดสินใจซ้ำในอนาคต หรือ
- เป็น query/comparison ที่ถ้าหายไปในแชทจะเสียดาย

อย่าสร้างหน้าใหม่เมื่อ:
- เป็นรายละเอียดเล็กมาก
- เป็น note บาง ๆ ที่ควร fold เข้า page ใหญ่กว่า
- เป็นข้อมูล sensitive / route / token / ID ที่ไม่ควรเผยใน wiki

## ห้ามทำในเฟสแรก
- ห้ามย้าย note เดิม
- ห้าม rename note เดิม
- ห้ามแตะ canonical notes โดยตรง
- ห้าม ingest ทั้ง vault
- ห้ามตั้ง cron อัตโนมัติ
- ห้าม seed skill-copy notes แบบ copy ตรง ๆ
