# HEARTBEAT - Automated Maintenance

## Health Check (every 30 minutes via systemd timer)
Runs `health-check.sh`:
1. Service status (systemctl is-active openclaw)
2. API connectivity (MiniMax, OpenRouter, Brave, Perplexity)
3. Telegram bot reachability
4. Disk usage (>80% warn, >90% critical)
5. Memory available (<256MB critical)
6. Workspace file presence

## Resource Monitor (every 30 min via cron)
Runs `resource-monitor.sh`:
1. Disk usage
2. RAM usage
3. CPU load

## Not Implemented
These checks are planned but not yet built:
- Active task status tracking
- API quota/spend tracking
- Backup verification
- Error log scanning
- File integrity (checksums)
- GitHub sync status
