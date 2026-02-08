# Project Index

Detailed technical overview of all MilkBot projects.

## 1. OpenClaw -- Autonomous Coding Agent

**Repo**: [milkbotai/claw-install](https://github.com/milkbotai/claw-install)

### What It Does

OpenClaw is a production installer that deploys an autonomous AI coding agent on a fresh Ubuntu 24.04 server. One command, 11 steps, fully operational in under 15 minutes.

The agent:
- Reads workspace `.md` files for instructions and personality
- Routes coding tasks to MiniMax M2.1 (primary) and DeepSeek v3.2 (validation)
- Manages its own memory (`MEMORY.md`) with task state tracking
- Commits its own code to GitHub daily
- Self-monitors via health checks every 6 hours
- Backs up to Google Drive every 6 hours
- Sends Telegram alerts when something breaks

### Architecture

```
install.sh (root, 11 steps)
  -> preflight  -> user setup  -> API wizard
  -> integrations (SSH, Gmail, Drive)
  -> OpenClaw npm install  -> workspace deploy
  -> Streamlit dashboard  -> backups
  -> Cloudflare tunnel  -> systemd services
  -> validation (50+ checks)
```

### Provider Routing

| Provider | Role | Billing |
|----------|------|---------|
| MiniMax M2.1 | Primary coding model | $20/mo subscription |
| DeepSeek v3.2 (OpenRouter) | Validation / overflow | $25/mo credits |
| Brave Search | Quick web lookups | Free tier (2K/mo) |
| Perplexity Pro | Deep research | Credit-based |

### Key Metrics

- 54 PRD stories implemented
- 4-phase security audit completed (GO recommendation)
- 409 tests passing (297 static + 18 integration + 94 dashboard)
- 17 workspace personality files
- 7 runtime scripts (health, backup, alert, commit, monitor, prune, resume)

---

## 2. Kalshi Climate Exchange -- Autonomous Trading Agent

**Repo**: [milkbotai/Kalshi](https://github.com/milkbotai/Kalshi)

### What It Does

An autonomous AI agent that trades climate derivatives (temperature contracts) on the Kalshi prediction market. It finds the gap between what the weather will actually do and what the market thinks it will do.

The agent:
- Pulls real-time NWS forecast data for 10 U.S. cities
- Builds temperature probability distributions
- Scans Kalshi for mispriced YES/NO contracts
- Executes trades with strict position sizing
- Tracks P&L in PostgreSQL
- Displays everything on a live Streamlit dashboard

### Risk Management

| Parameter | Limit |
|-----------|-------|
| Per-trade risk | 2% of bankroll |
| City exposure | 3% max |
| Daily loss cap | 5% |
| Circuit breaker | Auto-halt on repeated API failures |
| Bankroll | $1,500 |

### Tech Stack

- Python 3.12 (async trading engine)
- PostgreSQL (trade history, P&L tracking)
- Kalshi REST API v2 (RSA key authentication)
- NWS API (meteorological data)
- OpenRouter / Claude (market analysis)
- Streamlit (dark-theme trading dashboard)
- Nginx + Cloudflare (infrastructure)

---

## 3. Binary Rogue -- [CLASSIFIED]

**Status**: Pre-deployment

Details restricted. Watch this space.

---

## Common Infrastructure

All projects deploy on Ubuntu 24.04 LTS VPS instances with:

- **systemd** for service management and scheduling
- **Cloudflare** for tunnels and DNS
- **Telegram** for operational alerts
- **Streamlit** for dashboards
- **GitHub** for version control and automated commits
- Dedicated service users with hardened sudo policies
