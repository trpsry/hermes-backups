---
aliases:
- Hermes Agent Model Routing
confidence: high
created: 2026-06-13
sources:
- '[[Hermes Agent Model Guide]]'
- '[[05 Areas/Hermes/Wiki/raw/articles/karpathy-llm-wiki-pattern|Karpathy LLM Wiki Pattern — source notes]]'
status: active
tags:
- hermes
- wiki
- comparison
- agent
- model
- routing
- workflow
- multi-agent
- stable
updated: 2026-06-13
wiki_type: comparison
---
# Hermes Agent Model Routing

## ใช้หน้านี้ทำอะไร
ใช้เป็นหน้าตัดสินใจเร็วว่า งานแบบไหนควรให้โมเดลไหนทำ และใครควรตรวจ final

## หลักคิด
- งานเสี่ยง / งาน final / งานต้องตัดสินใจ → ใช้เฮอเป็นคนคุม
- งานค้นข้อมูล / audit / draft → ให้ลี่ช่วยได้ แต่เฮอต้องตรวจ
- งาน config, root, Android system, token, cronjob, GitHub automation → เฮอคุมเอง และต้องขออนุมัติก่อนถ้าแตะระบบจริง

## Routing สั้น ๆ
- GPT-5.5: final review, plan ยาก, งานเสี่ยง, ตรวจงานลี่
- GPT-5.4: งานทั่วไป, สรุป, คุยประจำวัน
- DeepSeek V4 Pro: audit, technical research, setup/debug analysis
- DeepSeek V4 Flash: งานเบา/เร็ว/ประหยัด
- MiMo V2.5: งานมีภาพหรือ screenshot
- MiniMax M3: งานเขียนโพสต์/caption/script
- Qwen3.7 Max: งาน reasoning หนักมากเท่านั้น เพราะกิน token มาก

## Workflow แนะนำ
1. เฮอรับโจทย์และตัดสินใจว่าเสี่ยงไหม
2. ถ้าเป็น research/draft/audit ให้ส่งลี่ผ่าน Kanban
3. ลี่ส่งผลกลับมา
4. เฮอตรวจความถูกต้อง ความเสี่ยง และโทน
5. เฮอสรุป final ให้พี่จอน

## LLM Wiki note
หน้านี้เป็น comparison page ตามแนว [[05 Areas/Hermes/Wiki/raw/articles/karpathy-llm-wiki-pattern|Karpathy LLM Wiki Pattern — source notes]] เพราะเป็นความรู้ที่ต้องใช้ตัดสินใจซ้ำ และควรเก็บไว้ใน wiki แทนปล่อยหายในแชท

## Related
- [[05 Areas/Hermes/Wiki/entities/hermes-termux-android-backend|Hermes Termux Android Backend]]
- [[05 Areas/Hermes/Wiki/concepts/hermes-operating-layer|Hermes Operating Layer]]
- [[05 Areas/Hermes/Wiki/queries/hermes-knowledge-entry-map|Hermes Knowledge Entry Map]]
