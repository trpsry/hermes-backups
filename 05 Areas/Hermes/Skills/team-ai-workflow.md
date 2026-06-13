---
copied_on: 2026-06-12
skill_name: team-ai-workflow
skill_path: devops/team-ai-workflow/SKILL.md
source: Hermes skill
tags:
- hermes
- skills
- workflow
- telegram
- multi-agent
---
---
name: team-ai-workflow
description: "Multi-agent team workflow on Telegram — role-based coordination (Manager + Researcher/Writer), task routing, human approval gates, and topic-based delegation in Telegram groups."
version: 1.1.0
author: Hermes Agent (Her)
tags: [multi-agent, telegram, team, workflow, coordination, approval, content-creation]
related_skills: [multi-agent-discord, kanban-orchestrator, facebook-page-posting-workflow]
---

# Team AI Workflow (Telegram)

## When to use
- John assigns work that needs multiple agents (research + write + review)
- Content creation pipeline (find topic → research → draft → review → publish)
- Any task that benefits from separating research, writing, and approval stages

## Team Structure

```
John (Owner)
    │
    ├── เฮอ (Main Agent / Manager) — hmgw1
    │       • Receives all commands from John
    │       • Plans work, assigns to ลี่
    │       • Reviews ลี่'s output
    │       • Handles config/root/system alone
    │       • Explains risks before any risky action
    │       • Final answer goes through เฮอ before John uses it
    │
    └── ลี่ (Sub Agent / Researcher + Writer) — hmgw2
            • Researches topics
            • Summarizes findings
            • Drafts posts/content
            • Fact-checks and proposes angles
            • Flags risky tasks → waits for approval
            • CANNOT touch config/root/system/API keys
```

## Task Routing Rules

| Task Type | Who Does It | Notes |
|-----------|-------------|-------|
| topic research | ลี่ | เฮอ assigns, ลี่ delivers findings |
| topic writers | ลี่ | เฮอ assigns, ลี่ drafts content |
| All (manage + summarize) | เฮอ | เฮอ coordinates and delivers final |
| config / root / system | เฮอ only | ลี่ never touches these |
| Risky actions | เฮอ explains → John approves | No exceptions |

## Workflow Pattern

### 1. Receive command from John
- Parse the request
- Identify what needs research vs what needs writing

### 2. Plan and assign
- If research needed → assign to ลี่ with clear brief
- If writing needed → wait for research, then assign to ลี่
- If complex → break into stages, assign sequentially

### 3. ลี่ executes
- ลี่ researches / drafts in the assigned Telegram topic
- ลี่ delivers findings or draft back

### 4. เฮอ reviews
- Check quality, accuracy, tone
- If issues → send back to ลี่ with specific feedback
- If good → prepare final answer for John

### 5. John approves
- เฮอ presents final answer with clear approve/reject options
- John says "โพสต์เลย" / "อนุมัติ" / "แก้ก่อน" / "ยกเลิก"

## Approval Gates

Every deliverable needs explicit John approval before:
- Publishing to Facebook / social media
- System/config changes
- Any action with real-world side effects

**Approved phrases:** `อนุมัติ`, `โพสต์เลย`, `ส่งได้`, `โอเค`, `ลุย`
**Reject phrases:** `แก้ก่อน`, `ยกเลิก`, `ไม่เอา`, `เปลี่ยน`

## Telegram Group Setup

Both bots share the same Telegram group with topics:
- **Group:** "เฮอ & ลี่"
- **Topic 1 (General):** Main discussion
- **Topic "research":** ลี่ does research here
- **Topic "writers":** ลี่ drafts content here

### Finding Group/Topic IDs

The `send_message action=list` tool only returns display names, not numeric IDs.
Search gateway logs instead:

```bash
# Find all unique chat IDs across all logs
grep -roh "chat=-\d\+\|chat=\d\+" ~/.hermes/logs/ 2>/dev/null | sort -u

# Find messages from a specific group by name
grep "inbound message.*Group Name" ~/.hermes/logs/ | tail -5

# For secondmodel (ลี่) logs specifically
grep -roh "chat=-\d\+\|chat=\d\+" ~/.hermes/profiles/secondmodel/logs/ 2>/dev/null | sort -u
```

Log line format:
```
inbound message: platform=telegram user=... chat=-100XXXXXXXXXX msg='...'
```

Target format for `send_message`:
```
telegram:<chat_id>:<thread_id>     # group with topic
telegram:<chat_id>                  # group without topic / DM
```

