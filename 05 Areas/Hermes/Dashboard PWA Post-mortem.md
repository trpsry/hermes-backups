---
status: active
tags:
  - hermes
  - dashboard
  - pwa
  - post-mortem
type: technical
updated: 2026-06-07
---

# Dashboard PWA Post-mortem (2026-06-07)

## สถานะ
❌ **REVERTED** — custom patches ทั้งหมดถูกเอาออกแล้ว 7 มิ.ย. 2026

## สิ่งที่ถูกรีเวิร์ท
- Apple meta tags (apple-mobile-web-app-capable, status-bar-style, title, touch-icon)
- `<link rel="manifest">` 
- `<style>` block ที่มี CSS:
  - iOS safe-area fixes (notch)
  - Anti-zoom (touch-action, font-size 16px)
  - Mobile scroll/overflow fixes
  - Sidebar scroll fixes (failed 3 attempts)

## ไฟล์ที่受影响
- `~/.hermes/hermes-agent/hermes_cli/web_dist/index.html` — กลับไปเป็น Vite build output ปกติ
- `~/.hermes/hermes-agent/hermes_cli/web_dist/manifest.json` — ยังอยู่แต่ไม่ได้ถูก reference

## Sidebar Scroll Fix
**ปัญหา:** sidebar ไม่ scroll บน iPhone (hamburger menu)

**สาเหตุ:** 
- `lg:overflow-hidden` (desktop-only)
- หลาย `shrink-0` flex children
- Global CSS conflicts
- Device-varying safe-area math

**CSS-only attempts (FAILED 3x):**
1. v1: `height: 100dvh` → ไม่ work กับ notched iPhones
2. v2: `height: calc(100dvh - inset-top)` + padding → shrink-0 children ไม่ yield
3. v3: `height: calc(100dvh - inset-top - inset-bottom)` + overflow:hidden → still overflow

**ทางแก้ที่ถูกต้อง:**
ต้องแก้ source-level ใน `web/src/App.tsx` + `npm run build`

## ถ้าต้องการ re-apply
ดู `hermes-ecosystem` skill → `references/dashboard-pwa-standalone.md`
- **Approach A:** Quick fix — edit `web_dist/index.html` directly (survives until next rebuild)
- **Approach B:** Permanent fix — edit source `web/index.html` + `npm run build`

## Related
- [[MOC - Hermes]]
- [[Hermes Termux Android Context]]
