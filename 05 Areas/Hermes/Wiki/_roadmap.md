---
status: active
tags:
- hermes
- wiki
- roadmap
type: roadmap
created: 2026-06-13
updated: 2026-06-13
---
# Wiki Roadmap

> Roadmap การสร้างและเก็บงาน Hermes LLM Wiki
> Last updated: 13 มิ.ย. 2026

## สถานะตอนนี้
Hermes Wiki ชุดแรกพร้อมใช้งานแล้วในระดับ foundation:
- โครงหลักครบ
- seed page ชุดแรกครบ
- cleanup schema/tags/frontmatter ครบ
- broken wikilinks = 0
- index count ตรงกับไฟล์จริง

## Completed

### Phase 1 — Foundation ✅
- สร้าง `SCHEMA.md`, `index.md`, `log.md`, `_roadmap.md`
- เปิดหมวด `raw`, `entities`, `concepts`, `comparisons`, `queries`
- สร้าง Wiki templates ใน `09 Templates/`
- วาง tag taxonomy และ core file policy

### Phase 2 — Seed Content ✅
- สร้างหน้า seed ที่ใช้งานจริงจาก note เดิมแบบไม่ copy ทั้งหน้า
- ครอบคลุม entity / concept / comparison / query ที่สำคัญของโดเมน Hermes
- ข้ามหน้าที่เสี่ยง sensitive หรือ stale โดยตั้งใจ

### Phase 3 — Cleanup / Alignment ✅
- ทำ schema alignment ให้ tags/frontmatter สอดคล้องกันทั้งชุด
- แก้ core files และ README ให้บทบาทชัด
- ยืนยัน broken wikilinks = 0 และ count ตรงจริง

## Intentional not seeded
- `team-ai-workflow` — มีความเสี่ยง stale และมีรายละเอียด route/profile ควร rewrite ก่อน
- `hermes-sub-agent` — เป็น skill-copy ยาวและมีข้อมูลเชิงระบบที่ควร redact ก่อน
- `Finance Wallet Guard` — มี live endpoint และข้อมูลการเงินส่วนตัว จึงไม่ควรสรุปลง wiki

## Next use pattern
ตอนใช้งานจริง ให้เดินลำดับนี้:
1. อ่าน `index.md`
2. เข้า page ที่เกี่ยว
3. ถ้าข้อมูลใหม่เป็นของดิบ ให้เก็บเข้า `raw/`
4. ถ้ามีค่าระยะยาว ค่อยแตกเป็น wiki page หรือเก็บเข้า `queries/` / `comparisons/`
5. หลังเปลี่ยนแปลงก้อนใหญ่ ให้เช็ก `log.md` และ lint ซ้ำ

## Future expansion (ยังไม่ต้องรีบทำ)
- ขยาย seed pages เพิ่มเมื่อมีคำถามซ้ำหรือมี note สำคัญเพิ่ม
- เพิ่ม query-back loop จากคำตอบแชทที่มีคุณค่าระยะยาว
- พิจารณา lint routine แบบ manual เป็นช่วง ๆ ก่อนคิดเรื่องอัตโนมัติ

## Related
- [[SCHEMA]] — กติกา wiki
- [[index]] — catalog หน้าทั้งหมด
- [[log]] — บันทึกการเปลี่ยนแปลง
