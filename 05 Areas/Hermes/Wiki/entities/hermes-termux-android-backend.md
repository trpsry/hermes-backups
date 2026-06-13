---
status: active
wiki_type: entity
created: 2026-06-13
updated: 2026-06-13
aliases:
  - Hermes Termux Android Backend
sources:
  - [[Hermes Termux Android Context]]
  - [[Hermes Agent Technical Details]]
  - [[xurl Tool Details]]
confidence: medium
---
# Hermes Termux Android Backend

## คืออะไร
Android/Termux คือเครื่อง backend ที่รัน Hermes Agent ให้พี่จอน ส่วน iPhone 13 Pro คือเครื่องหลักที่พี่จอนใช้งานประจำวัน

## สิ่งสำคัญที่ต้องจำ
- Hermes จริงรันบน Android/Termux
- path หลักของ Hermes อยู่ใต้ `/data/data/com.termux/files/home`
- Obsidian vault อยู่ใน shared storage: `/storage/emulated/0/Documents/Hermes Vault`
- งานระดับ Android/root/system ต้องขออนุมัติก่อนเสมอ

## Termux quirks
- ไม่มี `/usr/bin/` แบบ Linux ปกติ
- script ควรใช้ path ของ Termux โดยตรงเมื่อจำเป็น
- งาน precision เช่น config/system/source code ควรใช้วิธีที่ถูกตั้งแต่แรก ไม่ลองผิดลองถูกหลายรอบ

## Gateway / dashboard
- Hermes gateway ใช้ wrapper/tmux ใน Android backend
- Dashboard ใช้จาก Android backend เป็นหลัก แล้วอุปกรณ์อื่นค่อยเปิดดูผ่าน network/bridge

## Tools ที่เกี่ยว
- `xurl` อยู่ที่ `~/.local/bin/xurl`
- บน Android บาง npm package ใช้ไม่ได้ ต้องใช้ patched/local build

## ข้อควรระวัง
- หน้านี้ไม่เก็บคำสั่ง root แบบละเอียดเพื่อกันการเผลอทำผิด
- ถ้าต้องแตะ Android settings, root, service, boot, network, firewall หรือ system process ต้องอธิบายข้อดี/ข้อเสีย/ความเสี่ยง และรอพี่จอนอนุมัติก่อน

## Related
- [[05 Areas/Hermes/Wiki/comparisons/hermes-agent-model-routing|Hermes Agent Model Routing]]
- [[05 Areas/Hermes/Wiki/concepts/hermes-operating-layer|Hermes Operating Layer]]
- [[05 Areas/Hermes/Wiki/queries/hermes-knowledge-entry-map|Hermes Knowledge Entry Map]]
