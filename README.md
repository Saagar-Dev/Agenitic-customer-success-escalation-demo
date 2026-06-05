# 🚨 Agentic Customer Success Escalation Engine

> 8 autonomous AI agents monitor a $5.7M B2B SaaS account portfolio simultaneously — scanning for churn risk, building recovery plans, and escalating before accounts reach the point of no return.

[![Built with Claude](https://img.shields.io/badge/Powered%20by-Claude%20Haiku-blueviolet)](https://anthropic.com)
[![Agentic AI](https://img.shields.io/badge/Type-Multi--Agent-orange)](https://docs.anthropic.com)
[![Pure HTML](https://img.shields.io/badge/Stack-Vanilla%20HTML%2FJS-blue)](https://developer.mozilla.org)

---

## What Is This?

A fully live, single-file agentic AI demo that runs 8 specialised Customer Success agents simultaneously in your browser. Each agent independently monitors the portfolio, investigates at-risk accounts, and creates formal escalation plans with recovery recommendations — all via real Claude API calls.

**The Scenario:** You are the Chief Customer Success Officer of a B2B SaaS company with a $5.7M ARR portfolio of 20 accounts. Six are critically at risk — renewal in under 30 days, usage collapsing, or executives going dark. Your CS team is stretched thin. You deploy 8 AI agents to work the portfolio simultaneously.

---

## The Portfolio

**$5.7M total ARR · 20 accounts · 6 critical (renewal < 30 days)**

| Account | ARR | Renewal | Risk | Key Signals |
|---------|-----|---------|------|-------------|
| **Apexus** | $430k | 18 days | 🔴 Critical | New CFO, contract reduction request |
| **Pinnacle** | $420k | 25 days | 🔴 Critical | Competitor evaluation underway |
| **TechVision** | $320k | 12 days | 🔴 Critical | Usage -60%, champion left |
| **Nexgen** | $290k | 20 days | 🔴 Critical | Invoice 45 days overdue |
| **NovaSoft** | $210k | 8 days | 🔴 Critical | Exec sponsor vacant, QBR refused |
| **Skylab** | $140k | 15 days | 🔴 Critical | NPS score 2, 5 unanswered escalations |
| Quantum | $760k | 50 days | 🟠 High | 40% user turnover, new team |
| Meridian | $650k | 90 days | 🟠 High | Usage declining 3 consecutive months |
| Polaris | $520k | 35 days | 🟠 High | Company-wide budget cuts |
| Acme Corp | $480k | 45 days | 🟠 High | Exec disengaged, 30% feature adoption |
| *…and 10 more* | | | | |

---

## The 8 CS Agents

| Agent | Role | Specialty |
|-------|------|-----------|
| 🔭 **Scout** | Portfolio Monitor | Broad health scan, catches anomalies specialists might miss |
| 💓 **Pulse** | NPS & Sentiment | Satisfaction scores, survey feedback, satisfaction decline signals |
| 🔍 **Lens** | Usage Intelligence | Product adoption, feature usage, login frequency drops |
| ⚡ **Rex** | Renewal Specialist | Contracts, renewal timelines, commercial risk |
| 🎙️ **Vera** | Voice of Customer | Support tickets, escalation history, relationship health |
| 💰 **Cole** | Financial Risk | ARR, payment delays, budget review signals |
| 🎯 **Quinn** | Escalation Coordinator | Triage and prioritisation of highest-value at-risk accounts |
| 🧠 **Sage** | Recovery Strategist | Building structured save plans for complex recoveries |

---

## What You're Watching

### Left — Risk Radar (D3)
Concentric rings showing all 20 accounts by churn risk:

- **🔴 Critical zone (inner)** — renewal < 30 days
- **🟠 High risk (middle)** — 31–90 days, declining signals
- **🟡 Medium (outer middle)** — monitored accounts
- **🟢 Safe zone (outer)** — healthy accounts

Bubble size = ARR value. When an agent creates an escalation, the account animates outward toward the safe zone. Agents pulse accounts they're actively working.

### Right — Renewal Urgency Timeline (D3)
All 20 accounts as horizontal bars, sorted top-to-bottom by days to renewal:

- **Bar length** = days remaining
- **Bar thickness** = ARR at stake
- **Colour** = risk level (red → amber → yellow → green)
- **Urgency zones** = <30d critical band, <60d high, <90d elevated
- When escalated → bar transitions to emerald with ✓

### Agent Strip
8 compact horizontal cards — each showing live status, budget spend bar, stamina bar, escalation pills, and a rolling decision log.

### Escalation Strip
Horizontal scrolling feed of created escalations. P1/P2/P3 severity, account name, ARR protected, recovery plan snippet. New cards appear from the left.

### ⚡ News Ticker
Bloomberg-style event strip. Every escalation, gap, milestone, and agent exhaustion fires a live message — all queued and played in sequence, none skipped.

### 🧠 Agent Reasoning Board
Kanban view — one column per agent. Every decision timestamped with the agent's action type badge + Claude's actual reasoning text from the API response.

---

## What the Metrics Mean

At the end of a mission:

| Metric | Meaning |
|--------|---------|
| **ARR Protected** | Total ARR value of accounts that received a formal escalation plan |
| **Escalations** | Number of P1/P2/P3 escalation plans created with recovery steps |
| **API Calls** | Total Claude Haiku calls across all 8 agents |

**Priority score (0–100):**
- 75+ → P1 escalation, urgent executive intervention
- 55–74 → P2, significant risk, CSM-led recovery plan
- 35–54 → P3, monitor with check-in cadence
- < 35 → healthy, no action needed

---

## Agent Tools

Each agent calls four tools:

| Tool | What It Does | Cost |
|------|-------------|------|
| `scan_accounts` | Scan portfolio for accounts matching agent's specialty | $5 |
| `pull_account_data` | Deep dive into one account from one data source | $8 |
| `assess_risk` | Synthesise data into a formal risk score + root cause | $10 |
| `create_escalation` | Create P1/P2/P3 escalation with recovery plan | $20 |
| `assign_action` | Add a specific recovery action item | $5 |

Each agent has a **$500 budget** and stamina pool. Budget controls how many deep dives an agent can do. Stamina represents cognitive bandwidth — it drains on each source access.

---

## Architecture

```
Claude Haiku API — 8 agents sharing a global rate limiter (2.2s between calls)

Scout  Pulse  Lens  Rex  Vera  Cole  Quinn  Sage
  \      |     |     |    |     |     /    /
   ╔══════════════════════════════════════════╗
   ║  20 Accounts × 5 Data Sources           ║
   ║  CRM · Product · Support · NPS ·        ║
   ║  Financial                               ║
   ╚══════════════════════════════════════════╝
```

**Rate limiting:** All 8 agents share a single `acquireSlot()` queue with a 2.2s gap. ~27 calls/min — safely within Anthropic Tier 1 limits. Agents stagger starts by 1.2s each.

**Pause:** Freezes the API queue (holds it in the `drainQ` loop), SVG animations, and the news ticker mid-animation. Resume continues from exactly where everything stopped.

---

## Controls

| Control | Function |
|---------|----------|
| **▶ RUN AGENTS** | Deploys all 8 agents simultaneously |
| **⏸ PAUSE / Resume** | Freezes API queue, Risk Radar SVG, timeline, ticker |
| **0.5× 1× 2×** | Speed multiplier (affects sleep delays between tool calls) |
| **🧠 Reasoning Board** | Full kanban of every decision + Claude's reasoning text |
| **⤢ Expand** | Full-screen reasoning board |

---

## Running Locally

```bash
# Clone
git clone https://github.com/saagar-devadiga/agentic-demos
cd agentic-demos

# Serve over HTTP (required for CORS)
npx serve .
# or: python3 -m http.server 8080

# Open in browser
open http://localhost:3000/cs_engine.html
```

Enter your Anthropic API key (`sk-ant-...`) in the intro screen.

> ⚠️ Must be served over **HTTPS** for production (GitHub Pages, Netlify). `file://` blocks API calls.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| AI | Claude Haiku (`claude-haiku-4-5-20251001`) |
| Visualisation | D3 v7 — risk radar (radial layout) + renewal timeline (horizontal bars) |
| Animation | SVG `animateTransform`, Web Animations API |
| Build | None — pure HTML/CSS/JS, single file |

---

## Why This Matters

Traditional CS tools show you dashboards. This shows you agents *acting*.

Each agent:
1. **Perceives** — reads the full account state across 5 data sources
2. **Reasons** — Claude decides which accounts are most at risk and why
3. **Acts** — creates a real escalation plan with specific recovery steps
4. **Adapts** — if another agent already escalated an account, moves on to the next

The result is a CS team that never sleeps, never gets stretched thin, and never misses a renewal.

---

## The Full Demo Suite

| File | Demo |
|------|------|
| `cs_engine.html` | **CS Escalation Engine** — this file |
| `rev_heb.html` | **Agentic Revenue Engine** — 10 sales agents, HEB visualisation, $1.6M pipeline |
| `kerala_v2.html` | **Kerala Kitchen** — 2 agents shop a directed market graph and cook a feast |
| `pokemon.html` | **Pokémon Battle** — agentic AI battle simulator |
| `brisbane.html` | **Brisbane City Intelligence** — 8 agents analyse real suburb data on a live Leaflet map |

---

## About

Built by **Saagar Devadiga** — Data Engineer & AI Developer, Brisbane, Australia.
MSc Data Science · Queensland University of Technology

> *"This is what the next generation of enterprise tooling looks like — not dashboards you read, but agents that act."*

---

*Powered by [Anthropic Claude](https://anthropic.com) · Visualised with [D3.js](https://d3js.org)*
