---
aliases:
- Hermes Knowledge Entry Map
confidence: medium
created: 2026-06-13
sources:
- '[[MOC - Hermes]]'
- '[[START HERE - Hermes Recovery]]'
- '[[Latest Work Checkpoint]]'
- '[[05 Areas/Hermes/Wiki/raw/articles/karpathy-llm-wiki-pattern|Karpathy LLM Wiki Pattern — source notes]]'
status: active
tags:
- hermes
- wiki
- query
- agent
- workflow
- memory
- seed
updated: 2026-06-13
wiki_type: query
---
# Hermes Knowledge Entry Map

## คำถามที่หน้านี้ตอบ
ถ้าเฮอหรือ agent ตัวอื่นต้องเริ่มจับบริบท Hermes ควรเริ่มอ่านจากไหนก่อน เพื่อไม่โหลดทั้ง vault มั่ว ๆ

## ลำดับอ่านแบบประหยัด context
1. ใช้ memory + current chat ก่อน
2. ถ้ายังบาง อ่าน [[START HERE - Hermes Recovery]]
3. อ่าน [[Hermes Identity and Operating Rules]] เฉพาะเมื่อเกี่ยวกับตัวตน/กฎหลัก
4. อ่าน [[AI Context Bridge]] เมื่อมี model/provider switch หรือ context บาง
5. อ่าน [[Latest Work Checkpoint]] เมื่อถามงานล่าสุด
6. ใช้ [[MOC - Hermes]] เป็นแผนที่รวม ถ้าต้องหา note เฉพาะทาง

## แผนที่พื้นที่
- Core/recovery: START HERE, Identity, AI Context Bridge, Latest Work Checkpoint
- Operating: Hermes Operating Notes, Daily Learning Loop
- Technical: Hermes Termux Android Context, Hermes Agent Technical Details, xurl Tool Details
- Model/workflow: Hermes Agent Model Guide; `team-ai-workflow` และ `hermes-sub-agent` ยังไม่ seed เข้า wiki เพราะต้อง rewrite/redact ก่อน
- Archive: recovery summaries เก่า อ่านเฉพาะเมื่อจำเป็นจริง

## วิธีใช้กับ LLM Wiki
- ถ้าคำถามเป็นความรู้ยาว ให้ค้น `index.md` ของ wiki ก่อน
- ถ้าคำตอบใหม่มีค่าระยะยาว ให้เก็บเข้า `queries/` หรือ `comparisons/`
- ถ้าเป็นแค่คำตอบชั่วคราว ไม่ต้องเก็บ

## Related
- [[05 Areas/Hermes/Wiki/comparisons/hermes-agent-model-routing|Hermes Agent Model Routing]]
- [[05 Areas/Hermes/Wiki/entities/hermes-termux-android-backend|Hermes Termux Android Backend]]
- [[05 Areas/Hermes/Wiki/concepts/hermes-operating-layer|Hermes Operating Layer]]
