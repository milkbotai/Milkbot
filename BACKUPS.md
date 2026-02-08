# BACKUPS - Backup & Restore

## Schedule
- **Frequency**: Every 6 hours (systemd timer)
- **Destination**: Google Drive /Openclaw_Backups/milkbot/ (via gdrive or rclone)
- **Local retention**: Last 30 backups in /opt/openclaw/backups/

## What's Backed Up
Archive includes (from /opt/openclaw):
- `workspace/` - All .md files, memory/ folder
- `config/` - Provider configs, systemd units, logrotate, sudoers (excludes .env, drive-credentials.json)
- `scripts/` - All runtime scripts

Excludes: `logs/`, `backups/`, `venv/`, `bin/`

## Restore
```bash
# List local backups
ls -lh /opt/openclaw/backups/

# Extract a specific backup
tar -xzf /opt/openclaw/backups/openclaw_backup_YYYYMMDD_HHMMSS.tar.gz -C /tmp/openclaw-restore/

# Copy to installation
cp -r /tmp/openclaw-restore/* /opt/openclaw/

# Restart service
sudo systemctl restart openclaw
```

## Manual Backup
```bash
sudo -u milkbot /opt/openclaw/scripts/backup-to-drive.sh
```
