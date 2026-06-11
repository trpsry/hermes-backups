---
created: 2026-05-28 22:38 +07
status: active
tags:
- hermes
- checklist
- skills
- recovery
---
# Hermes Built-in Skills Health Checklist

ใช้โน้ตนี้หลัง **update / reinstall / restore / ย้าย runtime / ล้างไฟล์** เพื่อเช็กว่า skill สำคัญของ Hermes ยังอยู่และโหลดได้จริง

## Quick checklist
- [ ] 1) เช็กว่า runtime หลักยังเป็นตัวที่ตั้งใจใช้
  - `readlink -f $(command -v hermes)`
- [ ] 2) เช็กว่า `~/.hermes/skills/.bundled_manifest` ยังมี entry ของ skill สำคัญ
- [ ] 3) เช็กว่า active file ยังอยู่จริงใน `~/.hermes/skills/.../SKILL.md`
- [ ] 4) เช็กว่า `skills_list()` ยังเห็น skill สำคัญ
- [ ] 5) เช็กว่า `skill_view(name)` โหลดได้จริงอย่างน้อยชุดนี้
  - `hermes-agent`
  - `gateway-troubleshooting`
  - `hermes-memory-vault`
  - `notification-manager`
  - `debugging-hermes-tui-commands`
- [ ] 6) ถ้ามีตัวไหนหาย ให้แยกให้ออกว่า
  - bundled source หาย
  - active local copy หาย
  - manifest ยังอยู่แต่ไฟล์หาย
- [ ] 7) ถ้าเป็น bundled skill ที่หายจาก active library ให้ลอง restore แบบมาตรฐานก่อน
  - `hermes skills reset --restore --yes <skill-name>`
- [ ] 8) หลังแก้แล้ว verify ซ้ำด้วย `skills_list()` และ `skill_view()`

## Current known risk factors
- อย่า `git clone` หรือแก้ไฟล์ตรง ๆ ใน `~/.hermes/skills/` ถ้าไม่จำเป็น
- ถ้ามี Hermes หลาย runtime/path ให้ยึด runtime หลักตัวเดียว
- restore แบบทับทั้งก้อนมีโอกาสทำให้ bundled skill หายจาก active library ได้

## Baseline result — 2026-05-28 22:38 +07
- Runtime check: ควรยึด `/data/data/com.termux/files/usr/bin/hermes`
- `hermes-agent`: โหลดได้
- `gateway-troubleshooting`: โหลดได้
- `hermes-memory-vault`: โหลดได้
- `notification-manager`: โหลดได้
- `debugging-hermes-tui-commands`: โหลดได้
- `~/.hermes/skills/.bundled_manifest`: ยังมี `hermes-agent`
- Active file present: `~/.hermes/skills/autonomous-ai-agents/hermes-agent/SKILL.md`

## If something breaks again
สรุปสั้น ๆ ก่อนเสมอ:
1. skill หายจาก `skills_list()` ไหม
2. `skill_view()` โหลดได้ไหม
3. bundled source ยังอยู่ไหม
4. active local copy ยังอยู่ไหม
5. manifest ยังจำอยู่ไหม

ถ้าตอบ 5 ข้อนี้ได้ จะไล่สาเหตุได้เร็วมากค่ะ
