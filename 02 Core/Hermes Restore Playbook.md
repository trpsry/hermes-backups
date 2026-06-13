---
tags:
- hermes
- restore
- backup
- termux
- core
type: playbook
status: active
updated: 2026-06-11
---
# Hermes Restore Playbook

ใช้หน้านี้เมื่อ Termux/Hermes หาย, ลงเครื่องใหม่, หรือ context หลุดหนักค่ะ

## 1. เข้าใจแหล่งความจำ
### หายในกรณีลบ Termux
พื้นที่ private ของ Termux จะหาย เช่น:
- `/data/data/com.termux/files/home`
- `~/.hermes`
- `~/.hermes/state.db`
- `.env`
- logs / cron / custom skills / plugins / venv

### มักรอดจากการลบ Termux
- Obsidian vault ใน shared storage:
  `/storage/emulated/0/Documents/Hermes vault`
- ไฟล์ใน `/storage/emulated/0/Download`, Documents ฯลฯ
- GitHub/Git backups ที่ push ไว้

## 2. Re-anchor จาก Obsidian
อ่านตามลำดับ:
1. [[START HERE - Hermes Recovery]]
2. [[Hermes Identity and Operating Rules]]
3. [[AI Context Bridge]]
4. [[Latest Work Checkpoint]]
5. Domain note ตามคำถาม เช่น [[Finance Wallet Guard]] หรือ [[Stock Manager v4 - Project Hub]]
6. ถ้าบริบทยังบางจริง ๆ ค่อยอ่าน [[Recovered Context Summary - 2026-05-27]]

## 3. ตรวจ Hermes state ที่ restore กลับมา
ถ้ามีเครื่องมือ session search ให้เช็ก session history ก่อนตอบเรื่องอดีต

จำไว้:
- เฮอไม่ได้อ่าน Telegram chat history โดยตรง
- เฮออ่าน Hermes session history จาก `~/.hermes/state.db` ถ้าไฟล์นี้ยังอยู่หรือถูก restore กลับมา

## 4. ตรวจไฟล์สำคัญหลัง restore
- `~/.hermes/config.yaml`
- `~/.hermes/.env` ห้ามเปิดเผย secrets
- `~/.hermes/state.db`
- `~/.hermes/skills/`
- cron jobs
- gateway log/status
- Obsidian vault path

## 5. Model/provider continuity
- Primary: OpenAI OAuth Login
- Fallback: OpenRouter + `google/gemini-3.1-flash-lite-preview`
- ถ้าสลับ provider/model ให้ยึด context เดิม ไม่เริ่มใหม่เหมือนคนแปลกหน้า

## 6. Token policy
- Context bridge/prefill ต้อง slim
- โหลดเฉพาะแกนที่ทำให้ไม่หลุดบริบท
- ไม่โหลด snapshot ใหญ่ทุกครั้ง เพราะกิน quota

## 7. หลัง restore สำเร็จ
- อัปเดต [[Latest Work Checkpoint]]
- ถ้าพบ workflow ใหม่ ให้บันทึกเป็น note หรือ skill
- ถ้าพบ bug ที่มีโอกาสเจอซ้ำ ให้สร้าง bug note
- ใช้ [[Daily Learning Loop]] ปิดงาน

## Related
- [[AI Context Bridge]]
- [[Latest Work Checkpoint]]
- [[Recovered Context Summary - 2026-05-27]] — archived recovery reference
