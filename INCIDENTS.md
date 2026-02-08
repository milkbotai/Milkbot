# INCIDENTS - Known Issues & Recovery

## Known Issues

### Service Won't Start
**Symptoms**: systemctl shows failed
**Recovery**: Check logs, verify .env, fix permissions

### API Quota Exhausted
**Symptoms**: 429 errors
**Recovery**: Wait for reset, use DeepSeek

### Backup Failures
**Symptoms**: Drive upload errors
**Recovery**: Verify OAuth, reauth if needed

## Incident Template
```markdown
## YYYY-MM-DD - Title
**Severity**: Critical | High | Medium | Low
**Duration**: [Start] to [End]

### Root Cause
[What happened]

### Resolution
[How fixed]

### Prevention
[Avoid future]
```
