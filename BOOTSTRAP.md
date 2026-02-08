# BOOTSTRAP - Runtime Initialization

## Initialization Sequence

On service start, the following steps execute via systemd:

1. **Load environment** - `.env` sourced by EnvironmentFile directive
2. **Auto-resume check** - `auto-resume.sh` runs as ExecStartPre:
   - Validates workspace files exist (SOUL.md, IDENTITY.md, AGENTS.md, MEMORY.md)
   - Detects IN_PROGRESS and QUEUED tasks in MEMORY.md
   - Logs task counts (detection only - actual resumption handled by OpenClaw agent)
3. **Start OpenClaw agent** - Main service process launches
4. **Health check** - Runs on timer every 30 minutes:
   - Validates openclaw systemd service status
   - Tests MiniMax and OpenRouter API connectivity
   - Checks disk usage (>80% warn, >90% critical)
   - Checks available memory (<256MB critical)
   - Validates workspace files present

## Known Limitations
- Task resumption is detection-only; the OpenClaw agent reads MEMORY.md to decide what to resume
- No time-based resume gating (the "<30min" concept is not enforced)
- No explicit "service ready" marker; systemd manages readiness
