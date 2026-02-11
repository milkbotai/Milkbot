# Missions

Mission system for structured task orchestration. Missions are stored in PostgreSQL (`ops_missions` / `ops_mission_steps` tables) and tracked here for agent context.

## Active Missions
<!-- Missions currently in_progress. Format:
| ID | Title | Priority | Steps Done | Status |
|----|-------|----------|------------|--------|
-->

## Proposed Missions
<!-- Missions awaiting approval. Format:
| ID | Title | Created By | Proposed At |
|----|-------|------------|-------------|
-->

## Recently Completed
<!-- Last 10 completed missions. Format:
| ID | Title | Duration | Outcome |
|----|-------|----------|---------|
-->

## Mission Workflow

1. **Proposed** - Agent or human creates a mission
2. **Approved** - Human approves (or auto-approved if all steps are trusted kinds)
3. **In Progress** - Steps are claimed and executed by agents
4. **Completed/Failed** - All steps done or mission aborted

## Step Claiming

Steps are claimed atomically via `claim_mission_step(step_id, agent_id)`. Claims expire after 10 minutes if not completed (prevents deadlocks from crashed agents).

## Quotas

Daily quotas are enforced per step kind. Check with `check_quota(kind)` before claiming. See POLICIES.md for current limits.
