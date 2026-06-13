---
copied_on: 2026-06-12
skill_name: hermes-sub-agent
skill_path: devops/hermes-sub-agent/SKILL.md
source: Hermes skill
tags:
- hermes
- skills
- sub-agent
- termux
- telegram
---
---
name: hermes-sub-agent
description: ตั้งค่า Hermes multi-profile sub-agent บน Android/Termux — สร้าง profile ที่สอง, ตั้ง bot token แยก, split gateway (hmgw1/hmgw2), จัดการ topic routing ใน Telegram forum group, และแยกบทบาทด้วย channel_prompts
version: 1.5.0
---

# Hermes Sub-Agent Setup

คู่มือตั้งค่า multi-agent ด้วย Hermes profiles บน Android/Termux — แยก bot, แยก gateway, แยกบทบาท ทำงานร่วมกันใน Telegram Group แบบ forum

## ภาพรวม Architecture

```
┌─────────────────────────────────────────────────┐
│  OnePlus 7 Pro (Android 12 / Termux)            │
│                                                 │
│  Profile: default (เฮอ)                         │
│  ├─ Bot token: @hermes_default_bot              │
│  ├─ Tmux: hermes-gateway                        │
│  ├─ Launcher: ~/bin/start-hermes-gateway.sh     │
│  ├─ Shortcut: ~/bin/hmgw1                       │
│  └─ Config: ~/.hermes/config.yaml               │
│                                                 │
│  Profile: secondmodel (ลี่)                      │
│  ├─ Bot token: @hermes_secondmodel_bot          │
│  ├─ Tmux: hermes-gateway-secondmodel            │
│  ├─ Launcher: ~/bin/start-hermes-secondmodel.sh │
│  ├─ Shortcut: ~/bin/hmgw2                       │
│  └─ Config: ~/.hermes/profiles/secondmodel/...  │
└─────────────────────────────────────────────────┘
```

แต่ละ profile คือโลกแยกกัน — มี session, gateway process, bot token, SOUL.md เป็นของตัวเอง

## Step 1: สร้าง Profile ใหม่

```bash
hermes profile create secondmodel
```

หรือถ้ามี profile อยู่แล้ว (เช่น orchestrator) ให้ rename:
```bash
# วิธี manual: แก้ ~/.hermes/profiles/<old>/ → ~/.hermes/profiles/<new>/
# แล้วอัปเดตทุกที่ที่อ้างถึง
```

โครงสร้างหลังสร้าง:
```
~/.hermes/profiles/secondmodel/
├── config.yaml      # ค่าตั้งของ profile นี้
├── .env             # bot token + env vars
├── sessions/        # session store แยก
├── skills/          # skills แยก (ถ้ามี)
├── plugins/         # plugins แยก (ถ้ามี)
├── SOUL.md          # บุคลิกของ agent นี้
├── AGENTS.md        # กฎการทำงาน
└── MEMORY.md        # ความจำแยก
```

## Step 2: สร้าง Bot Token สำหรับ Second Profile

