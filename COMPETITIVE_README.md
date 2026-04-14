# OpenClaw Station — Product & Competitive Intelligence

> Internal document for the team. Prepared for competitive review session.
> Manager directive: "Study the competition. Share what you learned. Find our unique situation rather than blindfolding ourselves."

---

## Table of Contents

1. [What OpenClaw Station Is](#1-what-openclaw-station-is)
2. [What We Have Built (Current State)](#2-what-we-have-built-current-state)
3. [Our Architecture — The Captain Model](#3-our-architecture--the-captain-model)
4. [The Competitor We Need to Study: Hermes Agent](#4-the-competitor-we-need-to-study-hermes-agent)
5. [Full Competitive Landscape](#5-full-competitive-landscape)
6. [Where We Win](#6-where-we-win)
7. [Where We Are Behind](#7-where-we-are-behind)
8. [What We Must Build Next](#8-what-we-must-build-next)
9. [Strategic Position Summary](#9-strategic-position-summary)

---

## 1. What OpenClaw Station Is

OpenClaw Station is a **multi-agent AI workforce platform** built around a structured command hierarchy. Instead of throwing a pile of tools at a single AI and hoping it figures things out, OpenClaw organizes AI agents into **stations** — isolated workspaces where a **Captain (manager agent)** governs and directs **worker agents** to execute tasks.

The core bet: structured human-like team organization applied to AI agents produces more reliable, auditable, and controllable outcomes than flat autonomous agent systems.

**Target users:** Teams and enterprises that need AI agents to execute real work across multiple channels — not just demos, not just chatbots.

---

## 2. What We Have Built (Current State)

### Platform Foundation

| Layer | What Exists |
|-------|-------------|
| **Auth** | JWT login/register, token persisted in localStorage, auto-injected into all API calls, 401 auto-logout with re-login prompt |
| **Proxy** | Next.js API route proxies all REST calls to backend; SSE streams go direct to backend with `?token=` query param |
| **Multi-tenancy** | Station-based isolation — each station has its own agents, tasks, memory, members, settings, and billing |
| **Station types** | `personal`, `team`, `enterprise` with regional deployment (`us-east`, `us-west`, `eu-west`, `ap-southeast`, `ap-northeast`) |

### Agent System

| Feature | Status |
|---------|--------|
| Agent catalog (browse installable agents by category) | Done |
| Install / uninstall agents per station | Done |
| Enable / disable individual agents | Done |
| Agent health monitoring (`healthy`, `degraded`, `offline`) | API exists, UI not yet built |
| Per-agent permissions: `budgetLimit`, `maxConcurrentRuns`, `requireApproval` | API exists, UI not yet built |
| Visual swarm view with real agent data | Done |
| Agent avatar + role visual mapping | Done |

### Task Execution

| Feature | Status |
|---------|--------|
| Create task with `objective` + `managerAgentId` | Done |
| Task status machine: `pending` → `planning` → `running` → `waiting_approval` → `completed` / `failed` | Done |
| Task approval workflow (human-in-the-loop) | Done — ActivityList, approve/reject wired |
| Cancel, retry, approve, reject actions | Done |
| Token budget per task (`tokenBudget`) | API exists, UI input not exposed |
| Step-level progress (`stepCount`, `completedStepCount`, embedded steps) | Done |
| Error message surfacing (`errorMessage`) | Done |
| Manager agent name display (`managerAgentName`) | Done |

### Real-Time Streaming

| Feature | Status |
|---------|--------|
| SSE per-task live log stream | Done — `EventSource` with `?token=` JWT auth |
| `task_update` / `task_complete` / `heartbeat` SSE events | Done |
| 8-second polling fallback when SSE fails | Done |
| Station-wide live events stream | API exists, not yet consumed in UI |

### Pages & UI

| Page | Status |
|------|--------|
| Dashboard — real agents, tasks, stats, activity feed | Done — API-integrated |
| Workspace — submit tasks, live logs, swarm status | Done — API-integrated |
| Activity — full task history, approve/retry, polling | Done — API-integrated |
| Agents / Swarm — install agents, view swarm | Done — API-integrated |
| Map — agent hierarchy visualization | Done — real agents loaded |
| Reports — generate, view, export (JSON/CSV/MD) | Done — API-integrated |
| Stations — list, create, manage | Done — API-integrated |
| Inbox — task-derived notifications | Done — derived from real tasks |
| Pricing | Static (intentional) |
| Settings | Placeholder — nothing built yet |

### Auth Guard

| Feature | Status |
|---------|--------|
| Protected routes auto-show login modal | Done |
| Page content blocked until authenticated | Done |
| Modal cannot be dismissed on protected routes | Done |

### Memory, Schedules, Integrations

| Feature | Status |
|---------|--------|
| Station memory (5 scopes: `station`, `agent`, `task`, `user`, `global`) | API exists, no UI |
| Cron schedules with full task template | API exists, no UI |
| Integrations (Telegram, Discord, Slack, Email) | API exists, no UI |
| LLM key management (BYOK vs platform) | API exists, no UI (settings page is empty) |

---

## 3. Our Architecture — The Captain Model

```
User
  |
  v
Station (isolated workspace)
  |
  +-- Captain (Manager Agent)
  |     Approves / routes / governs
  |     Owns task assignment
  |     Controls sensitive actions
  |
  +-- Worker Agent A  (e.g. researcher)
  +-- Worker Agent B  (e.g. coder)
  +-- Worker Agent C  (e.g. writer)
        |
        v
     External World (APIs, web, data)
```

**Security principle (aligned with OWASP / NIST / PCI guidance):**

- Agents never hold raw credentials, passwords, or payment data
- Captain holds trust; vault holds secrets; agents get only a scoped result or short-lived token
- `waiting_approval` status = human firewall before any high-risk action executes
- Per-agent `budgetLimit` + `tokenBudget` = financial guardrails
- Kill and rebuild: disable/uninstall a compromised agent, reinstall fresh — task state preserved in Captain layer

**Why this matters to investors and enterprise buyers:**

> "We designed a dual-layer AI architecture where a trusted AI Captain governs untrusted execution agents. The Captain acts as a security firewall, permission engine, and decision authority, ensuring sensitive data is never exposed to any execution agent, while maintaining continuity through dynamic agent replacement."

---

## 4. The Competitor We Need to Study: Hermes Agent

**Made by:** NousResearch (the team behind the Hermes LLM model family)
**Launched:** February 2026
**License:** MIT (free, open-source, self-hosted)
**GitHub:** github.com/NousResearch/hermes-agent

### What Hermes Does

Hermes Agent is a self-hosted autonomous AI agent built around one core bet: **self-improvement**. After completing complex tasks, the agent writes and stores reusable skills, refines them over time, and grows more capable with each session. It is built for a single power user or small team who wants a deeply personalized agent — not a managed workforce platform.

### Hermes Key Features

| Feature | What It Does |
|---------|-------------|
| **Self-improving skill system** | After complex tasks, agent creates reusable skills compatible with agentskills.io open standard. Skills are refined on reuse. |
| **Layered memory** | Bounded agent-curated memory (~2,200 chars), Honcho user modeling, FTS5 full-text session search, LLM summarization for cross-session recall |
| **Model-agnostic** | Works with 200+ models via OpenRouter, Nous Portal, OpenAI, Anthropic, custom endpoints |
| **6 deployment backends** | local, Docker, SSH, Daytona, Singularity, Modal — near-zero idle cost via hibernation |
| **Multi-platform messaging** | Telegram, Discord, Slack, WhatsApp, Signal, DingTalk, WeCom — 9+ channels, single agent process |
| **Subagent spawning** | Spawn isolated parallel subagents for parallel workstreams |
| **MCP integration** | Full Model Context Protocol support |
| **48 built-in tools** | Across 40 toolsets + RPC-based Python script integration |
| **Cron scheduling** | Built-in automation scheduling |
| **OpenClaw migration** | First-time setup wizard that detects OpenClaw config and imports settings, memories, skills, and API keys |
| **Security defaults** | Prompt injection scanning, credential filtering, command approval controls, container isolation |
| **Pricing** | Free |

### Hermes vs OpenClaw — Direct Comparison

| Dimension | OpenClaw Station | Hermes Agent |
|-----------|-----------------|--------------|
| **Core model** | Multi-agent workforce (Captain + workers) | Single autonomous agent (self-improving) |
| **Tenancy** | Multi-tenant stations (team / enterprise) | Single user / single agent |
| **Self-improving skills** | No | Yes — core differentiator |
| **Memory** | 5-scope structured records, human-readable | Bounded flat, model-curated, LLM-summarized |
| **Deployment** | SaaS (hosted) | Self-hosted (6 backends) |
| **Pricing** | Subscription | Free (MIT) |
| **SSE task streaming** | Yes — per-task, JWT-authenticated | No |
| **Task approval workflow** | Yes — typed status, per-agent flags | No |
| **Per-agent resource limits** | Yes — budget, concurrency, approval | No |
| **Multi-channel intake** | Yes — typed `TaskSourceChannel` on every task | Partial — single agent routing only |
| **Agent kill / rebuild** | Yes — explicit lifecycle operations | No |
| **Reports (7 types, 3 formats)** | Yes | No |
| **Multi-member RBAC** | Yes — 7-dimension permissions per member | No |
| **Governance / audit** | API exists, UI not built | Basic prompt-injection scanning |
| **Open-source** | No | Yes (MIT) |
| **Migration from OpenClaw** | N/A | Built-in wizard |

### What the Market Is Saying

Multiple head-to-head comparison articles were published within weeks of Hermes launching (April 2026). The framing is consistent:

- **Hermes wins** for: single-user power users, self-improvement, personal agent that gets smarter, free price, more messaging platform coverage
- **OpenClaw wins** for: teams, enterprise governance, structured multi-agent orchestration, task approval workflows, cost controls, operational reporting

**The risk:** Hermes built an import wizard for OpenClaw users on day one. This signals they are explicitly positioning against us as a user acquisition strategy. We need to watch this closely.

---

## 5. Full Competitive Landscape

### The Field at a Glance

| Platform | Type | Best For | Price | Self-Host |
|----------|------|----------|-------|-----------|
| **Hermes Agent** (NousResearch) | Personal agent runtime | Single user, self-improving agent | Free (MIT) | Yes |
| **CrewAI** | Multi-agent framework + managed cloud | Developer teams, role-based crews | Free / $25–99+/mo | Partial |
| **LangGraph** (LangChain) | Graph-based agent framework | Developers, stateful workflows | Free + usage | Yes |
| **AutoGen / Microsoft Agent Framework** | Enterprise agent framework | Azure/enterprise .NET developers | Free + Azure | Yes |
| **OpenAI Agents SDK** | Handoff-based agent framework | OpenAI-native developers | Free + API usage | Yes |
| **Google ADK** | Cloud agent framework | GCP/Gemini developers | Free + GCP usage | Yes |
| **Amazon Bedrock AgentCore** | Enterprise serverless runtime | AWS enterprise customers | AWS consumption | No |
| **Relevance AI** | No-code AI workforce builder | Non-technical business users | $0–custom | No |
| **SuperAGI** | GTM/sales AI agents | Sales and marketing teams | ~$49/user/mo | No |
| **n8n + Flowise** | Visual workflow + agent nodes | Automation / ops teams | Free + $20+/mo | Yes |
| **AgentGPT** | Single autonomous agent (browser) | Consumers, demos | Free / $40/mo | No |

### The Key Insight About Developer Frameworks

CrewAI, LangGraph, AutoGen, OpenAI SDK, Google ADK — these are all **frameworks, not products**. They give you the building blocks to assemble what OpenClaw already ships as a complete platform. The developer has to build:

- The workspace / tenant isolation layer
- The task approval UI and state machine
- The SSE streaming infrastructure
- The agent lifecycle management (kill/rebuild)
- The multi-channel task intake
- The permission and budget enforcement
- The reporting system
- The member management with RBAC

OpenClaw ships all of this out of the box. **We are the product layer that sits above the framework layer.**

### The Key Insight About Enterprise Cloud

AWS Bedrock AgentCore, Google Vertex AI, Azure Agent Framework — these are powerful but require deep cloud expertise, vendor lock-in, and enterprise procurement. They lack:

- Consumer-friendly workspace UI
- Multi-channel messaging intake (Telegram, Discord, Slack as task sources)
- Task approval workflow as a product-level UX
- Per-agent budget controls in a UI
- Operational reports for non-technical managers

OpenClaw sits between "build it yourself" (frameworks) and "hand it to cloud" (AWS/GCP/Azure). **That gap is our market.**

---

## 6. Where We Win

These are genuine differentiators no competitor fully matches:

### 1. Captain/Worker Hierarchy as a Product (Not Just Code)
Named captains with avatars, health states, kill/rebuild controls, and task routing — presented as a first-class UI/UX experience. CrewAI has the concept in code; we make it a product. No one else does.

### 2. SSE Task Streaming with JWT Auth
Live per-task log streaming, JWT-scoped to the tenant, with `task_update` / `task_complete` / `heartbeat` events. No competitor ships this as a product feature. Competitors offer post-hoc dashboards or SDK callbacks.

### 3. Task Approval Workflow — First-Class Status
`waiting_approval` is a typed status in the task state machine, with `requireApproval` per agent, per station, and per task creation. LangGraph's `interrupt()` is the closest competitor feature — but it's SDK-level with no product UI. Ours is fully wired end-to-end.

### 4. Station-Based Multi-Tenancy
Per-station agent isolation, members, settings, billing, and regional deployment. No framework or personal agent runtime (including Hermes) supports this. Enterprise cloud platforms approximate it with coarse IAM roles.

### 5. Per-Agent Resource Governance
`budgetLimit`, `maxConcurrentRuns`, `requireApproval` enforced at the individual agent record level. This is the financial and operational control layer that enterprise buyers need and no framework ships natively.

### 6. Multi-Channel Typed Task Intake
`TaskSourceChannel: "api" | "telegram" | "discord" | "slack"` — every task from every channel gets a full lifecycle: SSE stream, approval workflow, agent assignment, history, reports. Hermes routes more channels to a single agent but has no task data model around them.

### 7. Kill and Rebuild as a User Operation
Disable / uninstall a compromised agent, install a fresh one — with task state preserved in the Captain layer. Exposed as explicit product-level controls, not infrastructure abstraction.

### 8. Operational Reports
7 report types (`task_summary`, `performance`, `usage`, `security`, `custom`, `weekly`, `monthly`) with JSON / CSV / Markdown export, scoped per station. No developer framework ships this. Only enterprise cloud platforms have dashboards — and those are platform-level, not tenant-scoped downloadable reports.

### 9. 7-Dimension Member RBAC
`canManageAgents`, `canCreateTasks`, `canApproveTasks`, `canViewReports`, `canManageMembers`, `canManageIntegrations`, `canManageSettings`, `canManageBilling` — `canApproveTasks` in particular creates a dedicated human approver role tied directly to the `waiting_approval` workflow. No competitor has this.

---

## 7. Where We Are Behind

Be honest about this. This is what the team needs to understand and address:

### Behind Hermes

| Hermes Advantage | Why It Matters |
|-----------------|----------------|
| **Self-improving skills** | Agent gets smarter over time. This is deeply differentiating for power users. We have no equivalent. |
| **Free and open-source** | Price is a major acquisition lever, especially for individual developers and small teams exploring AI agents. |
| **More messaging platform coverage** | Hermes supports WhatsApp, Signal, DingTalk, WeCom on top of our TG/Discord/Slack |
| **Migration wizard from OpenClaw** | They are actively targeting our users. This is a threat signal. |
| **More deployment flexibility** | 6 backends including serverless Modal. We are SaaS-only. |

### Behind the Field Generally

| Gap | Detail |
|-----|--------|
| **Settings page is empty** | This is where vault connections, agent permissions UI, approval rules, budget limits, and LLM key management should live. Currently a placeholder. |
| **No vault / credential management UI** | Users cannot connect Google, Stripe, or external accounts through the UI. API types exist but no frontend. |
| **No agent permission configuration UI** | `budgetLimit`, `maxConcurrentRuns`, `requireApproval` cannot be set by the user anywhere. |
| **No audit log viewer** | No way to see who approved what, which agent accessed what, payment history. |
| **No memory management UI** | 5-scope memory system exists in the API. Users cannot view, edit, or manage it. |
| **No schedule management UI** | Cron scheduling exists in the API. No UI to create or manage schedules. |
| **No integration management UI** | Telegram, Discord, Slack connections exist in the API. No UI to configure them. |
| **No token refresh** | Access token expiry logs the user out instead of silently refreshing. |
| **No task approval from workspace** | `waiting_approval` tasks can only be approved from the Activity page, not from the workspace where you submitted the task. |

---

## 8. What We Must Build Next

Priority order based on competitive impact and completeness:

### Priority 1 — Close the Product Gap (Settings Page)
The Settings page is the most critical missing surface. It should expose:
- Per-agent permission configuration (budget, concurrency, approval gate)
- Station-level approval policies
- LLM key management (BYOK vs platform)
- External account connections (vault / OAuth)
- Audit log viewer
- Member RBAC management

**Why now:** Every enterprise sales conversation will hit this. Without it, we cannot credibly sell governance.

### Priority 2 — Task Approval from Workspace
When a task hits `waiting_approval`, the workspace panel should show an Approve / Reject button inline. Currently users must leave the workspace and go to Activity. This breaks the workflow.

### Priority 3 — Memory Management UI
The 5-scope memory system is a real differentiator that users cannot access. A simple view/edit/delete UI on the station settings or workspace page would make this visible and valuable.

### Priority 4 — Schedule & Integration Management UI
Cron tasks and channel connections (Telegram, Discord, Slack) exist in the backend. Surfacing them in the UI turns a backend capability into a visible product feature.

### Priority 5 — Skill System (Answer to Hermes)
Hermes's self-improving skill system is its strongest differentiator. We do not need to copy it exactly, but we need a response. Options:
- **Task templates** — let users save objectives as reusable templates
- **Agent memory annotations** — let Captains save successful task patterns as station memory
- **Skill library** — agents accumulate reusable procedures over completed tasks

### Priority 6 — Token Refresh
Silent JWT refresh using the stored `refreshToken`. Prevents session interruption during long-running tasks.

---

## 9. Strategic Position Summary

```
Personal / Single User         Team / Enterprise
        |                              |
   [Hermes Agent]              [OpenClaw Station]
   Free, self-hosted            SaaS, governed
   Self-improving               Multi-agent, auditable
   One user, deep memory        Multi-member, typed workflows
        |                              |
        v                              v
   "Your AI that               "Your AI workforce
    gets smarter"               that stays controlled"
```

**The market gap OpenClaw occupies:**

- Above developer frameworks (LangGraph, CrewAI, AutoGen) — we ship the product layer they require you to build
- Below enterprise cloud (AWS Bedrock, Google Vertex, Azure) — we are accessible without cloud vendor lock-in
- Adjacent to personal agents (Hermes) — we serve teams that need governance, not just capability

**The one sentence to use with investors:**

> "OpenClaw Station is the only platform that combines production multi-agent orchestration with enterprise governance — real-time task streaming, typed approval workflows, per-agent budget controls, and multi-tenant workspace isolation — without requiring a cloud vendor or a team of developers to build it."

**The honest risk to flag internally:**

Hermes is free, open-source, and already targeting OpenClaw users with a migration wizard. If we do not build the features only we can build — governance, approval workflows, reporting, team management — we will lose the differentiating ground that justifies our pricing. The answer is not to out-Hermes Hermes. The answer is to be so strong on the team/enterprise/governance layer that the comparison becomes meaningless.

---

*Document prepared: April 2026. Sources: codebase analysis + competitive web research. For internal use.*
