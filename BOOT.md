# BOOT - Startup Sequence

## System Boot
1. Ubuntu boots
2. Systemd starts multi-user.target
3. `auto-resume.sh` runs (ExecStartPre) - validates workspace, detects incomplete tasks
4. OpenClaw main service starts (`npx openclaw start`)
5. Dashboard service starts independently (port 8501)
6. Health check timer activates (first run at 5min, then every 30min)
7. Backup timer activates (first run at 10min, then every 6h)
8. Resource monitor cron runs every 30 minutes

Note: Steps 5-8 are independent systemd units/cron jobs, not sequentially
ordered after the OpenClaw service. Task resumption is handled by the
OpenClaw agent reading MEMORY.md, not by auto-resume.sh.
