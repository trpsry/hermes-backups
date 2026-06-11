---
tags:
- hermes
- context
- ai
- continuity
type: context-bridge
updated: 2026-05-28
status: active
---
# AI Context Bridge

## Purpose
ใช้หน้านี้เป็นสะพานต่อบริบทเวลาเปลี่ยน model/provider, new session, `/reset`, gateway restart หรือหลัง restore จาก backup

## Continuity guarantee
เฮอต้องไม่เริ่มใหม่เหมือนคนแปลกหน้าเมื่อสลับ model/provider ให้ re-anchor จาก:
- [[START HERE - Hermes Recovery]]
- [[Hermes Identity and Operating Rules]]
- [[Latest Work Checkpoint]]
- Hermes memory/current chat
- session history เฉพาะตอนถามเรื่องงานล่าสุด/สถานะงาน

## Model routing & failover
Primary provider:
- OpenAI OAuth Login / `openai-codex`

Primary models:
- `gpt-5.5` = architecture / วิเคราะห์ยาก / decision สำคัญ
- `gpt-5.4` = coding / debugging / workflow
- `gpt-5.4-mini` = งานทั่วไป / สรุป / คุย / งานเบา

Fallback provider:
- OpenRouter
- fallback model: `google/gemini-3.1-flash-lite-preview`

Fallback rule:
- ใช้เมื่อ OpenAI OAuth มีปัญหา, server outage, quota/rate-limit, overload หรือ connection failure
- ถ้า fallback ทำงาน ให้ต่อบริบทเดิม ไม่เริ่มใหม่
- ถ้าบริบทดูบาง ให้สรุป active state ก่อน แล้วค่อยทำงานต่อ

## Slim context principle
John ต้องการลด token/quota:
- auto-load/prefill ควรโหลดเฉพาะแกนสำคัญที่ทำให้ไม่หลุดบริบท
- ไม่โหลด full snapshot ใหญ่ทุกครั้ง
- ถ้างานไหนซับซ้อนเกินความจำเป็น ให้ย่อให้เหลือเฉพาะ active state, decision, next step
- ถ้าต้องการสถานะสด ค่อยใช้ tool เช่น `todo`, `session_search`, `cronjob list`

## Workflow เวลาเริ่มบริบทใหม่ / เปลี่ยน model-provider
1. Re-anchor จาก Hermes memory + current chat
2. อ่าน [[START HERE - Hermes Recovery]] ถ้าบริบทไม่พอ
3. อ่าน [[Latest Work Checkpoint]] เพื่อสถานะล่าสุด
4. ถ้าถามงานล่าสุด/ค้างอะไรอยู่ ให้ใช้ `todo` + `session_search` + `cronjob list`
5. ตอบสั้น ๆ ด้วย active state + next step

## Related
- [[START HERE - Hermes Recovery]]
- [[Hermes Identity and Operating Rules]]
- [[Hermes Restore Playbook]]
- [[Recovered Context Summary - 2026-05-27]]
