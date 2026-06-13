---
status: active
tags:
- hermes
- wiki
- log
type: log
updated: 2026-06-13
---
# Wiki Log

> Chronological record ของงาน Hermes LLM Wiki

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

## [2026-06-13] lint | Phase 4 health check passed
- แก้ตาม review ของลี่: เพิ่ม lint protocol ใน `SCHEMA.md`
- แก้ wikilinks ภายใน seed pages ให้ resolve ด้วย slug links
- ปรับ `hermes-knowledge-entry-map.md` ไม่ให้ดูเหมือน seed skill-copy notes แล้ว
- ตรวจสุขภาพ wiki แล้ว: 13 files, 5 seed pages, issues = 0

## [2026-06-13] lint | Phase 4 health check pass
- เพิ่ม protocol `ingest/query/lint` ใน `SCHEMA.md`
- แก้ cross-folder wikilinks ให้ resolve ได้จริงด้วย path/alias
- ยืนยัน broken wikilinks ใน wiki = 0 หลังแก้
- เก็บ `team-ai-workflow` และ `hermes-sub-agent` เป็น intentional not-seeded ต่อไปจนกว่าจะ rewrite/redact
