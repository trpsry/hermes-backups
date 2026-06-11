---
status: active
tags:
- hermes
- termux
- android
- context
- technical
type: area
updated: 2026-06-07
---
## Termux / Android

- ไม่มี `/usr/bin/` — shebang `#!/usr/bin/env` ใช้ไม่ได้ ต้องใช้ full path `#!/data/data/com.termux/files/usr/bin/bash`
- Hermes wrapper ที่ `/usr/bin/hermes` เคยมี shebang เสีย แก้แล้วเมื่อ 2026-05-31

## Hermes CLI & Gateway

- CLI: `/data/data/com.termux/files/usr/bin/hermes`
- Termux docs แนะนำ `hermes gateway run` แบบ foreground (background เป็น best-effort)
- ใช้ custom wrapper: `~/bin/start-hermes-gateway.sh` → tmux session `hermes-gateway`
- LAN dashboard: `hmdb`

## Paths & Backup

- Obsidian vault: `/storage/emulated/0/Documents/Hermes vault`
- Daily backup script: `~/.hermes/scripts/hermes_daily_backup.sh`
- Backup repo: `trpsry/hermes-backups` (clone ที่ `~/hermes-backup-repo`)
- Cron: 22:00 Bangkok (no-agent)
- Splits >49MB → 49MB chunks
- 30-day retention
- Auth: GITHUB_TOKEN

## xurl (X/Twitter CLI)

- Binary: `~/.local/bin/xurl`
- npm package พังบน Android
- ใช้ local patched build v1.1.1 — แค่ print OAuth URL แทน auto-open

## OnePlus 7 Pro

- เสียบปลั๊ก 24/7 (uptime > battery)
- **Bug เคย:** tmux/gateway ถูกฆ่าตอนกลางคืน → แก้โดย disable `oplus.athena` + whitelist Termux (Jun 2026)
- **Tasker rescue:** 04:00 ทุกวัน รัน `~/.termux/tasker/hmgw-ensure-gateway.sh`
- `termux-job-scheduler` มีอาการ hang; สงสัย OEM cleanup
- การแก้ไข oplus/system ต้องขออนุมัติแบบ staged เท่านั้น

## Magisk Root

- มี Magisk root ใน Termux environment
- `su -c` แบบ read-only ใช้ได้กับ: Android settings, deviceidle, appops, dumpsys power/package, logcat

## Mac Desktop

- Mac Desktop เป็น remote UI และ possible worker/computer-use target ผ่าน SSH/bridge
- Hermes จริงรันบน Android/Termux (source of truth)
