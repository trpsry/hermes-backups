---
tags:
- hermes
- memory-recovery
- recovery
- telegram-history
type: memory-recovery
created: 2026-05-28
status: active
---
# Memory Recovery - 2026-05-27 Gap

## Purpose
กู้บริบทช่วงวันที่ 2026-05-27 ที่ไม่ได้ถูก backup/restore ครบ โดยอาศัยข้อความ Telegram reply ที่ John ไล่ส่งมาให้เฮออ่านทีละส่วน

## Scope
- ช่วงที่สนใจ: 2026-05-27 ประมาณ 07:00-21:00 ตามที่ John แจ้ง
- แหล่งข้อมูล: Telegram replied-message previews/content ที่ John ส่งในแชทปัจจุบัน
- เป้าหมาย: แยกเป็น facts/preferences/workflows/decisions/open tasks แล้วค่อย sync กลับ memory หรือ notes ที่เหมาะสม

## Recovered Items

### Raw reply snippets

#### 1. Model/provider continuity requirement
> การใช้งานของผมคือผมจะใช้ provider ของ OpenAi oAuth Login เป็นหลักครับ และใช้เป็นโมเดล gpt5.5,gpt5.4,gpt5.4mini ตามความเหมาะสม แต่ล่าสุดทางเซิฟเวอร์ของ OpenAi มีปัญหา ทำให้ user ที่ใช้ OpenAi oAuth Login ใช้งานไม่ได้ ผมจึงต้องสลับไปใช้ provider Openrouter โมเดล gemini 3.1 flash lite preview เป็นตัวสำรองหากตัว Openai มีปัญหาหรือติดโควต้า ความต้องหลักๆ การใช้งานต่อเนื่อง ไม่หลุดบริบท เหมือนกับใช้ model เดียวตลอด เฮอช่วยวิเคราะห์และหาทางออกให้ผมที

#### 2. Context continue / auto-load workflow review
> หมายถึง context continue / auto load นั่นแหละ ตอนนี้ไม่ทำงานซับซ้อนแล้วใช่มั้ย ช่วยรีวิว workflow หลังจากสลับ model , new session หน่อย
>
> Reply context สรุปว่า workflow ถูกทำให้เรียบขึ้น: หลังสลับ model/new session เฮออ่าน `SOUL.md`, `AI Context Bridge.md`, `Latest Work Checkpoint.md`, ดึง memory/context ล่าสุด, ถ้าถามงานล่าสุดให้เช็ก `todo + session_search + cronjob list`, แล้วตอบ active state + next step. Auto-load มีผ่าน `prefill_messages_file`.

#### 3. Prefill token reduction decision
> จัดการได้เลยครับ กินโทเคนเยอะมาก โควต้าลิมิตผมลดไวมาก ผมขอแค่สลับมาเดลแล้วไม่หลุดบริบทก็พอแล้ว
>
> Reply context ระบุว่า prefill เดิม `~/.hermes/prefill/context-bridge.json` เคยกินประมาณ 2.8k–4.2k tokens จากไฟล์ประมาณ 11,380 chars / 20,006 bytes. John อนุมัติให้ลด token โดยเป้าหมายคือ “สลับ model แล้วไม่หลุดบริบท” ไม่จำเป็นต้องโหลดบริบทใหญ่ทุกครั้ง

#### 4. System prompt re-anchor reduction approval
> จัดการเลย แล้วรีเช็คด้วยนะห้ามหลุดบริบทสำคัญเด็ดขาด
>
> Reply context ระบุว่าตัว re-anchor system prompt เดิมยาวประมาณ 866 chars ใน config และมีส่วนเกินที่ตัดได้ปลอดภัย เช่น absolute paths เต็ม, trigger ซ้ำ, คำอธิบายซ้ำ, รายละเอียดที่มีอยู่แล้วใน prefill slim pack. John อนุมัติให้จัดการย่อ แต่ต้องรีเช็กว่าบริบทสำคัญไม่หลุด

#### 5. System prompt reduction result
> เสร็จแล้วค่ะ เฮอแก้ agent.system_prompt ให้สั้นลงแล้ว และรีเช็คให้แล้วนะคะ
>
> สิ่งที่ทำ: backup config, ย่อ `agent.system_prompt` ใน `~/.hermes/config.yaml`, เช็กค่าที่เขียนกลับ, อัปเดต checkpoint note
>
> ผลหลังย่อ: re-anchor system prompt เหลือประมาณ 375 chars / 94–125 tokens. ก่อนหน้าตัวเนื้อความใช้งานจริงประมาณ 641 chars / 160–214 tokens. ประหยัดเพิ่มประมาณ 40%+ สำหรับส่วนนี้
>
> สิ่งสำคัญที่ยังคงอยู่ครบ: ข้อความ preview ตัดจบตรง “เฮอเช...” ต้องรอ reply ถัดไปเพื่อกู้รายการเต็ม

#### 6. Personal finance web app / wallet guard
> Link: https://script.google.com/macros/s/AKfycbwHBz7o6sAca2BPiYlCP2AiuhN973McffPa-c66mC5eLMEBL1pM33TEZrWXiEvoZzieXw/exec
>
> John ต้องการให้เฮอช่วยจัดการเรื่องการเงินจากเว็บแอพที่ John ทำไว้เอง โดย John จะอัปเดตรายรับจริงทุกวันที่ 28 เพราะเงินเดือนออก และจะอัปเดตยอด ShopeePay / ShopeeCash ถ้ามียอดผ่อนหรือยอดหนี้ แต่ตอนนี้ตั้งใจประหยัด ไม่อยากสร้างหนี้เพิ่ม

