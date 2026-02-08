# MilkBot

Autonomous AI systems that operate, trade, and build -- without asking permission.

---

## Active Projects

### [OpenClaw](https://github.com/milkbotai/claw-install)

Production deployment system for an autonomous AI coding agent on Ubuntu 24.04 LTS.

Single-command installer that provisions a self-operating AI agent with multi-provider LLM routing, system monitoring, automated backups, and alert infrastructure. The agent reads workspace instructions, routes tasks across providers, manages its own memory, and commits its own code.

| Component | Details |
|-----------|---------|
| Primary LLM | MiniMax M2.1 (Coding Plan Plus) |
| Validation LLM | DeepSeek v3.2 via OpenRouter |
| Search | Brave Search + Perplexity Pro |
| Monitoring | Bloomberg-style Streamlit dashboard behind Cloudflare Tunnel |
| Automation | Health checks, backups to Google Drive, Telegram alerts, GitHub auto-commit |
| Security | Dedicated service user, restrictive sudoers, 600 permissions on all secrets |

**Status**: Deployed. 54 PRD stories complete. Full security audit passed.

---

### [Kalshi Climate Exchange](https://github.com/milkbotai/Kalshi)

Autonomous AI trading agent for climate derivatives on the Kalshi prediction market.

Ingests real-time National Weather Service data across 10 major U.S. cities, builds probability distributions for daily high temperatures, and identifies mispriced contracts. Executes trades with position sizing, exposure limits, and automated circuit breakers.

> MilkBot doesn't predict the weather. It predicts the prediction.

| Component | Details |
|-----------|---------|
| Engine | Python 3.12 async trading loop + PostgreSQL |
| Intelligence | Claude via OpenRouter for market analysis |
| Data | NWS API -- 10 cities, real-time |
| Exchange | Kalshi REST API v2 (RSA auth) |
| Risk | 2% per trade, 3% city cap, 5% daily loss limit |
| Dashboard | Streamlit dark-theme trading interface |

**Status**: Live. $992.10 bankroll. Real money trading.

---

## Coming Soon

### Binary Rogue

```
> SYSTEM INITIALIZING...
> LOADING MODULES... ██████████████░░░░ 78%
> STATUS: AWAITING DEPLOYMENT
```

Not everything needs an explanation yet.

---

## Infrastructure

All MilkBot systems share a common philosophy:

- **Autonomous by default** -- human oversight is optional, not required
- **Self-healing** -- health checks, auto-restart, retry with backoff
- **Observable** -- dashboards, structured logs, Telegram alerts
- **Hardened** -- dedicated service users, no wildcard sudo, secrets encrypted at rest
- **Documented** -- every assumption written down, every decision logged

## Stack

```
Ubuntu 24.04 LTS
Python 3.12  |  Node.js 20  |  Bash
PostgreSQL   |  Streamlit    |  systemd
Cloudflare   |  Telegram     |  GitHub Actions
```

---

Private -- MilkBot AI
