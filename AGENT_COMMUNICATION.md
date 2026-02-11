# Agent Communication Protocol (ACP)

## Binary Rogue Inter-Agent Communication Layer

**Status:** Active
**Infrastructure:** Redis (pub/sub, task queue) + PostgreSQL (persistent state)

---

## 1. Redis Channels

### Pub/Sub

| Channel | Purpose | Publishers | Subscribers |
|---------|---------|------------|-------------|
| `binaryrogue:heartbeats` | Agent liveness (30s interval) | All agents | Atlas |
| `binaryrogue:alerts` | Critical alerts | Atlas | MilkBot, all agents |
| `binaryrogue:signals` | Market signals | Scout | MilkBot |
| `binaryrogue:decisions` | Strategic decisions | MilkBot | All agents |
| `binaryrogue:tasks` | Task distribution | All agents | All agents |
| `binaryrogue:logs` | Structured log events | All agents | Atlas |

### State Storage (Redis Hashes)

| Key Pattern | Type | Purpose |
|-------------|------|---------|
| `binaryrogue:agents:{agent_id}` | Hash | Agent metadata, status, last_seen |
| `binaryrogue:budget:{agent_id}:{provider}` | String | Per-agent API quota usage |
| `binaryrogue:positions:{venue}` | Hash | Active trading positions |

### Connection

```
Host: 127.0.0.1
Port: 6379
Max Memory: 2GB
Eviction: allkeys-lru
Persistence: AOF enabled
```

---

## 2. PostgreSQL Schema

Database `binaryrogue` stores persistent state that survives Redis restarts.

### Tables

See database/schema/002_ops_missions.sql for the current schema. Tables: ops_missions, ops_mission_steps, ops_policy, ops_agent_events, ops_outcomes, ops_reaction_rules.

### Connection

```
Host: 127.0.0.1
Port: 5432
Database: binaryrogue
User: binaryrogue
```

---

## 3. Communication Patterns

### Fire-and-Forget (Pub/Sub)

For real-time notifications that don't need persistence. If no subscriber is listening, the message is lost.

```python
# Broadcast a critical alert
redis.publish("binaryrogue:alerts", json.dumps({
    "level": "critical",
    "source": "agent:002",
    "message": "MilkBot missed 3 heartbeats",
    "timestamp": datetime.utcnow().isoformat()
}))
```

### Task Queue (Redis List)

For work items that must be processed exactly once, even if the consumer restarts.

```python
# Push a task
redis.lpush("binaryrogue:tasks", json.dumps({
    "task_id": str(uuid.uuid4()),
    "sender": "agent:001",
    "receiver": "agent:003",
    "action": "scan_market",
    "payload": {"venue": "kalshi"},
    "priority": "high",
    "status": "pending",
    "created_at": int(time.time())
}))

# Worker pops tasks (blocking)
task = redis.brpop("binaryrogue:tasks", timeout=0)
```

### Persistent Events (PostgreSQL)

For audit trails and queries against historical data.

```python
# Log an agent event
cursor.execute("""
    INSERT INTO agent_events (agent_name, event_type, payload)
    VALUES (%s, %s, %s)
""", ("milkbot", "task_completed", json.dumps({
    "task": "deploy-dashboard",
    "duration_seconds": 45
})))
```

---

## 4. Heartbeat Protocol

Every agent sends a heartbeat every **30 seconds**.

```python
def send_heartbeat(agent_id, status="alive"):
    # Redis: real-time presence
    redis.hset(f"binaryrogue:agents:{agent_id}", mapping={
        "status": status,
        "last_seen": datetime.utcnow().isoformat(),
        "uptime_seconds": str(get_uptime())
    })
    redis.publish("binaryrogue:heartbeats", json.dumps({
        "agent_id": agent_id,
        "status": status,
        "timestamp": datetime.utcnow().isoformat()
    }))

    # PostgreSQL: persistent history
    cursor.execute("""
        INSERT INTO agent_heartbeats (agent_name, status, metadata)
        VALUES (%s, %s, %s)
    """, (agent_id, status, json.dumps({"uptime": get_uptime()})))
```

**Timeout:** 90 seconds (3 missed heartbeats = agent down → Atlas escalates)

---

## 5. Budget Tracking

```python
def track_usage(agent_id, provider, cost):
    key = f"binaryrogue:budget:{agent_id}:{provider}"
    redis.incrbyfloat(key, cost)

    usage = float(redis.get(key) or 0)
    limit = BUDGET_LIMITS[provider]

    if usage > limit * 0.9:
        redis.publish("binaryrogue:alerts", json.dumps({
            "level": "warning",
            "source": agent_id,
            "message": f"Near {provider} limit: ${usage:.2f}/${limit}"
        }))
```

---

## 6. Message Flow

```
MilkBot ──── tasks ────► Scout (research requests)
Scout ────── signals ───► MilkBot (market intelligence)
Atlas ────── alerts ────► MilkBot (system issues)
All ──────── heartbeats ► Atlas (liveness monitoring)
MilkBot ──── decisions ─► All (strategic direction)
All ──────── events ────► PostgreSQL (audit trail)
```

---

*Communication is infrastructure. If agents can't talk, they can't coordinate. If they can't coordinate, they can't compound.*
