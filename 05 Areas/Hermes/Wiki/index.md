---
created: 2026-06-13
status: active
tags:
- hermes
- wiki
- index
type: index
updated: 2026-06-13
---
# Hermes Wiki Index

> Catalog ของ LLM Wiki ฝั่ง Hermes อ่านหน้านี้ก่อนค้น/สร้างหน้าใหม่
> Last updated: 13 มิ.ย. 2026 | Wiki pages: 19

## ใช้อันนี้ยังไง
1. อ่าน [[SCHEMA]] ก่อนถ้าจะสร้าง/แก้หน้า
2. ใช้ index นี้หา page ที่เกี่ยวข้อง
3. อ่าน page ที่เกี่ยวข้อง แล้วค่อยตอบหรืออัปเดต
4. ถ้าคำตอบมีค่าระยะยาว ให้เก็บกลับเข้า `queries/` หรือ `comparisons/`

## Raw
- [[karpathy-llm-wiki-pattern]] — source note ยืนยันแนวคิด LLM Wiki: raw/wiki/schema, ingest/query/lint, index/log
- [[05 Areas/Hermes/Wiki/raw/README|raw README]] — กติกาพื้นที่ต้นทาง

## Entities
- [[hermes-termux-android-backend]] — Android/Termux backend ที่รัน Hermes, path สำคัญ, safety boundary
- [[05 Areas/Hermes/Wiki/entities/README|entities README]] — กติกาหมวด entity

## Concepts
- [[hermes-daily-learning-loop]] — post-task reflection: 5 คำถามหลังจบงานสำคัญ
- [[hermes-obsidian-usage-guide]] — โครงสร้างโฟลเดอร์ Obsidian vault สำหรับ Hermes
- [[hermes-operating-layer]] — วิธีทำงานของเฮอ, safety, memory boundary, เฮอ-ลี่ workflow
- [[hermes-restore-playbook]] — ขั้นตอนกู้ระบบเมื่อ Termux/Hermes หาย
- [[hermes-skills-health-checklist]] — เช็กลิสต์ตรวจสุขภาพ skill หลัง update/restore
- [[hermes-wiki-beginner-guide]] — คู่มือเริ่มต้นใช้งาน Hermes Wiki ตั้งแต่ภาพรวมจนถึงวิธีใช้จริง
- [[05 Areas/Hermes/Wiki/concepts/README|concepts README]] — กติกาหมวด concept

## Comparisons
- [[hermes-agent-model-routing]] — เลือกโมเดล/โปรไฟล์ตามชนิดงานและความเสี่ยง
- [[05 Areas/Hermes/Wiki/comparisons/README|comparisons README]] — กติกาหมวด comparison

## Queries
- [[hermes-knowledge-entry-map]] — ลำดับอ่านบริบท Hermes แบบประหยัด context
- [[05 Areas/Hermes/Wiki/queries/README|queries README]] — กติกาหมวด query

## Core files
- [[SCHEMA]] — กติกา wiki และ Karpathy alignment
- [[index]] — catalog กลางของหน้า wiki ทั้งหมด
- [[log]] — บันทึกการเปลี่ยนแปลง
- [[_roadmap|Wiki Roadmap]] — สถานะความคืบหน้าและแนวขยายต่อ

## ยังไม่ seed ตอนนี้
- `team-ai-workflow` และ `hermes-sub-agent` — เป็น skill-copy / มีความเสี่ยง stale และมีข้อมูล route/profile ต้อง rewrite ก่อน
- `Hermes Agent Technical Details` และ `xurl Tool Details` — fold เข้าหน้า [[hermes-termux-android-backend]] แล้ว ไม่ทำ standalone
- `Finance Wallet Guard` — มี Google Apps Script URL (live endpoint) + ตัวเลขการเงินส่วนตัว เสี่ยงเกิน ใช้แค่ wikilink ไป canonical note แทน