Supergroup chats start with `-100`; DM chats are positive integers.

## Cross-Agent Communication

Currently no direct bridge between hmgw1 (เฮอ) and hmgw2 (ลี่) inside Telegram topics.

**Important practical rule:** do not rely on bot-to-bot Telegram messages as the main handoff path. Even if both bots are in the same group/topic and routing looks correct, the second bot may not reliably receive the first bot's message as an inbound task trigger.

Recommended approaches:
1. **Kanban / internal handoff** (recommended): เฮอ creates a task assigned to `secondmodel`, dispatches it, then posts a short status update in writer/research for John to see.
2. **Via John as middleman** (fallback): ถ้ายังไม่ได้ตั้ง Kanban/internal handoff ให้เฮอบอก John ว่าจะส่งอะไรให้ลี่
3. **Direct bot-to-bot bridge** (future): only if Hermes gets a real native bridge later — do not assume topic routing alone solves this

### Required handoff sequence (John's setup)

When John gives work in `⚡ Header` and Her decides Leslie should continue it, do this in the same turn:
1. Create a Kanban task assigned to `secondmodel`
2. Dispatch it (or otherwise ensure the worker picks it up)
3. Only after the task exists, post a short status line in `writer` or `research`
4. Treat those Telegram topics as **reporting/review rooms**, not as the delivery bus for the actual handoff

Default decision rule for John's setup:
- If the Header request is mainly research, draft, rewrite, or critique, and John did not explicitly say "เฮอทำเอง" or equivalent, Her should treat Kanban handoff to `secondmodel` as the default path before giving any long final answer in Header.
- A short plan in Header is fine, but saying "จะส่งต่อ" without a real task is not a handoff.

### ALL / writer / research mention semantics
For John's setup, Telegram topic chatter is allowed, but do not confuse it with task transfer:
- `@mention` in `💬 ALL`, `📝 writer`, or `🔎 research` can be a signal to reply, acknowledge, or discuss.
- In `💬 ALL`, both bots should reply **only when explicitly @mentioned**; if not mentioned, do not jump in or race each other.
- Mention behavior in `💬 ALL` must be symmetric: if John asks Her and tells her to call Leslie next, Her must answer first and explicitly @mention ลี่ in the same visible reply; if John asks Leslie and tells her to call Her next, Leslie should answer first and then tag Her as instructed.
- This also applies to lightweight roll-call / intro / status prompts, not only real delegated work. If John says `ตอบก่อนแล้วแท็กน้องลี่มาต่อ` or equivalent, Her should not stop at `ลี่มาต่อได้เลย` — the actual @mention is required.
- It is **not** by itself proof that cross-profile delegation happened.
- Leslie should treat mention-only traffic as chat/reporting unless a real Kanban task also exists.
- When auditing whether Her delegated correctly, verify the Kanban artifact first (task id, assignee, dispatch/run state), then use Telegram messages only as reporting evidence.

### Delivery workflow v2 — Header → worker task → final review task → ALL

For John’s current requirement, every Header task that needs Leslie must create **two Kanban cards**. See `references/header-kanban-final-review-flow.md` for the complete checklist, task-body requirements, and verification pattern.

### Header planning-only requests (do not over-delegate)

If John asks Her to **review a note, plan the work, decide the split, and post the planning status in Header** before actual execution starts, Her may do that first **without creating Leslie worker cards yet**.

Use this lighter path only when all of these are true:
- the work requested right now is planning / scoping / role split / early findings
- no actual research-writing execution has started yet
- Her is only updating the working note and reporting the plan in Header
- saying "next I recommend inventory/classification" is still a proposal, not a dispatched task

In this planning-only case, Her should:
1. read the note / context
2. inspect enough surrounding structure to make the plan real
3. decide what Leslie *would* own once execution starts
4. update the working note or plan if needed
5. post a concise Header status with findings, split, and suggested next phase

Do **not** claim handoff happened unless a real Kanban task exists.
Once John says to actually start the delegated phase, switch back to the normal two-card Kanban flow.

1. **Leslie worker card** (`assignee=secondmodel`)
   - Body says what Her assigned.
   - Body includes report topic target:
     - research: `telegram:-1003985263404:59`
     - writer: `telegram:-1003985263404:3`
   - Leslie must immediately post in that topic: "ลี่รับงานจากเฮอแล้ว: ..."
   - Leslie does the work.
   - Leslie posts completion in the same topic: "งานเสร็จแล้ว กำลังส่งให้เฮอตรวจ final"
   - Leslie completes the card with a summary/output for Her.

