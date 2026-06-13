---
status: active
tags:
- hermes
- wiki
- schema
type: schema
---
# SCHEMA

## เป้าหมาย
Wiki นี้เอาไว้เก็บความรู้ระยะยาวของเฮอแบบเชื่อมโยงกันได้ และโตต่อได้

## เส้นแบ่ง
- Memory = preference / fact สั้น ๆ
- Skills = ขั้นตอนทำงานที่ใช้ซ้ำ
- Session = ประวัติแชท / งานชั่วคราว
- Wiki = ความรู้เชิงลึกที่ควรค้นซ้ำได้
- Canonical notes = recovery / identity / operating rules / note หลัก ห้ามเขียนทับ

## หมวดหลัก
- `raw/` = source note หรือสรุปต้นทางแบบยังไม่กลั่นมาก
- `entities/` = คน ระบบ เครื่องมือ โปรไฟล์ หรือองค์ประกอบที่เป็นตัวตนชัด
- `concepts/` = แนวคิด วิธีคิด กฎ ตรรกะ workflow
- `comparisons/` = เทียบ A vs B / ทางเลือก / tradeoff
- `queries/` = คำตอบเชิงวิเคราะห์ที่ควรเก็บกลับมาใช้

## กติกาเขียนหน้าใหม่
1. ห้าม copy note เดิมทั้งหน้า
2. ให้สรุปใหม่แบบสั้น แล้วใส่ `sources:` กลับไปหา note ต้นทาง
3. ถ้าเกี่ยวกับ recovery / identity / operating rules ให้ลิงก์หา note หลัก แทนการเขียนทับ
4. ถ้าไม่ชัดว่าควรเก็บไหม ให้ยังไม่สร้าง
5. หน้าใหม่ควรอ่านรู้เรื่องในตัวเอง แต่ไม่ยาวเกินจำเป็น

## แม่แบบ frontmatter ขั้นต่ำ
```yaml
status: draft|active
wiki_type: raw|entity|concept|comparison|query
sources:
  - [[Note Name]]
updated:
```

## ห้ามทำในเฟสแรก
- ห้ามย้าย note เดิม
- ห้าม rename note เดิม
- ห้ามแตะ canonical notes โดยตรง
- ห้าม ingest ทั้ง vault
- ห้ามตั้ง cron อัตโนมัติ
