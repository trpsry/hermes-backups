---
aliases:
- Skills Health Checklist
confidence: high
created: 2026-06-13
sources:
- '[[Hermes Built-in Skills Health Checklist]]'
status: active
tags:
- hermes
- wiki
- concept
- skills
- recovery
- seed
- stable
updated: 2026-06-13
wiki_type: concept
---
# Skills Health Checklist

## คืออะไร
ชุดตรวจสุขภาพ skill สำคัญของ Hermes ใช้หลัง update / reinstall / restore / ย้าย runtime / ล้างไฟล์

## เช็กลิสต์ 8 ข้อ
1. เช็กว่า runtime หลักยังเป็นตัวที่ตั้งใจใช้ (`readlink -f $(command -v hermes)`)
2. เช็กว่า bundled manifest ยังมี entry ของ skill สำคัญ
3. เช็กว่า active file ยังอยู่จริง
4. เช็กว่า `skills_list()` ยังเห็น skill สำคัญ
5. เช็กว่า `skill_view()` โหลด skill ชุดนี้ได้จริง: `hermes-agent`, `gateway-troubleshooting`, `hermes-memory-vault`, `notification-manager`, `debugging-hermes-tui-commands`
6. ถ้ามีตัวหาย → แยกสาเหตุ: bundled source หาย / active local copy หาย / manifest เหลือแต่ไฟล์หาย
7. Restore มาตรฐาน: `hermes skills reset --restore --yes <skill-name>`
8. Verify ซ้ำหลังแก้

## Diagnostic shortcut (5 คำถาม)
1. skill หายจาก `skills_list()` ไหม
2. `skill_view()` โหลดได้ไหม
3. bundled source ยังอยู่ไหม
4. active local copy ยังอยู่ไหม
5. manifest ยังจำอยู่ไหม

## Related
- [[05 Areas/Hermes/Wiki/concepts/hermes-restore-playbook|Hermes Restore Playbook]] — ใช้ checklist นี้เป็นส่วนหนึ่งของการ restore
- [[05 Areas/Hermes/Wiki/entities/hermes-termux-android-backend|Hermes Termux Android Backend]] — runtime context
