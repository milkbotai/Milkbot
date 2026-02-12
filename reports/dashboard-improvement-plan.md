# OpenClaw Dashboard Improvement Plan

**Plan ID:** `dashboard-improvement-plan-001`
**Date:** 2026-02-12
**Prepared By:** MilkBot (CEO)
**Status:** PLAN ONLY - Do not implement until approved

---

## Executive Summary

This document outlines a comprehensive plan to transform the OpenClaw dashboard from its current "version 1.0 amateur" state into a professional, real-time mission control center. The plan addresses visual design, information architecture, real-time updates, and agent visibility.

**Current State Assessment:** Functional but basic. Dark theme exists but lacks visual hierarchy, real-time updates, and agent-centric information.

**Target State:** Professional mission control with real-time agent monitoring, clear metrics, and scannable layout.

---

## 1. Current Dashboard Audit

### 1.1 Visual Design Assessment

| Aspect | Current State | Score | Issue |
|--------|---------------|-------|-------|
| Color Palette | Orange/Black/Blue/Green | 5/10 | Limited contrast, no accent hierarchy |
| Typography | Inter + SF Mono + Serif | 6/10 | Good but inconsistent sizing |
| Layout | 4-quadrant grid | 4/10 | Too static, no visual hierarchy |
| Visual Effects | Basic pulse animation | 3/10 | Missing transitions, hover states |
| Brand Identity | "MilkBot" centered | 5/10 | Lacks professional polish |
| Spacing | Inconsistent gaps | 4/10 | Panel heights vary |

### 1.2 Information Architecture

| Section | Current Content | Problem |
|---------|-----------------|---------|
| Hero | Brand name + tagline | Takes too much vertical space |
| Quote | Mencken quote | Decorative, low value |
| System Status | 5 services + circuit breaker | Good, but no trends |
| Live Operations | Log stream | Too noisy, hard to parse |
| Resource Monitor | CPU/RAM/Disk rings | Good visualization |
| Mission Timeline | Event stream | Disorganized |
| Token Usage | Metrics bar | Good, but static |
| Mission Queue | Status counts | Good foundation |

### 1.3 Technical Assessment

| Component | Status | Issue |
|-----------|--------|-------|
| Refresh Mechanism | Manual (30s config) | Not implemented in code |
| Real-time Data | Log file polling | High latency |
| Redis Integration | Available, unused | Opportunity missed |
| PostgreSQL Data | Available, unused | Opportunity missed |
| WebSocket Support | Not implemented | Needed for true real-time |
| Caching | None | Causes performance issues |

**Overall Score:** 5.2/10

---

## 2. Design Goals

### 2.1 Primary Objectives

1. **Professional Aesthetic** - Move from "hobby project" to "enterprise-grade"
2. **Scannability** - Information visible in <3 seconds
3. **Agent Visibility** - Clear view of what agents are doing
4. **Real-time Updates** - Sub-5-second data refresh
5. **Information Hierarchy** - Critical info prioritized
6. **Trust Signals** - Show system health at a glance

### 2.2 Design Principles

| Principle | Application |
|-----------|-------------|
| **Less is More** | Reduce visual noise, focus on metrics |
| **Progressive Disclosure** | Summary → details on demand |
| **Consistency** | Uniform components, colors, spacing |
| **Feedback** - Animations for state changes |
| **Accessibility** | High contrast, readable fonts |
| **Performance** | Lazy load, efficient refresh |

---

## 3. Proposed Information Architecture

### 3.1 New Dashboard Sections

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  HEADER: Logo | System Status Badge | Time | User                           │
├────────────────┬────────────────────┬─────────────────────┬──────────────────┤
│                │                    │                     │                  │
│  SYSTEM        │  AGENT ACTIVITY    │  RESOURCE MONITOR   │  ALERTS          │
│  OVERVIEW      │  (Real-time)       │  (Visual gauges)    │  (Live feed)     │
│                │                    │                     │                  │
│  • 5 services  │  • Active agents   │  • CPU %            │  • Critical      │
│  • Circuit     │  • Current task    │  • RAM %            │  • Warning       │
│  • Uptime      │  • Progress bar    │  • Disk %           │  • Info          │
│  • Health %    │  • Last activity   │  • Network I/O      │                  │
│                │                    │                     │                  │
├────────────────┴────────────────────┴─────────────────────┴──────────────────┤
│                                                                              │
│  MISSIONS & QUEUE                       │  PERFORMANCE METRICS               │
│                                         │                                   │
│  • Active missions                      │  • Token usage (graph)             │
│  • Queue depth                          │  • Response times                  │
│  • Success rate                         │  • Error rate trend                │
│  • Average duration                     │  • Cost per mission                │
│                                         │                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│  FOOTER: Version | Last Refresh | Quick Actions                           │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 3.2 New Section Details

