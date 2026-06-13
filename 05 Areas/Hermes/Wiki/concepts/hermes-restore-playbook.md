---
aliases:
- Hermes Restore Playbook
confidence: high
created: 2026-06-13
sources:
- '[[Hermes Restore Playbook]]'
status: active
tags:
- hermes
- wiki
- concept
- recovery
- seed
- stable
updated: 2026-06-13
wiki_type: concept
---
# Hermes Restore Playbook

## คืออะไร
ขั้นตอนกู้ระบบเมื่อ Termux/Hermes หาย, ลงเครื่องใหม่, หรือ context หลุดหนัก

## ขั้นตอนหลัก

### 1. เข้าใจแหล่งความจำ
- **หายแน่เมื่อลบ Termux**: `~/.hermes`, `state.db`, `.env`, logs, cron, custom skills, venv
- **มักรอด**: Obsidian vault ใน `/storage/emulated/0/Documents/Hermes vault`, ไฟล์ shared storage, GitHub backup

### 2. Re-anchor จาก Obsidian
อ่านตามลำดับ: [[START HERE - Hermes Recovery]] → [[Hermes Identity and Operating Rules]] → [[AI Context Bridge]] → [[Latest Work Checkpoint]]

### 3. ตรวจ Hermes state
เฮออ่าน session history จาก `~/.hermes/state.db` ไม่ได้อ่าน Telegram โดยตรง

### 4. ตรวจไฟล์สำคัญ
`config.yaml`, `.env` (ห้ามเปิด secrets), `state.db`, skills/, cron jobs, gateway, vault path

### 5. Model/provider continuity
Primary/fallback ต้องรักษาบริบทเดิม ไม่เริ่มใหม่เหมือนคนแปลกหน้า

### 6. Token policy
Context bridge/prefill ต้อง slim — โหลดเฉพาะแกน ไม่โหลด snapshot ใหญ่ทุกครั้ง

### 7. หลัง restore สำเร็จ
อัปเดต [[Latest Work Checkpoint]] → ใช้ [[05 Areas/Hermes/Wiki/concepts/hermes-daily-learning-loop|Daily Learning Loop]] ปิดงาน

## Related
- [[05 Areas/Hermes/Wiki/concepts/hermes-operating-layer|Hermes Operating Layer]]
- [[05 Areas/Hermes/Wiki/entities/hermes-termux-android-backend|Hermes Termux Android Backend]]
- [[05 Areas/Hermes/Wiki/queries/hermes-knowledge-entry-map|Hermes Knowledge Entry Map]]
- [[05 Areas/Hermes/Wiki/concepts/hermes-skills-health-checklist|Skills Health Checklist]]
