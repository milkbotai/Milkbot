# ESCALATION - Approval Workflows

These levels are guidelines for the OpenClaw agent's behavior. They are not
enforced by code gates - the agent is expected to self-classify actions and
follow these rules based on its SOUL.md instructions.

## Levels

### Level 0: Autonomous
- Bug fixes, refactoring, documentation, maintenance
- No approval needed; agent proceeds independently

### Level 1: Approval Required
- New features, architecture changes, external APIs, budget >$10
- Agent should pause and request approval via Telegram alert

### Level 2: Alert Then Proceed
- Performance issues, health check failures, budget warnings
- Agent sends Telegram notification then continues

### Level 3: Emergency Halt
- Security breach, corruption, budget exceeded, crash loops
- Agent sends alert and stops all work

## Approval Methods

| Method | Status | Notes |
|--------|--------|-------|
| Telegram alert (one-way) | Implemented | `alert-telegram.sh` sends notifications |
| Telegram commands (`/approve`) | Not implemented | Would require polling daemon or webhook |
| Dashboard approval button | Not implemented | Dashboard is read-only monitoring |
| Direct edit of MEMORY.md | Available | User can SSH in and edit directly |

## Current Implementation
The only enforced mechanism is Telegram **alerting** (one-way). The agent
classifies its own actions and sends alerts for Level 2+. For Level 1 tasks
requiring approval, the agent pauses and the user responds via direct
MEMORY.md edit or by restarting the service after review.