#### A. Header (Top Bar)
- **Left:** Logo + Environment (PROD/DEV)
- **Center:** Overall system health badge (Green/Yellow/Red)
- **Right:** Server time + Local time + User menu

#### B. System Overview (Top Left)
**Current:** Service list
**Proposed:** High-level health card with:
- Overall health percentage
- Uptime (days:hours:minutes)
- Active services count
- Circuit breaker status
- Quick action buttons (Restart All, Clear Cache)

#### C. Agent Activity (Top Center) - NEW
**Purpose:** Real-time view of agent work

**Components:**
- **Agent Cards:** Each agent with:
  - Name + Avatar
  - Current status (Working/Idle/Error)
  - Current task description
  - Progress bar
  - Elapsed time
  - Token usage this session
- **Activity Feed:** Recent actions per agent
- **State Changes:** Animated transitions

#### D. Resource Monitor (Top Right)
**Current:** Ring gauges
**Proposed:** Enhanced with:
- Trend sparklines (mini graphs)
- Historical context (last 24h)
- Predictive indicators (if trending up)
- Threshold indicators (warn/crit colors)

#### E. Alerts Panel (Middle Right) - NEW
**Purpose:** Live alert feed

**Components:**
- Color-coded severity (Critical/Warning/Info)
- Expandable details
- Dismiss functionality
- Auto-clear after resolution

#### F. Missions & Queue (Bottom Left)
**Current:** Status counts
**Proposed:** Enhanced with:
- Kanban-style view (Queued/In Progress/Done)
- Mission cards with progress
- Priority indicators
- Duration estimates
- Success/fail metrics

#### G. Performance Metrics (Bottom Right)
**Current:** Token usage bar
**Proposed:** Charts with:
- Token usage over time (line chart)
- Response time distribution
- Cost tracking
- Provider breakdown (MiniMax vs OpenRouter)

---

## 4. Real-Time Update Strategy

### 4.1 Architecture Options

| Option | Latency | Implementation | Pros | Cons |
|--------|---------|----------------|------|------|
| **Polling (Current)** | 30s+ | Easy | Simple | Delayed |
| **Auto-Refresh** | 5-10s | Medium | Good UX | Full reload |
| **Server-Sent Events** | <1s | Hard | True real-time | Complex |
| **WebSocket** | <1s | Hardest | Bidirectional | Overkill |
| **Streamlit Components** | 3-5s | Easy | Native support | Limited |

### 4.2 Recommended: Streamlit Auto-Refresh

**Approach:** Use `st_autorefresh` library with intelligent refresh rates

```python
from st_autorefresh import st_autorefresh

# Refresh every 5 seconds
count = st_autorefresh(interval=5000, key="dashboard_refresh")
```

**Benefits:**
- Simple implementation
- No WebSocket complexity
- Good user experience
- Low server load

### 4.3 Data Source Optimization

| Data Source | Current | Proposed | Refresh Rate |
|-------------|---------|----------|--------------|
| Service Status | Polling | Redis pub/sub | 1s |
| Resource Usage | Script run | psutil + cache | 5s |
| Logs | File read | Tail + buffer | 3s |
| Missions | Database query | Cache + refresh | 5s |
| Agent Activity | None | Redis channels | 1s |
| Alerts | File read | In-memory queue | 2s |

---

## 5. Visual Design Improvements

### 5.1 Color Palette Enhancement

| Current | Proposed | Usage |
|---------|----------|-------|
| `#ff6b35` (Orange) | `#ff6b35` | Primary brand |
| `#1e90ff` (Blue) | `#3b82f6` | Secondary/Info |
| `#00ff88` (Green) | `#22c55e` | Success/Good |
| `#ffaa00` (Amber) | `#f59e0b` | Warning |
| `#ff1744` (Crimson) | `#ef4444` | Error/Critical |
| `#000000` | `#0a0a0a` | Background |
| `#111111` | `#171717` | Cards |
| `#e0e0e0` | `#f1f5f9` | Text primary |
| `#888888` | `#94a3b8` | Text secondary |

### 5.2 Typography System

| Element | Font | Size | Weight | Usage |
|---------|------|------|--------|-------|
| Display | Inter | 48px | 700 | Hero titles |
| H1 | Inter | 32px | 600 | Section headers |
| H2 | Inter | 24px | 600 | Subsection |
| Body | Inter | 16px | 400 | Content |
| Caption | Inter | 12px | 400 | Labels |
| Mono | SF Mono | 14px | 400 | Code/Metrics |
| Badge | Inter | 11px | 600 | Tags |

### 5.3 Component Design

