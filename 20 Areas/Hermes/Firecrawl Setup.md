---
created: 2026-06-11
status: draft
tags:
- firecrawl
- hermes
- setup
- tools
- web-search
type: setup-guide
---
# Firecrawl Setup Guide for Hermes Agent

## ใช้ทำอะไร
Firecrawl คือเครื่องมือให้ Hermes ค้นเว็บและอ่านเว็บได้ดีขึ้น

ใช้กับงานแบบนี้:
- ค้นข่าว AI ล่าสุด
- อ่าน GitHub issue / docs / blog
- สรุปหน้าเว็บยาว ๆ
- ดึงข้อมูลเว็บเป็น markdown ให้ AI อ่านง่าย
- ใช้เป็นแหล่งข้อมูลก่อนบันทึกลง Obsidian

---

## Firecrawl ทำอะไรได้
หลัก ๆ มี 3 แบบ:
- **Search** = ค้นเว็บ
- **Scrape** = อ่านหน้าเว็บเดียว
- **Crawl** = ไล่อ่านหลายหน้าภายในเว็บ

สำหรับ Hermes ใช้หลัก ๆ คือ:
- `web_search` = ค้นเว็บ
- `web_extract` = ดึงเนื้อหาจาก URL

---

## ไฟล์ที่เกี่ยวข้อง
- `~/.hermes/.env`
- `~/.hermes/config.yaml`

> ⚠️ ห้ามบันทึก API key ลง Obsidian
> ถ้าต้องจด ให้เขียนแค่: `FIRECRAWL_API_KEY=[REDACTED]`

---

## วิธีตั้งค่าแบบแนะนำ

### 1. ใส่ API key ใน `.env`
```
FIRECRAWL_API_KEY=fc-your-key-here
```
> ห้ามแชร์ key หรือส่งเข้าแชทแบบเต็ม

### 2. ตั้ง backend ใน config
```yaml
web:
  backend: firecrawl
```
หรือใช้คำสั่ง:
```bash
hermes tools
```
แล้วเลือก Web Search & Extract → Firecrawl

---

## วิธีทดสอบ
ให้ Hermes ทดสอบแบบนี้:

**ค้นเว็บด้วย Firecrawl:**
> ค้นเว็บด้วย Firecrawl ว่า Hermes Agent Firecrawl ใช้งานยังไงจาก official docs แล้วสรุปพร้อมแหล่งอ้างอิง

**ทดสอบ extract:**
> อ่าน URL official docs ของ Hermes Web Search แล้วสรุปเฉพาะส่วน Firecrawl

---

## Model ที่ควรใช้

| งาน | โมเดล |
|---|---|
| ตั้งค่า / debug / ตรวจ config / งานเสี่ยง | **GPT-5.5 Thinking** |
| ค้นเว็บและสรุปข้อมูลทั่วไป | **GPT-5.5** หรือ **GPT-5.4** |
| ช่วย debug command/config | **DeepSeek V4 Pro** (ควรให้ GPT-5.5 ตรวจ final) |
| เขียนโพสต์หลังจากได้ข้อมูลแล้ว | **MiniMax M3** |

> ไม่แนะนำให้ใช้โมเดลเล็กเป็นตัวหลักตอนตั้งค่า เพราะอาจเรียก tool ผิดหรือสรุปผิดจากเว็บ

---

## Workflow แนะนำ
1. Firecrawl ค้นเว็บ
2. Hermes อ่านและสรุป
3. GPT-5.5 ตรวจความถูกต้อง
4. บันทึกผลลง Obsidian
5. เพิ่มลิงก์ใน MOC ถ้าเป็น note สำคัญ

**ตัวอย่าง:**
> หา docs ล่าสุดของ Hermes Firecrawl → สรุปเป็น note → บันทึกที่ `20 Areas/Hermes/Firecrawl Setup.md` → เพิ่มลิงก์ใน MOC - Hermes

---

## ข้อดี
- Hermes ค้นเว็บสดได้
- อ่านเว็บยาว ๆ ได้ง่ายขึ้น
- เหมาะกับ research / docs / GitHub issue
- เอาข้อมูลไปเก็บใน Obsidian ต่อได้ดี

---

## ข้อเสีย
- ต้องใช้ API key
- อาจมี quota / credit จำกัด
- ถ้าค้นเว็บผิดแหล่ง Hermes อาจสรุปผิดตาม
- เว็บบางเว็บอาจ scrape ไม่ได้หรือข้อมูลไม่ครบ

---

## ความเสี่ยง
- ห้ามเก็บ API key ใน Obsidian
- ห้ามให้ Hermes แสดง secret เต็ม
- ต้องเช็กแหล่งอ้างอิงก่อนใช้ข้อมูลจริง
- ถ้าใช้กับงานเสียเงิน/API paid ต้องให้พี่จอนอนุมัติก่อน

---

## Prompt ใช้สั่ง Hermes

ใช้ **GPT-5.5 Thinking** ตรวจและตั้งค่า Firecrawl ให้ Hermes

**เป้าหมาย:**
- ตรวจว่า `FIRECRAWL_API_KEY` มีใน `~/.hermes/.env` หรือไม่ โดยห้ามแสดง key เต็ม
- ตั้ง web backend เป็น firecrawl
- ทดสอบ `web_search` และ `web_extract`
- สรุปผลว่าใช้งานได้จริงไหม

**กฎ:**
- ห้ามแสดง API key/token/password
- ห้ามแก้ config สำคัญโดยไม่ backup
- ห้ามใช้ paid API เพิ่มเติมถ้าไม่ได้รับอนุมัติ
- รายงานไฟล์ที่แตะและผลทดสอบกลับมา
