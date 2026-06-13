---
status: active
tags:
- hermes
- wiki
- log
type: log
created: 2026-06-13
updated: 2026-06-13
---
# Wiki Log

> Chronological record ของงาน Hermes LLM Wiki

## [2026-06-13] docs | Beginner guide published
- สร้าง `concepts/hermes-wiki-beginner-guide.md`
- สรุปตั้งแต่เริ่มสร้าง Hermes Wiki จนถึงสถานะพร้อมใช้งานจริง
- อธิบายสิ่งที่เปลี่ยนไป, จุดเด่นของโครงใหม่, วิธีใช้สำหรับพี่จอน, และวิธีใช้สำหรับเฮอ
- อัปเดต `index.md` ให้เพิ่มหน้าใหม่และปรับจำนวนเป็น 19 pages
- ใช้หน้านี้เป็นคู่มือเริ่มต้นสำหรับคนและ agent ที่จะเข้ามาใช้ Hermes Wiki ต่อ

## [2026-06-13] polish | Final completion pass
- เพิ่ม `roadmap` เข้า Tag Taxonomy ใน `SCHEMA.md`
- เพิ่ม `Core file policy` ใน `SCHEMA.md` เพื่ออธิบายบทบาทของ `SCHEMA`, `index`, `log`, `_roadmap`, และ README pages ให้ชัด
- ปรับ `_roadmap.md` ให้สะท้อนสถานะจริง: foundation/seed/cleanup เสร็จแล้ว และเหลือ future expansion แบบไม่เร่ง
- อัปเดต `index.md` ให้ใส่ `Wiki Roadmap` ใน core files
- ตรวจซ้ำ broken wikilinks = 0, page count = 18 ตรงกับไฟล์จริง
- หลังรอบนี้ถือว่า Hermes Wiki foundation พร้อมใช้งานจริงแล้ว

## [2026-06-13] cleanup | Schema alignment
- เพิ่ม tags ให้ 9 wiki pages ตรงตาม taxonomy ใน `SCHEMA.md`
- แก้ README tags ให้ใช้ singular ตามหมวด (`entity`, `concept`, `comparison`, `query`)
- เพิ่ม `created` / `updated` ให้ core files และ README ที่ขาด
- ยืนยัน broken wikilinks = 0 และ index count = 18 ตรงจริง

## [2026-06-13] seed | Phase 2 — Content Expansion
- สร้าง 4 wiki pages ใหม่จาก seed candidates ที่มีประโยชน์จริง:
  - `concepts/hermes-daily-learning-loop.md` จาก [[Daily Learning Loop]]
  - `concepts/hermes-skills-health-checklist.md` จาก [[Hermes Built-in Skills Health Checklist]]
  - `concepts/hermes-restore-playbook.md` จาก [[Hermes Restore Playbook]]
  - `concepts/hermes-obsidian-usage-guide.md` จาก [[Obsidian Usage Guide]]
- ข้าม `Finance Wallet Guard` — มี Google Apps Script URL + ตัวเลขการเงินส่วนตัว เสี่ยงเกินสำหรับ wiki
- อัปเดต `index.md` — Concepts +4 หน้า, total = 18 pages
- ครบ 3-5 หน้าตามเป้าหมาย Phase 2 ✅

## [2026-06-13] execute | Phase 1 Foundation Polish ✅
- เพิ่ม explicit tag taxonomy ใน `SCHEMA.md` (structural, domain, operations, quality signals)
- Promote `confidence: high` สำหรับ 2 หน้า stable:
  - `concepts/hermes-operating-layer.md`
  - `comparisons/hermes-agent-model-routing.md`
- สร้าง `_roadmap.md` — Phase 1-4 plan
- สร้าง 3 Wiki templates ใน `09 Templates/`:
  - `Wiki Entity Template.md`
  - `Wiki Concept Template.md`
  - `Wiki Comparison Template.md`
- Verify broken wikilinks = 0 ✅
- ส่งมอบสถานะให้เฮอใน research topic แล้ว

## [2026-06-13] create | Wiki skeleton initialized
- สร้างโครง Wiki เฟสแรกใต้ `05 Areas/Hermes/Wiki/`
- สร้าง `SCHEMA.md`, `index.md`, `log.md`
- เปิดหมวด `raw`, `entities`, `concepts`, `comparisons`, `queries`
- ยังไม่ย้าย note เดิม
- ส่งงาน audit candidate pages + risks ให้ลี่แล้ว

## [2026-06-13] ingest | Karpathy LLM Wiki pattern
- เพิ่ม raw source note: `raw/articles/karpathy-llm-wiki-pattern.md`
- ใช้เป็นหลักตรวจว่าโครงนี้ตรงแนว LLM Wiki: raw/wiki/schema, ingest/query/lint, index/log

## [2026-06-13] seed | Hermes first wave
- สร้าง `comparisons/hermes-agent-model-routing.md` จาก [[Hermes Agent Model Guide]]
- สร้าง `entities/hermes-termux-android-backend.md` จาก [[Hermes Termux Android Context]], [[Hermes Agent Technical Details]], [[xurl Tool Details]]
- สร้าง `concepts/hermes-operating-layer.md` จาก [[Hermes Operating Notes]] และ [[Hermes Identity and Operating Rules]]
- สร้าง `queries/hermes-knowledge-entry-map.md` จาก [[MOC - Hermes]], [[START HERE - Hermes Recovery]], [[Latest Work Checkpoint]]
- อัปเดต `SCHEMA.md` ให้ชิดกับแนว Karpathy มากขึ้น
- อัปเดต `index.md` ให้มี page catalog พร้อม summary
- ยังไม่ seed `team-ai-workflow` / `hermes-sub-agent` เพราะเป็น skill-copy และต้อง rewrite/redact ก่อน

## [2026-06-13] lint | Phase 4 health check pass
- เพิ่ม protocol `ingest/query/lint` ใน `SCHEMA.md`
- แก้ cross-folder wikilinks ให้ resolve ได้จริงด้วย path/alias
- ยืนยัน broken wikilinks ใน wiki = 0 หลังแก้
- เก็บ `team-ai-workflow` และ `hermes-sub-agent` เป็น intentional not-seeded ต่อไปจนกว่าจะ rewrite/redact