#### Card Component
```css
.mc-card {
    background: #171717;
    border: 1px solid #2d2d2d;
    border-radius: 12px;
    padding: 20px;
    transition: all 0.2s ease;
}
.mc-card:hover {
    border-color: #3b82f6;
    transform: translateY(-2px);
    box-shadow: 0 8px 24px rgba(0,0,0,0.4);
}
```

#### Metric Component
```css
.mc-metric {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 16px;
    background: linear-gradient(135deg, #171717, #1a1a1a);
    border-radius: 12px;
}
.mc-metric-value {
    font-family: 'SF Mono', monospace;
    font-size: 28px;
    font-weight: 700;
    color: #f1f5f9;
}
.mc-metric-label {
    font-family: 'Inter', sans-serif;
    font-size: 12px;
    color: #94a3b8;
    text-transform: uppercase;
    letter-spacing: 1px;
}
```

#### Status Indicator
```css
.mc-status {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    padding: 4px 12px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: 600;
}
.mc-status::before {
    content: '';
    width: 8px;
    height: 8px;
    border-radius: 50%;
    animation: pulse 2s infinite;
}
.mc-status.working { background: rgba(34, 197, 94, 0.2); color: #22c55e; }
.mc-status.idle { background: rgba(148, 163, 184, 0.2); color: #94a3b8; }
.mc-status.error { background: rgba(239, 68, 68, 0.2); color: #ef4444; }
```

### 5.4 Animations

| Animation | Duration | Usage |
|-----------|----------|-------|
| Fade in | 300ms | New elements |
| Slide up | 400ms | Cards appearing |
| Pulse | 2s | Status indicators |
| Glow | 1s | Success states |
| Shake | 500ms | Error states |

---

## 6. Agent Activity Section Design

### 6.1 Agent Card Layout

```
┌──────────────────────────────────────────────────────┐
│  🤖 Atlas (Employee #002)                            │
│  Status: 🔄 Working  │  Duration: 12:34              │
├──────────────────────────────────────────────────────┤
│  Current Task:                                       │
│  Scanning logs for pre-existing issues...            │
│  ▓▓▓▓▓▓▓▓▓░░░░░░░  68%                             │
├──────────────────────────────────────────────────────┤
│  Session Stats:                                      │
│  Tokens: 45.2K  │  Steps: 7/12  │  Errors: 0        │
│  Last Activity: 2 seconds ago                        │
└──────────────────────────────────────────────────────┘
```

### 6.2 Agent Status States

| State | Icon | Color | Meaning |
|-------|------|-------|---------|
| Working | 🔄 | Green | Active task execution |
| Idle | 💤 | Gray | Waiting for assignment |
| Error | ❌ | Red | Task failed, needs review |
| Sleeping | 🌙 | Blue | Cooldown/scheduled |
| Initializing | ⏳ | Yellow | Starting up |

### 6.3 Activity Timeline Per Agent

```
Atlas Activity (Last 10 actions)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[12:34:56] ✅ Completed: Log analysis complete
[12:32:10] 🔄 Started: Scanning logs...
[12:30:45] ✅ Completed: VACUUM tables
[12:28:33] 🔄 Started: Database analysis
[12:15:22] ⚠️ Warning: Dead tuples detected
...
```

---

## 7. Implementation Plan

### Phase 1: Foundation (Days 1-3)

| Task | Effort | Description |
|------|--------|-------------|
| Install dependencies | 1h | st_autorefresh, plotly, altair |
| Create CSS variables | 4h | Define design system |
| Refactor layout structure | 8h | New grid system |
| Implement auto-refresh | 4h | 5-second refresh cycle |
| Create card components | 8h | Reusable MC-* components |

**Deliverable:** Basic layout with real-time refresh

### Phase 2: Agent Visibility (Days 4-7)

| Task | Effort | Description |
|------|--------|-------------|
| Agent data integration | 8h | Redis channel for agent updates |
| Agent card component | 8h | Full card with progress |
| Agent activity feed | 6h | Per-agent timeline |
| Status state machine | 4h | Working/Idle/Error logic |
| Agent management page | 8h | Enhanced 3_Agent_Management.py |

**Deliverable:** Full agent visibility

### Phase 3: Visual Polish (Days 8-10)

| Task | Effort | Description |
|------|--------|-------------|
| Color scheme update | 4h | New palette implementation |
| Typography system | 4h | Consistent font sizes |
| Animation system | 6h | Transitions, hover states |
| Charts and graphs | 8h | Performance metrics visualization |
| Responsive design | 4h | Mobile/tablet support |

**Deliverable:** Professional aesthetic

### Phase 4: Real-Time Optimization (Days 11-14)

