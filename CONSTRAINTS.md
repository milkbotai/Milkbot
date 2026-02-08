# CONSTRAINTS - Budget & Rate Limits

## API Limits
- **MiniMax**: 300 prompts/5hrs (Coding Plan Plus subscription)
- **OpenRouter**: $25/month (DeepSeek v3.2 validation)
- **Brave**: 2,000/month free tier
- **Perplexity**: Credit-based

Note: API quota tracking and spend alerts are not yet automated.
The agent should self-monitor usage based on these limits.

## Operational
- Max MEMORY.md size: 10MB (enforced by memory-prune.sh)
- Backup retention: 30 local copies (enforced by backup-to-drive.sh)
- Log retention: 30 days (enforced by logrotate)
- Max task duration: 4 hours (guideline, not enforced by code)
