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

## OpenCode Go — ตัวเลือกเด่นสำหรับลี่

> อ้างอิงจากหน้า OpenCode Go / docs providers ณ 13 มิ.ย. 2026
> รายชื่อใน Go เปลี่ยนได้ในอนาคต ถ้าจะใช้จริงให้เช็ก `/models` อีกครั้ง
> ตอนนี้ **Claude / Sonnet 4 ไม่มีใน OpenCode Go**

### DeepSeek V4 Pro
เหมาะกับ:
- งานเทคนิคหลัก
- config / setup / debug
- อ่าน code / command
- งานที่ต้องคุมคุณภาพคำตอบ

ใช้เป็น:
- ตัวหลักของลี่เวลางานมีความเสี่ยงทางเทคนิค แต่ยังไม่ถึงขั้นให้เฮอลงมือเองทั้งหมด

ข้อสังเกต:
- จาก OpenCode Go docs เป็นตัวกลาง ๆ ที่คุ้ม และ DeepSeek ระบุว่าเด่นด้าน agentic coding, reasoning, world knowledge

---

### DeepSeek V4 Flash
เหมาะกับ:
- งานเร็ว
- สรุปเบื้องต้น
- scan note / scan file จำนวนมาก
- งานย่อยที่ต้องประหยัด

ใช้เป็น:
- ตัวกวาดงาน / รอบแรก / งานไม่ซับซ้อน

ข้อสังเกต:
- เร็วและถูกมาก แต่ไม่ควรใช้เป็นตัว final สำหรับงานคิดยาก

---

### Kimi K2.7 Code
เหมาะกับ:
- งานโค้ดยาวหลายไฟล์
- งานที่ต้องคิดหลาย step
- งานที่มี tool call หลายรอบ
- repo-level coding

ใช้เป็น:
- ตัวเลือกพิเศษของลี่สำหรับงาน code-heavy กว่าปกติ

ข้อสังเกต:
- Kimi ระบุชัดว่าเด่นเรื่อง long-horizon coding, 256K context, multi-step tool use

---

### MiMo V2.5
เหมาะกับ:
- งานที่มีรูป / screenshot / chart
- งาน multimodal
- งาน agentic ทั่วไป
- coding ที่ต้องดูภาพหรือ UI ประกอบ

ใช้เป็น:
- ตัวอ่านภาพ + ทำงานต่อได้ในตัวเดียว

ข้อสังเกต:
- Xiaomi ระบุว่าเด่นด้าน agency, visual/audio understanding, long context และ coding คุ้มราคา

---

### MiniMax M3
เหมาะกับ:
- งาน context ยาวมาก
- งาน coding + content ปนกัน
- งานที่อาจมีรูป / video / desktop context
- rewrite ภาษาพร้อม reasoning

ใช้เป็น:
- ตัวอเนกประสงค์เมื่ออยากได้ทั้ง coding + multimodal + long context

ข้อสังเกต:
- MiniMax ระบุว่า M3 เด่นด้าน coding, agentic work, 1M context, native multimodal

---

### Qwen3.7 Max
เหมาะกับ:
- งาน reasoning หนัก
- งาน context ยาวมาก
- งานวิเคราะห์ยาก
- งาน agentic ที่ซับซ้อน

ใช้เป็น:
- ตัวหนักพิเศษของลี่ เฉพาะตอน DeepSeek V4 Pro ยังไม่พอ

ข้อสังเกต:
- Qwen3.7 Max เด่นด้าน reasoning/agent + 1M context
- **เปลือง token มาก** ควรใช้เฉพาะตอนจำเป็นจริง

---

### Qwen3.7 Plus
เหมาะกับ:
- งานสมดุลระหว่าง reasoning กับ cost
- งานที่ต้องมี vision / multimodal
- งานที่ไม่หนักถึง Max

ใช้เป็น:
- ตัวกลางระหว่าง DeepSeek V4 Pro กับ Qwen3.7 Max

ข้อสังเกต:
- จากข้อมูลที่หาได้ Plus เป็นสาย balanced / multimodal มากกว่า Max

---

## สูตรเลือกโมเดลก่อนเริ่มงาน

### เฮอ / default
- **GPT-5.5** = คุมแผน / final review / งานเสี่ยง / decision สำคัญ
- **GPT-5.4** = งานทั่วไปที่ไม่หนักมาก

### ลี่ / secondmodel
- **DeepSeek V4 Pro** = ค่าเริ่มต้นสำหรับงานเทคนิค
- **DeepSeek V4 Flash** = งานเร็ว / งานกวาด / งานประหยัด
- **Kimi K2.7 Code** = งานโค้ดยาวหลายไฟล์ / tool call หลายรอบ
- **MiMo V2.5** = งานรูป / screenshot / multimodal
- **MiniMax M3** = งาน long context + coding/content
- **Qwen3.7 Max** = งาน reasoning หนักมากเท่านั้น
- **Qwen3.7 Plus** = งาน balanced ที่อยากได้มากกว่า DeepSeek แต่ไม่อยากไปถึง Max

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
