---
status: active
tags:
  - hermes
  - xurl
  - tool
  - social-media
type: technical
updated: 2026-06-07
---

# xurl Tool Details

## Path
`~/.local/bin/xurl`

## ปัญหาที่เจอ
- npm install fails บน Android/Termux
- แก้ไขโดย patch v1.1.1 ที่ prints OAuth URL แทน

## Magisk Root Usage
ใช้ `su -c` แบบ read-only สำหรับ:
- Android settings
- deviceidle
- appops
- dumpsys

**ข้อควรระวัง:** ไม่แก้ไข system settings โดยไม่ได้รับอนุมัติแบบ staged

## Related
- [[MOC - Hermes]]
- [[Hermes Termux Android Context]]
