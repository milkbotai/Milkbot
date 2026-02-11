# ESCALATION — Approval Workflows

> See also: AGENTS.md § Escalation Matrix for the quick-reference table.

These levels are guidelines for agent behavior. They are not enforced by code
gates — agents self-classify actions and follow these rules per SOUL.md.

## Levels

### Level 0: Autonomous
- Bug fixes, refactoring, documentation, maintenance, health self-healing
- No approval needed — proceed independently
- Log what was done in MEMORY.md

### Level 1: Approval Required
- New features, architecture changes, external API integrations, budget > $10
- Pause and request approval via Telegram alert
- Wait for owner response before proceeding

### Level 2: Alert Then Proceed
- Performance degradation, health check warnings, budget approaching 75%
- Send Telegram notification, then continue working
- Log the alert and any action taken

### Level 3: Emergency Halt
- Security breach, data corruption, budget exceeded, crash loop (3+ restarts)
- Send Telegram alert immediately
- Stop all non-critical work until owner responds

## Approval Methods

| Method | Status | Notes |
|--------|--------|-------|
| Telegram alert (one-way) | Active | `alert-telegram.sh` sends notifications |
| Telegram commands (`/approve`) | Not implemented | Would need polling daemon or webhook |
| Dashboard approval button | Not implemented | Dashboard is read-only monitoring |
| Direct edit of MEMORY.md | Available | Owner SSHs in and edits directly |
| Redis task queue | Available | Owner can push approval via `binaryrogue:tasks` |

## Current State
The only enforced mechanism is Telegram **alerting** (one-way). Agents
self-classify their actions and send alerts for Level 2+. For Level 1 tasks
requiring approval, the agent pauses and the owner responds by editing
MEMORY.md or restarting the service after review.
