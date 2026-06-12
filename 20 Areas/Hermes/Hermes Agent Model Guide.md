---
created: 2026-06-11
status: draft
tags:
- hermes
- model-guide
- reference
type: guide
---
# Hermes Agent Model Guide แบบสั้น

## คำศัพท์
- **Agent** = AI ผู้ช่วยที่มีหน้าที่เฉพาะ
- **Sub-agent** = AI ตัวรอง ช่วยงานย่อย
- **Config** = ไฟล์ตั้งค่าระบบ เช่น config.yaml, .env
- **Debug** = ไล่หาสาเหตุ error
- **Workflow** = ขั้นตอนการทำงาน
- **Multimodal** = อ่านได้หลายแบบ เช่น ข้อความ + รูปภาพ
- **Final Review** = ตรวจรอบสุดท้ายก่อนใช้งานจริง

---

## เลือกโมเดลตามงาน

### GPT-5.5
เหมาะกับ:
- งานยาก
- ตรวจ final
- งานเสี่ยง
- วางแผนระบบ
- เข้าใจ prompt ไม่ชัด

ใช้เป็น:
- หัวหน้า / ตัวตรวจสุดท้าย

---

### GPT-5.4
เหมาะกับ:
- งานทั่วไป
- คุยประจำวัน
- สรุปข้อมูล
- อธิบายภาษาไทยง่าย ๆ

ใช้เป็น:
- ตัวหลักประหยัดกว่า GPT-5.5

---

### GPT-5.4 mini
เหมาะกับ:
- งานย่อย
- Sub-agent
- งานซ้ำ ๆ
- งานเร็ว

ใช้เป็น:
- ลูกมือประหยัด

---

### DeepSeek V4 Pro
เหมาะกับ:
- Hermes config
- setup
- debug
- Android / Termux / root
- GitHub / code / command

ใช้เป็น:
- ตัวเทคนิคหลัก

---

### DeepSeek V4 Flash
เหมาะกับ:
- คุย Telegram
- ตอบเร็ว
- สรุปเบา ๆ
- งานไม่ซับซ้อน

ใช้เป็น:
- ตัวเร็ว / ประหยัด

---

### MiMo V2.5
เหมาะกับ:
- อ่านรูป
- screenshot
- error จากภาพ
- งาน multimodal

ใช้เป็น:
- ตัวอ่านภาพ

---

### MiniMax M3
เหมาะกับ:
- เขียนโพสต์เพจ
- caption
- script
- content
- rewrite ภาษาให้น่าอ่าน

ใช้เป็น:
- ตัวเขียนคอนเทนต์

---

## Setup แนะนำ

### เฮอ / default
ใช้:
- **GPT-5.5** = งานสำคัญ / ตรวจ final
- **GPT-5.4** = ใช้ประจำ
- **DeepSeek V4 Pro** = งาน config/debug

หน้าที่:
- วางแผน
- คุมงาน
- ตรวจงานลี่
- สรุป final
- งานเสี่ยงต้องรอพี่จอนอนุมัติ

---

### ลี่ / secondmodel
ใช้:
- **DeepSeek V4 Pro** = research / ตรวจข้อมูล / งานเทคนิค
- **MiniMax M3** = เขียนโพสต์
- **DeepSeek V4 Flash** = ตอบเร็ว
- **MiMo V2.5** = อ่านรูป

หน้าที่:
- ค้นข้อมูล
- สรุป
- เขียน draft
- จับผิด
- ห้ามตัดสินใจ final แทนเฮอ

---

## สูตรใช้งาน

### งาน config / setup Hermes
DeepSeek V4 Pro วางแผน → GPT-5.5 ตรวจ → พี่จอนอนุมัติ → ค่อยลงมือ

### งานโพสต์เพจ
DeepSeek V4 Pro research → MiniMax M3 เขียน → GPT-5.5 ตรวจ final → พี่จอนอนุมัติ

### งานคุยทั่วไป
GPT-5.4 หรือ GPT-5.5 — ถ้าอยากไว ใช้ DeepSeek V4 Flash

### งานอ่านรูป
MiMo V2.5 — ถ้าเกี่ยวกับ error/config ให้ GPT-5.5 ตรวจซ้ำ

---

## สรุปสั้นสุด

| บทบาท | โมเดล |
|---|---|
| ฉลาดสุด | GPT-5.5 |
| ใช้ทั่วไป | GPT-5.4 |
| งานย่อย | GPT-5.4 mini |
| งาน config/debug | DeepSeek V4 Pro |
| ตอบไว | DeepSeek V4 Flash |
| อ่านรูป | MiMo V2.5 |
| เขียนโพสต์ | MiniMax M3 |