2. **Her final-review card** (`assignee=default`, `parent=<Leslie task id>`)
   - Reads parent result.
   - Reviews facts/tone/risk/completeness.
   - Posts review/final in Header `telegram:-1003985263404:51`.
   - Posts delivery summary in ALL `telegram:-1003985263404:185`, mentioning John with `@trpsry`.
   - Completes the review card.

This is stronger than prompt-only delegation because the final review becomes a durable follow-up task instead of relying on Her to remember to poll Leslie manually.

### After a dry-run gets approved: escalate in staged phases

When John first approved only a **dry-run / plan**, then later says to actually execute it, do not rewrite the whole task from scratch as one big action. Convert the approved plan into **small staged worker cards**.

Recommended pattern:
- keep phase 1 narrow and low-risk
- tell Leslie exactly what real side effects are now allowed
- require a short **before / after** summary in the worker output
- keep the same two-card flow: Leslie executes, Her reviews, then Her reports in ALL
- for note/file cleanup, phase 1 should prefer: move the minimum necessary files, add short clarifier text, and verify links after the move before touching broader archive/support cleanup

John-specific reason:
- this keeps approval boundaries clear
- makes the ALL summary easier to read
- reduces the chance that "approved the plan" gets silently stretched into "clean up everything at once"

### Parent-completion auto-follow review behavior

In the two-card Kanban flow, a default-profile final-review card that depends on Leslie's worker card may auto-promote and run once the parent task completes. Treat that as expected behavior, not a surprise.

Practical rule:
- still verify the real artifacts yourself after Leslie finishes
- do not assume the review card posting means the underlying file/link changes are correct
- use the review card as the durable reminder + reporting step, not as proof by itself

### Verification pattern

After changing cross-profile workflow, run a real smoke test instead of trusting prompts/config by inspection:
- create a tiny Kanban task assigned to `secondmodel`
- dispatch it
- verify the task reaches `done`
- verify run metadata or summary clearly shows the worker received the task via Kanban/internal handoff, not Telegram relay

### Cross-profile Kanban skill attachment rule

When Her creates a Kanban task for another profile, do **not** casually attach extra skills unless you are sure that worker profile can resolve them.

Practical rule:
- bundled skills are usually safe
- profile-local / agent-authored skills may differ between profiles
- if the task is simple enough, prefer a plain task body over forcing extra skills
- if a worker crashes immediately after spawn, inspect whether the attached skill name is unavailable in that worker profile before assuming the workflow itself is broken
- recovery path: create a clean replacement card without the questionable extra skill, dispatch again, and report the new task id(s) clearly

This matters because a bad attached skill can make a healthy Kanban handoff look like a broken multi-agent bridge.

### Pitfall

Kanban workers run under their assigned profile's own skill library. A skill visible in `default` is **not automatically visible** to `secondmodel`.

When a worker crashes with `Unknown skill(s): ...`, check this first:
- confirm the skill exists in the main profile
- confirm the same skill exists in `~/.hermes/profiles/<assignee>/skills/...`
- if missing, install/copy the skill into that profile before retrying the task
- then run a fresh smoke test task to confirm the worker can now load the skill and complete end-to-end

John-specific lesson from this class of failure:
- do not read "ลี่ไม่ทำ" as a behavior problem until Kanban logs are checked
- if the task spawned but crashed before work began, treat it as a profile/skill loading problem first

### Pitfall

If Her says "จะส่งงานต่อให้ลี่" but has not actually created a task yet, the handoff is not real. In John's setup, never present a Telegram topic post alone as proof that Leslie received work.

## Post Review Workflow (เHoverview งานลี่)

When ลี่ drafts content (Facebook post, research summary, etc.), เฮอ must review before John sees it:

### Review Checklist
1. **Fact-check** — Verify claims are accurate, not overstated
   - Check numbers/stats against sources
   - Flag "เวอร์" (exaggerated) claims
   - Verify technical accuracy
2. **Tone adjustment** — Match "คลั่ง Ai" page tone
   - จัดจ้าน but not clickbait
   - Direct but not misleading
   - Opinionated but backed by facts
3. **Reduce oversimplification** — AI topics need nuance
   - "งานเร็วขึ้น 3-5 เท่า" → "เร็วขึ้นสำหรับงานบางประเภท"
   - "ผิดพลาดน้อยลง" → "จับผิดได้ดีขึ้น"
   - "คนไม่ต้องนั่งเฝ้า" → "แต่ยังต้องมีคน监督 อยู่นะ"
