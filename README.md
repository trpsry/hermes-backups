# Hermes Plan Review Pack

ชุด skill สำหรับ Hermes Agent ของพี่จอน

เป้าหมาย:
ให้ AI วางแผนก่อนลงมือจริง แล้วให้ AI อีกตัวช่วยจับผิดก่อนพี่จอนอนุมัติ

## Skills

### hermes-grill-me
ใช้เมื่องานยังไม่ชัด ให้เฮอถาม requirement ก่อน

### hermes-with-docs-review
ใช้กับงานที่ต้องอ้างอิง Obsidian, CONTEXT.md, docs หรือ ADR

### hermes-plan-review
ใช้เมื่อมีแผนอยู่แล้ว ให้ลี่ตรวจแบบจับผิด

## Roles

- พี่จอน = Owner / Approver
- เฮอ = Planner / Manager / Final Reviewer
- ลี่ = Critic / Researcher / Reviewer

## Main Flow

พี่จอนสั่งงาน
→ เฮอวางแผน
→ ลี่ตรวจแผน
→ เฮอแก้แผน
→ พี่จอนอนุมัติ
→ ค่อยลงมือจริง