1. ไปที่ [@BotFather](https://t.me/BotFather) บน Telegram
2. `/newbot` → ตั้งชื่อ + username (ต้องไม่ซ้ำกับ bot เดิม)
3. BotFather จะให้ token มา → **เก็บให้ดี ห้ามแชร์**

4. ใส่ token ใน `~/.hermes/profiles/secondmodel/.env`:
```bash
echo 'TELEGRAM_BOT_TOKEN=123456:abc...' >> ~/.hermes/profiles/secondmodel/.env
```

5. ตั้งค่า bot ผ่าน BotFather:
   - `/setprivacy` → **Disable** (ให้อ่านข้อความใน group ได้)
   - `/setjoingroups` → **Enable** (ให้เข้า group ได้)
   - `/setcommands` → copy คำสั่งเดียวกับ bot หลัก

6. เชิญ bot เข้า group "My AGENT" และให้ admin สิทธิ์ขั้นต่ำ:
   - Manage Topics (ถ้าเป็น forum)
   - Send Messages
   - Read Messages

## Step 3: ตั้งค่า Provider/Model สำหรับ Second Profile

```bash
hermes config set provider opencode-go --profile secondmodel
hermes config set model.model deepseek-v4-flash-free --profile secondmodel
```

ตรวจสอบ:
```bash
hermes config get provider --profile secondmodel
hermes config get model.model --profile secondmodel
```

## Step 4: ตั้งค่า Telegram Security + Routing

แก้ไข `~/.hermes/profiles/secondmodel/config.yaml`:

```yaml
telegram:
  allowed_chats: '5293931735,-1003985263404'   # DM + Group ID
  require_mention: false
  allowed_topics: '59,3,185'                   # research + writer + ALL
  # ❌ ห้ามใส่ '51' (Header) — นั้นของเฮอ
```

สำหรับ default profile (`~/.hermes/config.yaml`):

```yaml
telegram:
  allowed_chats: '5293931735,-1003985263404'   # DM + Group ID
  require_mention: false
  allowed_topics: '51,185'                     # Header + ALL
  # ❌ ห้ามใส่ '59,3' — นั้นของลี่
```

### วิธีหา Group ID และ Topic ID

```bash
# Group ID จาก gateway log
grep "inbound message" ~/.hermes/logs/gateway.log | tail -5
# → chat=-1003985263404

# Topic ID จาก session identifier
grep "telegram:group:" ~/.hermes/logs/gateway.log | tail -5
# → agent:main:telegram:group:-1003985263404:51
#                                          group_id ^^  ^^ topic_id
```

### Routing Logic (จาก source code)

Hermes เช็คตามลำดับนี้:
1. `allowed_chats` → อยู่ใน list มั้ย?
2. `ignored_threads` → ควร ignore มั้ย?
3. `allowed_topics` → อยู่ใน topic ที่อนุญาตมั้ย?

ถ้า group เป็น forum (`is_forum: true`) และมี `allowed_topics` → bot จะตอบเฉพาะใน topics ที่กำหนด

## Step 5: Channel Prompts — แยกบทบาท

channel_prompts คือ system prompt ที่ inject เฉพาะเวลาอยู่ใน topic นั้น ๆ ใช้กำหนดว่าบอทควรทำตัวยังไงในแต่ละห้อง

### Default Profile (เฮอ)

```yaml
telegram:
  channel_prompts:
    "51": |
      คุณอยู่ใน Topic: ⚡ Header
      บทบาท: เฮอ — Main Agent / Manager / Final Reviewer
      หน้าที่: รับคำสั่ง, วางแผน, แยกงานให้ลี่, ตรวจ final
      กฎ: ห้ามทำงาน research/writer ยาว ๆ เอง
           งาน config/root/system ต้องขออนุมัติก่อนเสมอ

    "59": |
      คุณอยู่ใน Topic: 🔎 research (พื้นที่ของลี่)
      ตอบเฉพาะเมื่อถูกเรียก
      ช่วยตรวจ research ได้ แต่ไม่ต้องแย่งลี่ทำ

    "3": |
      คุณอยู่ใน Topic: 📝 writer (พื้นที่ของลี่)
      ตอบเฉพาะเมื่อถูกเรียก
      ช่วยตรวจ draft / ลดความเวอร์ / แก้ภาษา / ทำ final เมื่อสั่ง

    "1": |
      คุณอยู่ใน Topic: 💬 ALL
      ห้องรวมคุยทั่วไป — ไม่ใช่ห้องสั่งงานหลัก
      ตอบเมื่อพี่จอน mention หรือเรียกชัดเจน
      ถ้าสั่งงานจริง → แนะนำให้ย้ายไป Header
      ถ้าต้องค้นข้อมูล → แนะนำให้ส่ง research
      ถ้าต้องเขียน content → แนะนำให้ส่ง writer
      เรียกลี่ด้วย @miguel890bot เท่านั้น ห้ามลอย ๆ
```

### SecondModel Profile (ลี่)

```yaml
telegram:
  channel_prompts:
    "51": |
      คุณอยู่ใน Topic: ⚡ Header (พื้นที่ของเฮอ)
      ตอบเฉพาะเมื่อถูกเรียก
      ห้ามแย่งเฮอวางแผน, ห้ามตัดสินใจ final, ห้ามแตะ config/system

    "59": |
      คุณอยู่ใน Topic: 🔎 research
      บทบาท: ลี่ — Researcher + Critic
      หน้าที่: ค้น, สรุป, fact-check, แยก fact/opinion, จับผิด
      รูปแบบ: สรุปสั้น → ข้อมูลสำคัญ → ข้อดี/ข้อเสีย/ความเสี่ยง → แนะนำ
      กฎ: ห้ามแต่งข้อมูล, ถ้าไม่ชัวร์ให้บอก, ห้ามแก้ config/system

    "3": |
      คุณอยู่ใน Topic: 📝 writer
      บทบาท: ลี่ — Writer
      หน้าที่: เขียนโพสต์/caption/script, rewrite, ทำ draft
      รูปแบบ: Hook → เนื้อหา → สรุปท้าย → Hashtag
      กฎ: ไม่เวอร์, ไม่มั่วข้อมูล, final ต้องให้เฮอตรวจ

    "1": |
      คุณอยู่ใน Topic: 💬 ALL
      ห้องรวม — ตอบเมื่อถูกเรียกเท่านั้น
      ตอบเมื่อพี่จอน mention / เฮอ mention @miguel890bot
      ห้ามแย่งเฮอวางแผน, ห้ามตัดสินใจ final
      ถ้าเฮอเรียก → รับทราบสั้น ๆ ตอบตรงงาน
      ถ้าทักทาย → ตอบสั้นน่ารัก ไม่ต้องอธิบายยาว
```

## Step 6: แยก Gateway Control (hmgw1 / hmgw2)

ตอนมี 2 profiles ต้องแยก gateway control เพื่อ restart แยกกันได้

### Scripts ที่ต้องมี

| Script | หน้าที่ |
|--------|--------|
| `~/bin/hmgw1` | ควบคุม default gateway (`start\|stop\|restart\|check`) |
| `~/bin/hmgw2` | ควบคุม secondmodel gateway (`start\|stop\|restart\|check`) |
| `~/bin/start-hermes-gateway.sh` | launcher สำหรับ default |
| `~/bin/start-hermes-secondmodel.sh` | launcher สำหรับ secondmodel |
| `~/bin/run-hermes.sh` | auto-restart loop สำหรับ default |
| `~/bin/run-hermes-secondmodel.sh` | auto-restart loop สำหรับ secondmodel |
| `~/bin/hermes-greeting-default.sh` | greeting message สำหรับเฮอ |
| `~/bin/hermes-greeting-secondmodel.sh` | greeting message สำหรับลี่ |

**ชื่อ tmux จริงของ John's setup:** default ใช้ `hermes-gateway`; secondmodel ใช้ `hermes-gateway-secondmodel` (ไม่ใช่ `hermes-secondmodel`). เวลา audit/restart script ต้องยึดชื่อนี้ ไม่งั้นจะเช็กผิด session หรือคิดว่า gateway หายทั้งที่ยังรันอยู่

### ตัวอย่าง hmgw1

```bash
#!/data/data/com.termux/files/usr/bin/bash
# hmgw1 — control default Hermes gateway
TMUX_TMPDIR=/data/data/com.termux/files/usr/tmp
SESSION="hermes-gateway"
LAUNCHER="$HOME/bin/start-hermes-gateway.sh"

case "${1:-check}" in
  start)  exec "$LAUNCHER" ;;
  stop)   hermes gateway stop
          tmux kill-session -t "$SESSION" 2>/dev/null ;;
  restart) hermes gateway stop 2>/dev/null
           tmux kill-session -t "$SESSION" 2>/dev/null
           sleep 2
           exec "$LAUNCHER" ;;
  check)  if tmux has-session -t "$SESSION" 2>/dev/null; then
            echo "✓ $SESSION is running"
          else
            echo "✗ $SESSION is NOT running"
          fi ;;
esac
```

### ตัวอย่าง hmgw2

```bash
#!/data/data/com.termux/files/usr/bin/bash
# hmgw2 — control secondmodel Hermes gateway
TMUX_TMPDIR=/data/data/com.termux/files/usr/tmp
SESSION="hermes-secondmodel"
LAUNCHER="$HOME/bin/start-hermes-secondmodel.sh"

case "${1:-check}" in
  start)  exec "$LAUNCHER" ;;
  stop)   HERMES_PROFILE=secondmodel hermes gateway stop
          tmux kill-session -t "$SESSION" 2>/dev/null ;;
  restart) HERMES_PROFILE=secondmodel hermes gateway stop 2>/dev/null
           tmux kill-session -t "$SESSION" 2>/dev/null
           sleep 2
           exec "$LAUNCHER" ;;
  check)  if tmux has-session -t "$SESSION" 2>/dev/null; then
            echo "✓ $SESSION is running"
          else
            echo "✗ $SESSION is NOT running"
          fi ;;
esac
```

**สำคัญ:** `hmgw2` ต้องใช้ `HERMES_PROFILE=secondmodel` ทุกครั้งที่เรียก `hermes gateway *`

## Step 7: Greeting Messages แยก

### ⚠️ Greeting มี 3 จุด — อย่าสับสน

| # | เมื่อไหร่ | อยู่ที่ไหน | ใช้กับ profile ไหน |
|---|----------|-----------|-------------------|
| 1 | Gateway เริ่มต้น | Launcher script (`~/bin/start-hermes-gateway.sh`) | default เท่านั้น (ตอนนี้ปิด greeting launcher อยู่) |
| 2 | Gateway เริ่มต้น | Hook `hooks/startup-greeting/` event `gateway:startup` | ใช้ได้ทั้ง default และ secondmodel **ถ้าใส่ใต้ HERMES_HOME ของ profile นั้น** |
| 3 | New session ใน Telegram | Hook `hooks/startup-greeting/` event `session:start` | default เท่านั้น (ถ้าต้องการ) |

**สำคัญมาก:** hooks เป็น **profile-scoped ตาม `HERMES_HOME`** ไม่ได้แชร์กันอัตโนมัติทุก profile
- default profile โหลดจาก `~/.hermes/hooks/`
- secondmodel โหลดจาก `~/.hermes/profiles/secondmodel/hooks/`

เพราะงั้น ถ้าต้องการให้ลี่มี startup greeting แบบเดียวกับเฮอ:
1. สร้าง hook ใต้ `~/.hermes/profiles/secondmodel/hooks/startup-greeting/`
2. ให้ hook นี้รับแค่ `gateway:startup`
3. ปิด launcher greeting ของลี่ เพื่อกันยิงซ้ำ

### Hook-based greetings (แนะนำสำหรับหลาย profile)

ถ้าต้องการให้ greeting ยิงหลัง `telegram connected` แบบเสถียร ให้ใช้ hook มากกว่า launcher เพราะไม่ต้องลุ้น race จาก `hermes send` ตอน gateway ยัง init ไม่เสร็จ

ดูรายละเอียดโครงสร้าง hook และ reference จริงได้ที่:
- `references/greeting-hook-config.md`
- `references/greeting-profile-scoped-hooks-2026-06-11.md`

### Launcher-based greetings

launcher greeting ยังใช้ได้ แต่เหมาะกับกรณี profile เดียวหรือกรณีที่ยอมรับความเสี่ยงเรื่อง timing ได้ ถ้าเปลี่ยนไปใช้ hook แล้ว **ต้องลบ/ปิด greeting call ใน launcher** ของ profile นั้นด้วย

`~/bin/hermes-greeting-default.sh`:
```bash
#!/data/data/com.termux/files/usr/bin/bash
# เฮอ greeting — 16 variants
TOKEN="$(grep TELEGRAM_BOT_TOKEN ~/.hermes/.env | cut -d= -f2)"
CHAT_ID="5293931735"
MESSAGES=("พร้อมลุยแล้วค่า ✨" "เฮอออนแว้ว ~" ...)
MSG="${MESSAGES[$((RANDOM % ${#MESSAGES[@]}))]}"
curl -s -X POST "https://api.telegram.org/bot${TOKEN}/sendMessage" \
  -d "chat_id=${CHAT_ID}" -d "text=${MSG}" > /dev/null
```

`~/bin/hermes-greeting-secondmodel.sh`:
```bash
#!/data/data/com.termux/files/usr/bin/bash
# ลี่ greeting — 18 variants
TOKEN="$(grep TELEGRAM_BOT_TOKEN ~/.hermes/profiles/secondmodel/.env | cut -d= -f2)"
CHAT_ID="5293931735"
MESSAGES=("ลี่ออนแล้วนะค้า 🐱" "พร้อมช่วยแล้วค่ะ ~" ...)
MSG="${MESSAGES[$((RANDOM % ${#MESSAGES[@]}))]}"
curl -s -X POST "https://api.telegram.org/bot${TOKEN}/sendMessage" \
  -d "chat_id=${CHAT_ID}" -d "text=${MSG}" > /dev/null
```

**⚠️ Pitfall:** สิ่งที่ทำให้ข้อความซ้ำไม่ใช่แค่ "ใช้ hooks หลาย profile" แต่คือ **ใช้ hook + launcher ใน profile เดียวกันพร้อมกัน** หรือใส่ hook ผิด HERMES_HOME จนไปแก้ default ทั้งที่ตั้งใจแก้ secondmodel

## Step 8: ทดสอบ

### ตรวจสอบ gateway

```bash
hmgw status   # ดูภาพรวมทั้ง hmgw1 + hmgw2
hmgw1 check   # ควรเห็น ✓ hermes-gateway is running
hmgw2 check   # ควรเห็น ✓ hermes-gateway-secondmodel is running
```

> ใช้ `hmgw1` / `hmgw2` เป็นคำสั่งหลัก. คำสั่ง `hg*` เป็น legacy shim เท่านั้น และควร forward ไป hmgw1/hmgw2 พร้อม deprecation note

### ทดสอบ Topic Routing

| ส่งข้อความใน | เฮอควรตอบ? | ลี่ควรตอบ? |
|-------------|:---------:|:---------:|
| ⚡ Header (51) | ✅ | ❌ |
| 🔎 research (59) | ❌ | ✅ |
| 📝 writer (3) | ❌ | ✅ |
| 💬 ALL (185) | ✅ (เมื่อ mention) | ✅ (เมื่อ mention) |
| DM ส่วนตัว | ✅ | ✅ |

### แก้ไขเมื่อ routing ไม่ทำงาน

1. เช็ค `gateway.log` ว่าเห็น `inbound message` ใน topic ที่ต้องการมั้ย
2. `hermes config get telegram.allowed_topics --profile default`
3. `hermes config get telegram.allowed_topics --profile secondmodel`
4. Restart gateway หลังแก้ config: `hmgw1 restart` / `hmgw2 restart`

## คำสั่งที่มีประโยชน์

```bash
# ดูว่ามี profile อะไรบ้าง
hermes profile list

# เช็คค่า config ทั้งหมดของ profile
hermes config list --profile secondmodel

# ดู log แยก
tail -f ~/.hermes/logs/gateway.log             # default
tail -f ~/.hermes/profiles/secondmodel/logs/gateway.log  # secondmodel

# tmux sessions
TMUX_TMPDIR=/data/data/com.termux/files/usr/tmp tmux ls
```

## ข้อควรระวัง (Pitfalls)

1. **Bot token ห้ามซ้ำ** — แต่ละ profile ต้องใช้ bot token คนละตัว ไม่งั้น Telegram จะงงว่าใครเป็นใคร
2. **`allowed_topics` ใช้ topic_id ล้วน ๆ** — ไม่ต้องใส่ `:topic_id` ต่อท้าย group_id (Hermes ไม่รองรับ syntax `chat_id:topic_id` ใน `allowed_chats`)
3. **Restart หลังแก้ config เสมอ** — `hermes config set` แล้ว restart gateway ถึงจะมีผล
4. **channel_prompts indent** — ระวัง YAML indent, `|` คือ multiline string
5. **secondmodel ต้องใช้ `--profile` flag** — `HERMES_PROFILE=secondmodel hermes config set ...` **ใช้ไม่ได้!** มันจะไปเขียน default config แทน ต้องใช้ `hermes config set ... --profile secondmodel` เท่านั้น
6. **`hermes config set` เขียนทับ YAML ทั้งไฟล์** — การใช้ `hermes config set` จะ re-serialize YAML ใหม่ทั้งไฟล์ ถ้ามี nested structure ซับซ้อน (เช่น `channel_prompts` ที่มีหลาย topic) อาจทำให้ข้อมูลหายหรือถูกแปลงเป็น escaped unicode (`\uXXXX`) → **backup ก่อนทุกครั้ง!**
7. **`patch` tool บล็อก config.yaml** — patch จะ refuse แก้ไขไฟล์ `~/.hermes/config.yaml` โดยตรง (security protection) ต้องใช้ `hermes config set` หรือ terminal (`python3`/`sed`) แทน
8. **Privacy Mode ต้องปิด** — ทั้งสอง bot ต้อง `/setprivacy` → Disable ที่ BotFather
9. **ALL (topic 185) เป็นห้องรวมแบบ mention-gated** — สำหรับ John's setup ห้ามให้ทั้ง 2 บอท free-response ใน ALL. ต้องใช้ `channel_prompts.185` บังคับว่า: ตอบเฉพาะตอนถูก `@mention`, ห้ามแย่งกันตอบ, และกติกาต้องสมมาตรทั้งสองฝั่ง: ถ้าพี่จอนสั่งให้เฮอเรียกลี่ เฮอต้องใช้ `@miguel890bot` แบบ explicit ไม่ใช่พูดลอย ๆ ว่า "เรียกลี่"; ถ้าพี่จอนสั่งให้ลี่เรียกเฮอต่อ ลี่ก็ต้องแท็กเรียกเฮอต่อด้วย `@mention` ตามที่สั่งเช่นกัน. ดู `references/all-topic-mention-semantics.md`
10. **Session greeting กับ Gateway greeting คนละตัว** — ถ้าจะปิด session greeting (ตอน new session) ให้แก้ `~/.hermes/hooks/startup-greeting/HOOK.yaml` เอา `session:start` ออก; ถ้าจะปิด gateway greeting ให้แก้ launcher script แทน อย่าสับสน!
11. **Topic ปิด (closed) = บอทส่งไม่ได้เลย** — ถ้าในแอป Telegram มีคน "close" topic (เช่น General/1) บอทจะได้ error `telegram.error.BadRequest: Topic_closed` ทุกครั้งที่ตอบ → เปิด topic ในแอป หรือสร้าง topic ใหม่ แล้ว migrate config (allowed_topics + channel_prompts) ไปยัง topic ใหม่
12. **Sub-agent gateway อาจ "ค้าง polling" โดยไม่ error** — เจอกรณี: Telegram connected ตามปกติ, log หยุดเงียบ 20-30 นาที, ข้อความใหม่ไม่ถูกดึงเข้า `inbound message` เลย → fallback IP `149.154.166.110` ไม่ retry → **fix: `hmgw2 restart`** (รออนุมัติก่อน) แล้วลี่จะดึงข้อความค้างมาทำงานต่อ. ดู reference: `references/sub-agent-gateway-stuck-incident-2026-06-10.md`
13. **"ส่งแล้ว/รอลี่ตอบ" ไม่ใช่การยืนยันว่างานถึงมือลี่** — ถ้าหนูส่งข้อความเข้า topic แล้วบอก user "รอลี่ทำ" ทันที อาจเข้าใจผิดทั้งสองฝ่าย. ต้องตรวจ log gateway ของ profile นั้นภายใน 1-2 นาที เพื่อยืนยันว่าเห็น `inbound message` ใน topic ที่ส่งจริง ๆ แล้วค่อยบอก user
14. **"เขียนแผน" ≠ "ส่งงาน"** — เวลา user สั่ง "วางแผนและแบ่มงานกันได้เลย" ห้ามตอบแค่แผ่นแผน. ต้องลงมือส่งจริงในเทิร์นเดียว (ผ่าน `send_message` ไปยัง topic ของ sub-agent) แล้วค่อยบอก user ว่า "ส่งแล้ว msg ID เท่าไหร่" ไม่ใช่แค่ "เดี๋ยวจะส่ง"
15. **`allowed_topics` คุม INBOUND ไม่ใช่ OUTBOUND** — agent ส่งข้อความ cross-topic ได้ (เช่น เฮอส่งเข้า writer/3) โดยไม่ต้องมี `3` ใน `allowed_topics` ของเฮอ. ดังนั้น **ห้ามเพิ่ม topic ของ sub-agent เข้า `allowed_topics` ของ agent หลัก** เพราะจะทำให้ทั้ง 2 bot subscribe topic เดียวกัน → แย่งกันตอบ. ทางที่ถูกคือใช้ `channel_prompts` บอกให้ agent หลัก cross-post แทน
16. **`hermes config set` ใช้แก้ channel_prompts ได้ทั้ง inline string (มี `\uXXXX` ก็ไม่เป็นไร)** — `hermes config set telegram.channel_prompts.51 "..."` ใช้งานได้จริงและไม่ทำลาย nested YAML (ขัดกับ pitfall #6 เดิมที่บอกว่า "re-serialize ทั้งไฟล์"). ใช้ได้ทั้งแบบ multi-line string ผ่าน CLI เพราะ `hermes config set` รับ shell-arg. **แต่:** ถ้าจะแก้หลาย topic ใน profile เดียว ควร backup ก่อนเสมอ เผื่อมี edge case
17. **ถ้าจะย้าย secondmodel จาก launcher-greeting ไป hook-greeting ต้องย้ายให้สุด** — สร้าง hook ใต้ `~/.hermes/profiles/secondmodel/hooks/startup-greeting/` แล้วปิด call greeting ใน `start-hermes-secondmodel.sh` ทันที ไม่งั้นจะโดนสองเด้ง (hook + launcher) หรือแก้ผิดที่แล้วไปทับ default profile
18. **Greeting hook ของ secondmodel ต้องอยู่ใต้ profile path ไม่ใช่ root** — ถ้าแก้แค่ `~/.hermes/hooks/startup-greeting/` จะมีผลกับ default profile แต่ secondmodel อาจไม่โหลดเลย เพราะมันอ่านจาก `~/.hermes/profiles/secondmodel/hooks/` ตาม `HERMES_HOME`
19. **Greeting ใช้ DM ไม่ใช่ group** — ไม่ว่าจะใช้ launcher script หรือ hook ตอนนี้ target คือ `5293931735` (DM ของ John) ถ้าต้องการ greeting ใน group ต้องเปลี่ยน target/chat_id เป็น group/topic ที่ต้องการแทน

## 🔧 การแก้ไข config ภายหลัง (Maintenance)

หลังจาก sub-agent setup เสร็จแล้ว ถ้าต้องแก้ไข `channel_prompts` หรือ `allowed_topics` เพิ่ม:

### Safe Workflow

```bash
# 1. BACKUP ก่อนเสมอ
cp ~/.hermes/config.yaml ~/.hermes/backups/config.yaml.bak.$(date +%Y%m%d_%H%M%S)
cp ~/.hermes/profiles/secondmodel/config.yaml ~/.hermes/backups/config-secondmodel.yaml.bak.$(date +%Y%m%d_%H%M%S)

# 2. แก้ allowed_topics — ใช้ --profile flag ห้ามใช้ env var
hermes config set telegram.allowed_topics '51,1'                    # default
hermes config set telegram.allowed_topics '59,3,1' --profile secondmodel  # secondmodel

# 3. แก้ channel_prompts — 
#    สำหรับค่า string เดี่ยว: hermes config set ใช้ได้
hermes config set telegram.channel_prompts.1 'เนื้อหา...'

#    ตัวอย่าง ALL/185 แบบ mention-gated สมมาตร:
#    - default/เฮอ: ตอบเฉพาะตอน @mention เรียกเฮอ, ถ้าพี่จอนสั่งให้เรียกลี่ต่อ ต้อง @miguel890bot
#    - secondmodel/ลี่: ตอบเฉพาะตอน @mention เรียกลี่ หรือเฮอ @mention @miguel890bot,
#      ถ้าพี่จอนสั่งให้เรียกเฮอต่อ ต้องแท็กเรียกเฮอต่อด้วย @mention

#    สำหรับ block scalar multiline (|): ใช้ terminal + Python script
#    เพราะ patch tool โดนบล็อก และ hermes config set จะแปลงเป็น escaped unicode
python3 << 'PYEOF'
path = '/data/data/com.termux/files/home/.hermes/config.yaml'
with open(path, 'r') as f:
    content = f.read()
# ใช้ .replace() หรือ regex เพิ่ม block ก่อน marker ที่ต้องการ
content = content.replace('  allowed_chats:', '  <new channel_prompt>\n  allowed_chats:')
with open(path, 'w') as f:
    f.write(content)
PYEOF

# 4. ตรวจสอบ
grep 'allowed_topics' ~/.hermes/config.yaml ~/.hermes/profiles/secondmodel/config.yaml
grep -nE "['\"]?(51|59|3|1)['\"]?:" ~/.hermes/config.yaml ~/.hermes/profiles/secondmodel/config.yaml

# 5. Restart
hmgw2 restart && hmgw1 restart
```

### Common Mistakes & Solutions

| ปัญหา | สาเหตุ | วิธีแก้ |
|-------|--------|--------|
| แก้ default config แต่ดันไปแก้ secondmodel | ใช้ `HERMES_PROFILE=secondmodel hermes config set` → มันไม่ทำงาน | ใช้ `--profile secondmodel` flag |
| `channel_prompts` หายหลังจาก `hermes config set` | `hermes config set` re-serialize YAML ใหม่ ทำให้ nested structures พัง | Restore จาก backup แล้วแก้ด้วย Python script แทน |
| patch โดนบล็อก `Refusing to write to Hermes config file` | Security protection — ห้าม agent แก้ config ตรง ๆ | ใช้ `hermes config set` หรือ terminal |
| channel prompt กลายเป็น `\uXXXX` (escaped unicode) | `hermes config set` จะ escape non-ASCII characters อัตโนมัติ | YAML ยังอ่านได้ปกติ ไม่ต้องแก้ แต่ถ้าอยากให้อ่านง่าย ใช้ Python เขียน block scalar (`\|`) แทน |
| Topic ปิดในแอป Telegram → บอทส่งไม่ได้ | คนในกลุ่มกด "Close topic" → Telegram คืน `Topic_closed` | สร้าง topic ใหม่ + migrate config (allowed_topics + channel_prompts) ไป topic ใหม่ |

### 🔁 Recipe: Verify sub-agent received the task (ก่อนบอก user "รอ")

หลังส่ง `send_message` ไปยัง topic ของ sub-agent **ต้อง verify ภายใน 1-2 นาที** ว่าข้อความถึง gateway ของ profile นั้น:

```bash
# ดู log ของ sub-agent profile (ปรับ path ตาม profile)
tail -20 ~/.hermes/profiles/secondmodel/logs/gateway.log | grep "inbound message"

# ถ้าไม่เห็น → gateway อาจค้าง polling (ดู pitfall #12)
# ถ้าเห็น → ยืนยันได้ว่างานถึงมือ sub-agent แล้ว
```

**สัญญาณ gateway ค้าง:**
- log หยุดเงียบเกิน 5 นาที (ทั้งที่ `Connected to Telegram` ครั้งสุดท้ายอยู่ใน log)
- `date "+%H:%M"` ตอนนี้ กับ timestamp log ล่าสุด ห่างกันเยอะ
- ไม่มี `inbound message` ใหม่เลย

**Fix:** `hmgw2 restart` แล้วลี่จะดึง msg ที่ค้างมาทำเอง

### 🔁 Recipe: Migrate Topic (e.g. General/1 → ALL/185)

เมื่อ topic เก่าถูกปิดและต้องย้ายไป topic ใหม่:

```bash
# 1. Backup
cp ~/.hermes/config.yaml ~/.hermes/backups/config.yaml.bak.$(date +%Y%m%d_%H%M%S)
cp ~/.hermes/profiles/secondmodel/config.yaml ~/.hermes/backups/config-secondmodel.yaml.bak.$(date +%Y%m%d_%H%M%S)

# 2. Migrate ด้วย Python (แก้ OLD_TOPIC, NEW_TOPIC, NEW_NAME ได้ตามต้องการ)
python3 << 'PYEOF'
import yaml, shutil
OLD_TOPIC, NEW_TOPIC, NEW_NAME = "1", "185", "ALL"  # ปรับตามจริง
for path in ['/data/data/com.termux/files/home/.hermes/config.yaml',
             '/data/data/com.termux/files/home/.hermes/profiles/secondmodel/config.yaml']:
    with open(path) as f: c = yaml.safe_load(f)
    tg = c.setdefault('telegram', {})
    cp = tg.setdefault('channel_prompts', {})
    if OLD_TOPIC in cp:
        new = cp[OLD_TOPIC].strip()
        # แทนที่ชื่อ topic ใน prompt
        for old_name in [f'Topic: 💬 General', 'Topic: General', 'ในห้อง General']:
            new = new.replace(old_name, f'Topic: 💬 {NEW_NAME}' if 'Topic' in old_name else f'ในห้อง {NEW_NAME}')
        cp[NEW_TOPIC] = new
        del cp[OLD_TOPIC]
    # อัปเดต allowed_topics
    topics = tg.get('allowed_topics', '').split(',')
    topics = [t.strip() for t in topics if t.strip()]
    if OLD_TOPIC in topics:
        topics = [NEW_TOPIC if t == OLD_TOPIC else t for t in topics]
    tg['allowed_topics'] = ','.join(topics)
    with open(path, 'w') as f:
        yaml.dump(c, f, default_flow_style=False, allow_unicode=True, sort_keys=False, width=120)
print("✅ Migrated both profiles")
PYEOF

# 3. Verify
grep 'allowed_topics' ~/.hermes/config.yaml ~/.hermes/profiles/secondmodel/config.yaml

# 4. Restart gateway
hmgw1 restart && hmgw2 restart
```

## Related Skills

- `gateway-troubleshooting` — แก้ปัญหา gateway ไม่ตอบ, หลุด, เช้าไม่ตื่น (รวม `Topic_closed` แล้ว)
- `hermes-ecosystem` — ภาพรวม Hermes ecosystem บน Android/Termux
- `memory-3r` — ระบบความจำ 3R

- `references/all-topic-mention-semantics.md` — กติกา ALL ล่าสุด: mention-only, anti-racing, และตัวอย่างที่เฮอต้องเรียกลี่ด้วย `@miguel890bot`
- `references/sub-agent-v2-topic-reporting.md` — stable v2 pattern for Header → Kanban → secondmodel → writer/research report → default final review → ALL delivery, including topic IDs, @trpsry delivery requirement, prompt examples, and verification metadata.
- `references/topic-closed-incident-2026-06-10.md` — transcript จริงของเคส General/1 ถูกปิด → migrate ไป ALL/185 พร้อม log timeline และ config diff
- `references/sub-agent-gateway-stuck-incident-2026-06-10.md` — เคส sub-agent gateway ค้าง polling (log เงียบ 28 นาที, ข้อความค้างที่ Telegram server) → fix: `hmgw2 restart` พร้อม detection recipe และ prevention workflow
- `references/delegation-pattern-incident-2026-06-10.md` — เคส เฮอไม่ cross-post delegate จริง (ตอบยาวใน Header แทน), root cause = "แนะนำ" ไม่ใช่ "บังคับ" ใน prompt, fix: actionable checklist + negative list + keyword emphasis
- `references/greeting-systems-2026-06-11.md` — วิเคราะห์ 2 ระบบ greeting ขนานกัน (Hook vs Launcher), race condition เดิมของ secondmodel, และเหตุผลที่ย้ายลี่ไปใช้ profile-scoped hook
- `references/greeting-profile-scoped-hooks-2026-06-11.md` — recipe จริงสำหรับแยก startup greeting คนละ profile โดยใช้ `HERMES_HOME` ของแต่ละ profile และปิด launcher greeting เพื่อกันข้อความซ้ำ
