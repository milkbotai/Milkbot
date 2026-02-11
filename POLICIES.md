# Policies

System policies governing mission approval, quotas, retries, and circuit breakers. Policies are stored in PostgreSQL (`ops_policy` table) and summarized here for agent context.

## Auto-Approve Rules

Step kinds that execute without human approval:
- `health_check` - routine health validation
- `memory_prune` - agent memory cleanup
- `backup` - scheduled backup operations
- `git_commit` - automated workspace commits
- `status_report` - system status generation

All other step kinds require explicit human approval before execution.

## Daily Quotas

| Step Kind | Daily Limit | Purpose |
|-----------|-------------|---------|
| `web_search` | 50 | Prevent excessive API usage |
| `api_call` | 200 | Rate limit external service calls |
| `git_commit` | 20 | Prevent commit spam |

Quotas reset at midnight UTC. Check remaining quota with `check_quota('kind')`.

## Retry Limits

| Step Kind | Max Retries |
|-----------|-------------|
| Default | 3 |
| `deploy` | 1 |
| `backup` | 5 |

Failed steps are retried up to the limit. After exhausting retries, the step is marked `failed` and the mission may need human intervention.

## Circuit Breaker

- **Failure threshold**: 5 consecutive failures
- **Recovery timeout**: 300 seconds (5 minutes)

When the circuit breaker trips, all new step claims are paused until the recovery timeout elapses. This prevents cascading failures.

## Modifying Policies

Policies are stored in the `ops_policy` table:

```sql
-- View all policies
SELECT key, value, description FROM ops_policy;

-- Update a quota
UPDATE ops_policy SET value = '{"daily_limit": 100}', updated_at = NOW()
WHERE key = 'quota:web_search';

-- Add a new auto-approve kind
UPDATE ops_policy
SET value = jsonb_set(value, '{kinds}', value->'kinds' || '["new_kind"]'::jsonb),
    updated_at = NOW()
WHERE key = 'auto_approve_kinds';
```
