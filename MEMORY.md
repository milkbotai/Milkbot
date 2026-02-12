# Operational Memory

## Recent Events
- [2026-02-12 06:00] System deployed: OpenClaw 2026.2.9 on Contabo VPS (Ubuntu)
- [2026-02-12 06:15] Fixed EACCES bug in installer (exit 243 — CWD inheritance in npx spawn)
- [2026-02-12 06:20] Fixed gateway command: `openclaw start` → `openclaw gateway`
- [2026-02-12 06:30] All 6 services verified active after simulated reboot
- [2026-02-12 06:35] Telegram channel configured: dmPolicy=open, streamMode=partial
- [2026-02-12 06:40] Plugins enabled: telegram, llm-task, lobster, open-prose
- [2026-02-12 06:45] CI passing: 478/478 tests (shellcheck SC2001 fix pushed)
- [2026-02-12 06:52] First 3 missions created and approved (22 total steps)

## Active Context - UPDATED 2026-02-12 Evening
### Current Missions (ALL COMPLETED ✅)
1. **Mission 1** ✅ COMPLETE - Technical Audit (Atlas)
2. **Mission 2** ✅ COMPLETE - Competitive Intelligence (Scout)
3. **Mission 3** ✅ COMPLETE - Performance Baseline (MilkBot + Atlas)
4. **Mission 4** ✅ COMPLETE - Morning Briefing (MilkBot)

### Recent Completions (Today)
- **Repo Fix**: Dashboard pushed to wrong repo, reverted and fixed
- **Twitter/X Access**: Puppeteer tweet fetcher implemented
- **Brave API Automation**: Scripts + cron jobs deployed
- **Dashboard v2.0**: Complete implementation

### System State
- Services: All 14 systemd units active
- Database: binaryrogue (PostgreSQL) operational
- Redis: Heartbeats, alerts, signals, tasks channels active
- Telegram: MilkBot responding on DMs
- Dashboard: v2.0 operational at dashboard.milkbot.ai

### Pending
- **Google Drive Backup Setup** ⭐ HIGH PRIORITY
  - Status: OAuth authorization needed
  - Requires manual terminal interaction (sudo)
  - Command: `sudo rclone authorize drive ...`
  - Backups currently only stored locally (7 files in /opt/openclaw/backups/)
- **Awaiting Mission 5** from human (after Missions 1-4 completed)

## Long-term Learnings
- **CWD inheritance trap**: When `sudo -u <user> bash -c "npx ..."` runs from a directory the target user cannot access, `child_process.spawn()` fails with EACCES (exit 243). Always `cd` to an accessible directory first inside the `bash -c` block.
- **OPENCLAW_HOME split-brain**: The CLI writes to `~/.openclaw/` by default, but the gateway reads from `$OPENCLAW_HOME/.openclaw/`. Channel and plugin configs MUST be written to the OPENCLAW_HOME path or they won't be seen by the running service.
- **Gateway command is `gateway`**: The OpenClaw CLI uses `openclaw gateway` to start the service. `openclaw start` does not exist.
- **Shellcheck in CI**: The CI workflow installs shellcheck and runs it on all .sh files. Local dev environments may not have it, causing test count discrepancies (447 local vs 478 CI).

## Automated Learnings
<!-- Auto-populated by outcome-learner.sh (daily at 2 AM) -->
<!-- Each entry: [timestamp] Learning cycle: X/Y succeeded (Z%), avg Nms -->
<!-- Oldest entries are pruned when this section exceeds 30 lines -->

## Important Notes
- **Bot token**: Telegram bot token is in openclaw.json — do NOT commit to git
- **PAT tokens**: Temporary GitHub PATs should be deleted after use; never persist in config
- **Report convention**: All mission reports go to /opt/openclaw/workspace/reports/<name>-<NNN>.md
- **Mission numbering**: Missions auto-increment in ops_missions. Next mission will be ID 5.
- **Agent assignments**: Check metadata.assigned_agent on each mission for primary owner

## Mission 5 COMPLETED - Dashboard WebSocket Fix
**Date:** 2026-02-12
**Owner:** MilkBot (#001)

### Issue
- Dashboard showing raw HTML instead of React-rendered app
- WebSocket connection issues through cloudflared tunnel

### Fixes Applied
1. Created `/opt/openclaw/dashboard/.streamlit/config.toml` with WebSocket optimization
2. Restarted dashboard service with new configuration
3. Validated all endpoints through cloudflared tunnel

### Validation Results
- ✅ Dashboard URL: https://dashboard.milkbot.ai (HTTP/2 200)
- ✅ WebSocket Health: /_stcore/health returns "ok"
- ✅ Static Assets: JS, CSS, fonts loading correctly
- ✅ React App: Bootstrapping via index.Drusyo5m.js

### Report
- Location: `/opt/openclaw/workspace/reports/dashboard-websocket-fix.md`

### Pending
- **Google Drive Backup Setup** ⭐ HIGH PRIORITY (requires manual sudo)

## Mission 6 COMPLETED - Dashboard Header Audit
**Date:** 2026-02-12
**Owner:** Team (MilkBot + Atlas + Scout)

### Auditor's Claim
- "Dashboard returning raw HTML due to wrong Content-Type headers"
- "Content-Type is text/plain or application/octet-stream"

### Our Findings
- Content-Type is CORRECT: `text/html` ✅
- All static assets have correct MIME types ✅
- HTML structure is valid ✅
- Cloudflare proxying correctly ✅

### Root Cause Analysis
The auditor's findings do NOT match current system state. Possible explanations:
1. Browser cache with old/stale content
2. Temporary issue that's now resolved
3. Network-level caching (proxy/ISP)
4. Browser extension interference

### Verification Results
| Test | Result |
|------|--------|
| Main page Content-Type | `text/html` ✅ |
| JS bundle Content-Type | `application/javascript` ✅ |
| CSS Content-Type | `text/css` ✅ |
| HTML Structure | Valid DOCTYPE + React root ✅ |
| Cloudflare Headers | No modifications ✅ |

### Verdict
🎯 **DASHBOARD IS WORKING CORRECTLY**

### Recommendations
If users see raw HTML:
1. Clear browser cache
2. Hard refresh (Ctrl+Shift+R)
3. Test in incognito mode

### Report
- Location: `/opt/openclaw/workspace/reports/dashboard-audit-006.md`