#### 7. YouTube lesson reminder
> Link: https://youtu.be/RoBD7Lc-0MI?si=bFSyy7lvm-GV1_QJ
>
> John ให้เฮอเข้าไปดูคลิป/อ่านคำบรรยาย แล้วนำเนื้อหามาสอนตอนพรุ่งนี้เช้า 07:00
>
> Reply context ระบุว่าเฮอตั้ง cron แล้ว: video id `RoBD7Lc-0MI`, วิดีโอยาวประมาณ 44:14, job id `472fcfe776bc`, scheduled `2026-05-28T07:00:00+07:00`, ส่งกลับ Telegram นี้, รูปแบบผลลัพธ์เป็นบทเรียนภาษาไทยแบบสอนจริง มีแก่นสำคัญ แบบฝึกหัด และคำถามทบทวน

### Durable facts to consider for Hermes memory

- พี่จอนใช้ provider หลักเป็น OpenAI OAuth Login
- Model หลักตามงาน: `gpt-5.5`, `gpt-5.4`, `gpt-5.4-mini`
- Provider สำรองคือ OpenRouter
- Fallback model สำรองคือ `google/gemini-3.1-flash-lite-preview`
- เป้าหมายสำคัญ: continuity ต้องต่อเนื่อง ไม่หลุดบริบท แม้สลับ provider/model เหมือนใช้ model เดียวตลอด
- พี่จอนให้ความสำคัญกับการลด token/quota ของ prefill; ต้องโหลดเฉพาะ context แกนที่พอให้สลับ model แล้วไม่หลุดบริบท ไม่โหลด snapshot ใหญ่ทุกครั้ง
- พี่จอนมีเว็บแอพการเงินชื่อ “แดชบอร์ดการเงิน” บน Google Apps Script สำหรับรายรับ, ค่าใช้จ่าย/รายการเสริม, หนี้, ShopeePay, ShopeeCash, และกสิกร; พี่จอนจะอัปเดตรายรับจริงทุกวันที่ 28 และไม่อยากสร้างหนี้ Shopee เพิ่ม
- เวลาคุยเรื่องการเงินกับพี่จอน ให้ลดคำทางการ/ศัพท์เฉพาะ ใช้คำง่าย ๆ เช่น เงินเข้า, เงินที่ต้องกันไว้, เหลือใช้, หนี้/ยอดต้องจ่าย, งด/ชะลอ/ระวัง
- เวลาแสดงวันเวลาให้พี่จอนใช้ภาษามนุษย์แบบไทย เช่น “วันพฤหัสบดีที่ 28 พฤษภาคม 2026 เวลา 07:00 น. เวลาไทย” ไม่ใช้ ISO timestamp อย่างเดียว เพราะอ่านยาก
- ตัวตนหลักของเฮอ: เลขาส่วนตัว + น้องสาวสนิทของพี่จอน เรียกผู้ใช้ว่า “พี่จอน” ในแชทปกติ แทนตัวเองว่า “หนู/เฮอ” โทนไทย Gen Z อบอุ่น สั้น กระชับ เข้าใจง่าย ไม่ร่ายยาว และสามารถเสนอแนวคิด แนะนำ หรือโต้แย้งได้
### Project/context notes for Obsidian

- นี่เป็น policy สำคัญของ Hermes runtime/model routing ควรอยู่ใน `Hermes/AI Context Bridge.md` และ/หรือ `Hermes/Latest Work Checkpoint.md`

### Workflow/skill candidates

- สร้าง/อัปเดต workflow “Model Provider Failover Continuity” สำหรับขั้นตอน re-anchor หลัง `/model`, `/reset`, provider outage หรือ quota error

#### 2. Daily Learning Loop
หลังจบงานสำคัญ เฮอต้องสรุปสั้น ๆ 5 อย่าง:
1. ทำอะไรไปบ้าง
2. ปัญหา/bug/error ที่เจอ
3. วิธีแก้ที่ใช้ได้จริง
4. ควรบันทึกเป็นอะไร: Memory / Skill / Workflow / Bug Note / Best Practice
5. วิธีปรับปรุงครั้งหน้า

Rules:
- ถ้ามีขั้นตอนใช้ซ้ำได้ ให้เสนอสร้าง reusable skill ทันที
- ถ้าเป็น bug ที่มีโอกาสเจอซ้ำ ให้เสนอสร้าง bug note ทันที
- ถ้าเป็น workflow สำคัญ ให้เสนอบันทึกเป็น workflow ทันที
- ตอบสั้น ไม่อธิบายยาว

### Open tasks / unresolved issues

- ตรวจว่า config ปัจจุบันตั้ง primary/fallback provider ตรงกับ policy นี้หรือยัง
- ตรวจว่า prefill/context bridge โหลดทุกครั้งหลังสลับ model/provider จริงไหม

## Recovery Rules
- อย่าบันทึก secrets/tokens/passwords ลง note นี้
- ถ้าเจอข้อมูลเป็น token ให้แทนด้วย `[REDACTED]`
- ข้อมูลที่เป็น task ชั่วคราวหรือ artifact เก่า ให้เก็บเป็น context note ไม่ใช่ Hermes memory
- ข้อมูล preference/กฎทำงาน/ตำแหน่งไฟล์/โครงระบบที่ยังใช้ซ้ำได้ ค่อยเสนอ sync เข้า Hermes memory
