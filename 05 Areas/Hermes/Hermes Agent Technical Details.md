---
status: active
tags:
  - hermes
  - agent
  - technical
type: technical
updated: 2026-06-07
---

# Hermes Agent Technical Details

## File Naming Convention
Hermes Agent ใช้ **uppercase filenames** สำหรับ core files:
- `SOUL.md`
- `AGENTS.md`
- `MEMORY.md`
- `USER.md`

**สำคัญ:** ค้นหาด้วย case-insensitive patterns
```bash
find -iname "*soul*"
find -iname "*memory*"
```

หรือเช็คทั้ง 2 case variants เมื่อหา system files ใน `~/.hermes/`

## Precision Work Policy
เมื่อทำงานเกี่ยวกับ:
- Config
- System files
- Source code

**ใช้วิธีที่ถูกต้องตั้งแต่แรก** แทนที่จะลองหลายๆ รอบกับ cheap model

**เหตุผล:** ประหยัดเงินมากกว่าการใช้ cheap model + หลาย retries

## Related
- [[MOC - Hermes]]
- [[Hermes Termux Android Context]]