4. **Format for readability** — Short paragraphs, bullet points, clear structure

### Review Output Format
```
🔍 ผลตรวจ:

| จุด | สถานะ |
|-----|-------|
| ข้อเท็จจริง | ✅/⚠️/❌ |
| โทน | ✅/⚠️ |
| ความเวอร์ | ✅/⚠️ |

✅ Final Version พร้อมโพสต์:
[corrected version]

สรุปที่แก้:
- [change 1]
- [change 2]
```

### Reading ลี่'s Output from Logs
```bash
# Find ลี่'s recent responses in the group
grep -E "response ready.*1003985263404" ~/.hermes/profiles/secondmodel/logs/gateway.log | tail -5

# Read from secondmodel session database
python3 -c "
import sqlite3
conn = sqlite3.connect('/data/data/com.termux/files/home/.hermes/profiles/secondmodel/state.db')
cursor = conn.cursor()
cursor.execute(\"SELECT content FROM messages WHERE session_id='SESSION_ID' AND role='assistant' ORDER BY id DESC LIMIT 1\")
row = cursor.fetchone()
if row: print(row[0])
conn.close()
"
```

## Security Settings (from OpenClaw patterns)

When running multiple bots in a Telegram group, apply these security settings:

### Group Policy
- Set `allowed_chats` in config.yaml to limit which groups the bot joins
- Use specific chat IDs, not wildcards

### User Restriction
- Set `allowed_users` to John's user ID only
- Bot should only respond to authorized users in groups

### Require Mention
- In groups: `require_mention: true` prevents spam (bot only responds when @mentioned)
- For convenience in small trusted groups: `require_mention: false`
- John's current setup: DM = no mention needed, Group = mention required

### Cross-Agent Visibility
- For agents to see each other's sessions, set `session.visibility: all`
- For agents to communicate directly, enable `agent_to_agent: true`
- Currently Hermes uses Kanban + delegate_task for cross-agent coordination

## Pitfalls

1. **Both bots in same topic** — If เฮอ and ลี่ are both in topic 1, they may both respond to the same message. Use @mention or topic separation to avoid confusion.
2. **Topic structure** — ลี่ can post in any topic (e.g., topic 3). Topics don't need to be named "research" or "writers" — just use topic IDs. Check gateway logs for active topics.
3. **Token burn** — Two bots responding = double tokens. Keep ลี่ on cheaper model (deepseek-v4-flash-free).
4. **Style mismatch** — ลี่ may write in different tone than เฮอ. Always review before John sees it.
5. **Cross-agent communication** — Currently no direct bridge. Use John as middleman or read ลี่'s output from logs.
6. **Shared hooks duplicate messages** — Both gateways load `~/.hermes/hooks/startup-greeting`. Move greeting logic to launcher scripts instead. See `hermes-ecosystem` → `references/cross-profile-hooks-isolation.md`.
7. **"เขียนแผน" ≠ "ส่งงานจริง"** — เวลา John สั่ง "วางแผนและแบ่งงานกันได้เลย" ต้องลงมือส่งจริงในเทิร์นเดียว (call `send_message` ไปยัง topic ของ sub-agent) แล้วบอก John ว่า "ส่งแล้ว msg ID เท่าไหร่" ห้ามตอบแค่แผ่นแผน. เคยพลาด: ตอบแผน + "เดี๋ยวจะส่ง" → John ถามซ้ำว่าทำไมไม่เห็นสถานะ
8. **Verify sub-agent รับงานจริง ก่อนบอก John "รอ"** — หลัง `send_message` ต้องตรวจ `tail -20 ~/.hermes/profiles/<name>/logs/gateway.log | grep "inbound message"` ภายใน 1-2 นาที ถ้าไม่เจอ → gateway ค้าง polling (ดู `hermes-sub-agent` pitfall #12) ไม่ใช่ "ลี่ยังคิดอยู่"

## References
- `references/header-kanban-final-review-flow.md` — John-specific v2 delivery protocol: Header planning → Leslie Kanban worker task → writer/research reporting → Her final-review task → Header + ALL delivery.
- `references/telegram-team-setup.md` — Group/topic setup, finding IDs, cross-agent patterns
- `references/content-research-techniques.md` — Finding trending AI topics, API queries
