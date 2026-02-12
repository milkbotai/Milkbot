# Missions

Mission system for structured task orchestration. Missions are stored in PostgreSQL (`ops_missions` / `ops_mission_steps` tables) and tracked here for agent context.

## Active Missions

| ID | Title | Priority | Agent | Steps | Status |
|----|-------|----------|-------|-------|--------|
| 1 | Repository Deep Audit & Documentation Sync | 10 | Atlas | 0/7 | approved |
| 2 | Competitive Intelligence & Market Analysis | 8 | Scout | 0/6 | approved |
| 3 | System Health & Optimization Testing | 7 | MilkBot + Atlas | 0/9 | approved |

### Mission 1: Repository Deep Audit & Documentation Sync
**Agent:** Atlas (Resilience Engineer) | **Priority:** 10 | **Deadline:** 08:00 UTC
**Report:** `/opt/openclaw/workspace/reports/audit-001.md`

| Step | Kind | Title |
|------|------|-------|
| 1 | health_check | Pre-audit system health baseline |
| 2 | code_generation | Scan codebase for technical debt markers |
| 3 | code_generation | Cross-reference documentation against code |
| 4 | code_generation | Detect configuration drift: docs vs deployed |
| 5 | code_generation | Generate prioritized fix list |
| 6 | documentation | Update ARCHITECTURE.md with current state |
| 7 | documentation | Compile final audit report and update MEMORY.md |

### Mission 2: Competitive Intelligence & Market Analysis
**Agent:** Scout (Intelligence Officer) | **Priority:** 8
**Report:** `/opt/openclaw/workspace/reports/competitive-intel-001.md`

| Step | Kind | Title |
|------|------|-------|
| 1 | code_generation | Profile top 10 autonomous AI agent platforms |
| 2 | code_generation | GitHub activity & community sentiment analysis |
| 3 | code_generation | Gap analysis: OpenClaw vs top 5 competitors |
| 4 | code_generation | Extract production deployment best practices |
| 5 | code_generation | Social channel monitoring & mention tracking |
| 6 | documentation | Compile comparison matrix & strategic recommendations |

### Mission 3: System Health & Optimization Testing
**Agent:** MilkBot (lead) + Atlas (support) | **Priority:** 7
**Report:** `/opt/openclaw/workspace/reports/perf-baseline-001.md`

| Step | Kind | Title |
|------|------|-------|
| 1 | health_check | Baseline system health snapshot |
| 2 | log_analysis | Analyze recent logs for pre-existing issues |
| 3 | backup | Backup verification run 1 of 3 |
| 4 | backup | Backup verification run 2 of 3 |
| 5 | backup | Backup verification run 3 of 3 |
| 6 | testing | Run full test suite and validate CI parity |
| 7 | code_generation | PostgreSQL mission queue performance analysis |
| 8 | health_check | Post-test system health comparison |
| 9 | documentation | Compile performance report with optimization plan |

## Proposed Missions
<!-- None pending -->

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