| Task | Effort | Description |
|------|--------|-------------|
| Redis integration | 8h | Replace polling with pub/sub |
| Data caching layer | 6h | Reduce database queries |
| Performance tuning | 8h | Optimize render time |
| WebSocket fallback | 12h | For true real-time (optional) |
| Load testing | 4h | Verify under load |

**Deliverable:** Sub-5-second updates

### Phase 5: Testing & Polish (Days 15-17)

| Task | Effort | Description |
|------|--------|-------------|
| User testing | 8h | Review with team |
| Accessibility audit | 4h | WCAG compliance |
| Cross-browser test | 4h | Chrome/Firefox/Safari |
| Documentation | 4h | Update README |
| Final polish | 8h | Address feedback |

**Deliverable:** Production-ready dashboard

---

## 8. Technical Recommendations

### 8.1 Dependencies to Add

```txt
st_autorefresh>=0.3.0      # Auto-refresh functionality
plotly>=5.20.0             # Interactive charts
altair>=5.0.0              # Alt charts
redis>=5.0.0               # Already present
```

### 8.2 New Directory Structure

```
/opt/openclaw/dashboard/
├── app.py                      # Main dashboard (refactored)
├── components/                  # NEW: Reusable components
│   ├── __init__.py
│   ├── cards.py                # Agent/summary cards
│   ├── metrics.py              # Metric displays
│   ├── charts.py               # Chart wrappers
│   └── alerts.py               # Alert components
├── pages/
│   ├── 1_Mission_Queue.py      # Refactored
│   ├── 2_Learning_Insights.py  # Refactored
│   ├── 3_Agent_Management.py   # Enhanced
│   └── 4_Real_Time.py          # NEW: Live agent monitoring
├── utils/
│   ├── __init__.py
│   ├── redis_client.py         # NEW: Redis utilities
│   ├── data_cache.py           # NEW: Caching layer
│   └── formatters.py           # Reusable formatters
└── config.yaml                 # Updated
```

### 8.3 Redis Channel Design

```python
# Channel naming convention
CHANNELS = {
    "agent_heartbeat": "agents:heartbeat",      # Agent alive signals
    "agent_activity": "agents:activity",        # Task updates
    "mission_events": "missions:events",        # Mission lifecycle
    "alerts": "system:alerts",                  # Alert notifications
    "metrics": "system:metrics",                # Resource metrics
}

# Message format
{
    "channel": "agents:heartbeat",
    "timestamp": "2026-02-12T17:00:00Z",
    "data": {
        "agent_id": "002",
        "status": "working",
        "current_task": "Scanning logs...",
        "progress": 0.68
    }
}
```

---

## 9. Estimated Effort & Timeline

| Phase | Days | Effort (Hours) |
|-------|------|----------------|
| Phase 1: Foundation | 3 | 25 hours |
| Phase 2: Agent Visibility | 4 | 34 hours |
| Phase 3: Visual Polish | 3 | 26 hours |
| Phase 4: Real-Time Optimization | 4 | 38 hours |
| Phase 5: Testing & Polish | 3 | 28 hours |
| **Total** | **17 days** | **~151 hours** |

---

## 10. Risks & Mitigations

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Performance degradation with auto-refresh | Medium | High | Limit refresh rate, use caching |
| Redis complexity | Low | Medium | Start simple, add incrementally |
| Design inconsistency | Medium | Low | Create component library first |
| Browser compatibility | Low | Low | Test early, use standard CSS |
| Scope creep | High | High | Freeze requirements at phase start |

---

## 11. Success Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Time to first information | <2 seconds | Lighthouse audit |
| Data freshness | <5 seconds | End-to-end latency |
| Lighthouse score | >90 | Performance audit |
| User satisfaction | >8/10 | Team review |
| Agent visibility | 100% | All agents shown |

---

## 12. Files to Modify

| File | Action | Priority |
|------|--------|----------|
| `/opt/openclaw/dashboard/app.py` | Refactor | P1 |
| `/opt/openclaw/dashboard/components/*.py` | Create | P1 |
| `/opt/openclaw/dashboard/pages/3_Agent_Management.py` | Enhance | P2 |
| `/opt/openclaw/dashboard/pages/4_Real_Time.py` | Create | P2 |
| `/opt/openclaw/dashboard/utils/redis_client.py` | Create | P4 |
| `/opt/openclaw/dashboard/requirements.txt` | Update | P1 |
| `/opt/openclaw/dashboard/config.yaml` | Update | P1 |

---

## 13. Next Steps

1. **Review and approve** this plan
2. **Prioritize phases** (may skip some for MVP)
3. **Allocate time** for implementation
4. **Assign ownership** (Atlas for technical, MilkBot for design review)
5. **Begin Phase 1** after approval

---

*Plan prepared by MilkBot (CEO)*
*For review and approval before implementation*
*Status: AWAITING APPROVAL*
