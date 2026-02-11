# TOOLS — Integrations

## LLM Providers
- **MiniMax M2.1**: Primary coding — Coding Plan Plus ($20/mo, 300 req/5hrs)
- **DeepSeek v3.2**: Validation/overflow via OpenRouter ($25/month)

## Search
- **Brave Search**: Quick facts (2,000/month free tier)
- **Perplexity Pro**: Deep research (credit-based)

## Databases
- **Redis**: Inter-agent pub/sub, task queues, heartbeats, state storage (localhost:6379, 2GB max)
- **PostgreSQL**: Persistent state — heartbeats, task queue, event log (localhost:5432, database: binaryrogue)

## Communications
- **Telegram**: Alerts via @Milkbot420 (one-way, rate-limited)
- **GitHub**: milkbotai/Milkbot.git (daily auto-commit via github-commit.sh)
- **Gmail**: iambinaryrogue@gmail.com (configured, not yet automated)
- **Google Drive**: Backup uploads every 6 hours (30-day retention)

## Infrastructure
- **OpenClaw**: AI gateway — LLM routing, agent orchestration (v2026.2.9)
- **Cloudflare**: Tunnel to dashboard.milkbot.ai (public access, no auth)
- **Streamlit**: Dashboard on port 8501

## Monitoring
- **health-check.sh**: API connectivity, disk, memory, services (every 30min)
- **resource-monitor.sh**: CPU, RAM, disk thresholds (every 30min via cron)
- **alert-telegram.sh**: Rate-limited Telegram notifications
