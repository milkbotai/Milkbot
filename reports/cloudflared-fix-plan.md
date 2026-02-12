# Cloudflared Tunnel Fix Plan

## Problem Statement
cloudflared tunnel failing to restart 4+ times in 24 hours
- Impact: Dashboard inaccessible (dashboard.milkbot.ai)
- Current uptime: ~95% (target: 99%+)

---

## Diagnostic Steps (15 min)

### Step 1: Check Service Logs
```bash
journalctl -u cloudflared -n 50 --no-pager
```
**Look for:** Errors, credential issues, network failures

### Step 2: Verify Configuration
```bash
cat /etc/cloudflared/config.yml
```
**Check:** Tunnel ID, credentials file path, ingress rules

### Step 3: Test Tunnel Manually
```bash
cloudflared tunnel --config /etc/cloudflared/config.yml --dry-run
```

### Step 4: Check Network/Firewall
```bash
iptables -L -n | grep cloudflare
netstat -tulpn | grep cloudflared
```

### Step 5: Verify Credentials
```bash
ls -la /etc/cloudflared/
cat ~/.cloudflared/*.json 2>/dev/null
```

---

## Common Fixes by Symptom

| Symptom | Likely Cause | Fix |
|---------|--------------|-----|
| "token expired" | Credentials stale | Regenerate tunnel token |
| "connection refused" | Network/firewall | Open port 7844 |
| "permission denied" | File ownership | `sudo chown cloudflared:cloudflared /etc/cloudflared/*` |
| "unknown tunnel" | Config error | Verify tunnel ID in config |
| Crash on start | Resource issue | Check memory, increase limits |

---

## Fix Implementation (45-90 min)

### Option A: Credentials Issue (Most Common)
```bash
# 1. Get new token from Cloudflare Zero Trust
# 2. Update credentials
sudo nano /etc/cloudflared/creds.json

# 3. Restart service
sudo systemctl restart cloudflared

# 4. Verify
systemctl status cloudflared
journalctl -u cloudflared -n 10
```

### Option B: Service Configuration
```bash
# 1. Edit service file
sudo nano /etc/systemd/system/cloudflared.service

# 2. Add restart policy
[Service]
Restart=always
RestartSec=10

# 3. Reload and restart
sudo systemctl daemon-reload
sudo systemctl restart cloudflared
```

### Option C: Network/Firewall
```bash
# 1. Allow cloudflared ports
sudo ufw allow 7844/tcp
sudo ufw allow 443/tcp

# 2. Or check existing rules
sudo iptables -L INPUT -v -n | grep 7844
```

---

## Verification Checklist

- [ ] `systemctl status cloudflared` shows "active (running)"
- [ ] `journalctl -u cloudflared` shows no errors
- [ ] Dashboard accessible via dashboard.milkbot.ai
- [ ] Tunnel shows "HEALTHY" in Cloudflare dashboard
- [ ] No restart failures in logs for 1 hour

---

## Rollback Plan

If fix fails:
```bash
# Restore previous config
sudo cp /etc/cloudflared/config.yml.bak /etc/cloudflared/config.yml
sudo systemctl restart cloudflared
```

Backup current config first:
```bash
sudo cp /etc/cloudflared/config.yml /etc/cloudflared/config.yml.bak
```

---

## Estimated Time

| Phase | Duration |
|-------|----------|
| Diagnostics | 15 min |
| Fix implementation | 45-90 min |
| Verification | 15 min |
| **Total** | **1h 15m - 2h** |

---

## Post-Fix Monitoring

Add to health-check.sh:
```bash
# Check cloudflared tunnel health
if ! curl -s --max-time 5 https://dashboard.milkbot.ai > /dev/null; then
    echo "WARN: Dashboard unreachable"
    # Trigger alert
fi
```

---

*Plan ready for execution. Start with diagnostic steps.*
