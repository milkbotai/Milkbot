# INTEGRATIONS — OAuth, Databases & Webhooks

## Databases

### Redis
- Host: 127.0.0.1:6379
- Max memory: 2GB (allkeys-lru eviction)
- Persistence: AOF enabled
- Used for: pub/sub, task queues, heartbeats, state storage

### PostgreSQL
- Host: 127.0.0.1:5432
- Database: binaryrogue
- User: binaryrogue
- Tables: ops_missions, ops_mission_steps, ops_policy, ops_agent_events, ops_outcomes, ops_reaction_rules
- Used for: persistent state, audit trail, task history

## OAuth

### Google Drive
- Credentials: /opt/openclaw/config/google-credentials.json
- Scopes: drive.file, drive.appdata

### Gmail
- Auth: App-specific password
- Stored: GMAIL_APP_PASSWORD env var

### Cloudflare
- Token: CF_API_TOKEN env var
- Permissions: Tunnel Edit, DNS Edit

## Webhooks
(None configured yet)
