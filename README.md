# OpenClaw Station — Backend API

> **A Station is your team's AI workspace.** You install agents, give them tasks, and they run automatically — researching, writing, monitoring, or communicating on your behalf across Telegram, Discord, Slack, and Email.

---

## Who Should Read What

This repository contains two documentation files. Find your role below and jump straight to the right section.

### Not a developer? Start here.

You do not need to read any code or technical diagrams to understand what OpenClaw Station does, how tasks flow, and what it costs.

| I want to know... | Go to |
|-------------------|-------|
| What the platform does in plain English | [What is OpenClaw Station?](#what-is-openclaw-station) |
| How a task works — one agent | [Scenario 1 — One Task, One Agent](#scenario-1--one-task-one-agent) |
| How a task works — agent backlog | [Scenario 2 — Two Tasks, One Agent](#scenario-2--two-tasks-one-agent-back-to-back) |
| How two agents collaborate | [Scenario 3 — Two Agents Working Together](#scenario-3--two-agents-working-together) |
| indexx.ai — daily market briefing | [Scenario 4 — indexx.ai Daily Market Report](#scenario-4--indexxai-daily-market-index-report) |
| BitcoinYay — mobile onboarding & mining | [Scenario 5 — BitcoinYay Mobile](#scenario-5--bitcoinyay-mobile-user-opens-app-logs-in-starts-mining) |
| WallStreet.indexx.ai — buy or launch a token | [Scenario 6 — WallStreet.indexx.ai](#scenario-6--wallstreetindexxa-investor-buys-a-token--enterprise-launches-a-token) |
| emmm.io — prediction market | [Scenario 7 — emmm.io Prediction Market](#scenario-7--emmmio-prediction-market-polymarket-style) |
| What it costs to run | [Cost Analysis](#cost-analysis) |
| Pricing options | [Pricing Models](#pricing-models) |

### Developer or engineer? Start here.

| I want to know... | Go to |
|-------------------|-------|
| How to run the project locally | [Setup & Installation](#setup--installation) |
| All API endpoints | [Complete API Reference](#complete-api-reference) |
| Environment variables | [Environment Variables](#environment-variables) |
| How LLM keys are resolved | [LLM_ARCHITECTURE.md — Key Resolution Flow](./LLM_ARCHITECTURE.md#key-resolution-flow) |
| How each scenario runs technically | [LLM_ARCHITECTURE.md — Scenario A/B/C](./LLM_ARCHITECTURE.md#scenario-a--single-agent-single-task) |
| Which model is used and when | [LLM_ARCHITECTURE.md — Model Selection Policy](./LLM_ARCHITECTURE.md#model-selection-policy) |
| Database writes per scenario | [LLM_ARCHITECTURE.md — Database Writes](./LLM_ARCHITECTURE.md#database-writes-per-scenario) |
| Token counting and cost logging | [LLM_ARCHITECTURE.md — Token Accounting](./LLM_ARCHITECTURE.md#token-accounting) |
| Sprint plan and module list | [Sprint Roadmap](#sprint-roadmap) |

---

## What is OpenClaw Station?

OpenClaw Station is a platform where you create **Stations** — isolated workspaces for your team or project. Inside each Station you install **AI Agents** from a catalog, connect your communication channels (Telegram, Discord, Slack, Email), and give the station tasks to run.

You write the goal. The agents do the work.

**Key concepts in plain English:**

| Term | What it means |
|------|---------------|
| **Station** | Your team's AI workspace — like a project room with its own agents, tasks, and history |
| **Agent** | An AI worker with a specific skill — researching, writing, monitoring, or communicating |
| **Task** | A job you give the station — agents pick it up, execute it, and return the result |
| **Manager Agent** | A senior agent that reads your task, breaks it into steps, and delegates to specialist agents |
| **Memory** | Notes an agent saves so it can remember context across future tasks |
| **Schedule** | A task set to run automatically on a recurring timer (daily, weekly, etc.) |
| **Channel** | Where results or updates are sent — Telegram, Discord, Slack, or Email |
| **Platform Key** | The platform pays for the AI — you pay via credits |
| **BYOK** | Bring Your Own Key — you connect your own Claude or OpenAI API key, you pay your own AI bill |

---

## Tech Stack

| Layer            | Technology                                       |
|------------------|--------------------------------------------------|
| Framework        | NestJS 10 (Node.js / TypeScript)                 |
| Database         | MongoDB via Mongoose 8                           |
| Cache / Queue    | Redis (ioredis) + BullMQ                         |
| Auth             | Passport.js — JWT (access + refresh tokens)      |
| Real-time        | Socket.IO (WebSocket) + NestJS SSE               |
| Rate Limiting    | @nestjs/throttler                                |
| Payments         | Stripe                                           |
| Blockchain       | BTCY token (EVM-compatible via RPC)              |
| AI / LLM         | OpenAI-compatible API (configurable base URL)    |
| Object Storage   | Tencent Cloud COS                                |
| Messaging bots   | Telegram Bot API, Discord.js, Slack Bolt         |
| Email (SMTP)     | Nodemailer                                       |
| Validation       | class-validator + class-transformer              |
| Security         | Helmet, CORS, throttler guards                   |

---

## Setup & Installation

### Prerequisites

- Node.js >= 20
- MongoDB >= 6
- Redis >= 7

### Steps

```bash
# 1. Clone the repository
git clone https://github.com/your-org/openclaw-station-backend.git
cd openclaw-station-backend

# 2. Install dependencies
npm install

# 3. Copy the environment template
cp .env.example .env

# 4. Configure environment variables
#    Edit .env with your own values (see table below)
nano .env

# 5. Start in development mode (hot-reload)
npm run start:dev

# 6. (Optional) Build and run in production
npm run build
npm run start:prod
```

The API will be available at `http://localhost:3000/api/v1`.

---

## Environment Variables

| Variable                  | Default / Example                                      | Description                                                 |
|---------------------------|--------------------------------------------------------|-------------------------------------------------------------|
| `PORT`                    | `3000`                                                 | HTTP port the server listens on                             |
| `NODE_ENV`                | `development`                                          | `development`, `production`, or `test`                      |
| `MONGODB_URI`             | `mongodb://localhost:27017/openclaw_station`           | Full MongoDB connection string                              |
| `REDIS_HOST`              | `localhost`                                            | Redis host                                                  |
| `REDIS_PORT`              | `6379`                                                 | Redis port                                                  |
| `REDIS_PASSWORD`          | *(empty)*                                              | Redis password (leave empty if no auth)                     |
| `JWT_SECRET`              | *(change in production)*                               | Secret key for signing access tokens                        |
| `JWT_REFRESH_SECRET`      | *(change in production)*                               | Secret key for signing refresh tokens                       |
| `JWT_EXPIRES_IN`          | `15m`                                                  | Access token expiry                                         |
| `REFRESH_EXPIRES_IN`      | `7d`                                                   | Refresh token expiry                                        |
| `AI_API_KEY`              | `your_ai_api_key_here`                                 | API key for the LLM provider                                |
| `AI_BASE_URL`             | `https://api.openai.com/v1`                            | Base URL for the OpenAI-compatible API                      |
| `AI_DEFAULT_MODEL`        | `gpt-4o`                                               | Default model used by agents                                |
| `TENCENT_COS_SECRET_ID`   | `your_cos_secret_id`                                   | Tencent Cloud COS secret ID                                 |
| `TENCENT_COS_SECRET_KEY`  | `your_cos_secret_key`                                  | Tencent Cloud COS secret key                                |
| `TENCENT_COS_BUCKET`      | `your-bucket-name`                                     | COS bucket name for file uploads                            |
| `TENCENT_COS_REGION`      | `ap-guangzhou`                                         | COS region                                                  |
| `TELEGRAM_BOT_TOKEN`      | `your_telegram_bot_token`                              | Token from @BotFather for the platform Telegram bot         |
| `ADMIN_EMAIL`             | `admin@openclaw.io`                                    | Email address seeded as the initial admin user              |
| `STRIPE_SECRET_KEY`       | `sk_test_...`                                          | Stripe secret key for subscription billing                  |
| `STRIPE_WEBHOOK_SECRET`   | `whsec_...`                                            | Stripe webhook signing secret                               |
| `SMTP_HOST`               | `smtp.gmail.com`                                       | SMTP server hostname                                        |
| `SMTP_PORT`               | `587`                                                  | SMTP server port                                            |
| `SMTP_USER`               | `your_email@gmail.com`                                 | SMTP authentication username                                |
| `SMTP_PASS`               | `your_email_password`                                  | SMTP authentication password / app password                 |
| `SMTP_FROM`               | `noreply@openclaw.io`                                  | Default "From" address for transactional emails             |
| `DISCORD_BOT_TOKEN`       | `your_discord_bot_token`                               | Discord bot token                                           |
| `SLACK_BOT_TOKEN`         | `your_slack_bot_token`                                 | Slack bot OAuth token (`xoxb-...`)                          |
| `SLACK_SIGNING_SECRET`    | `your_slack_signing_secret`                            | Slack app signing secret for request verification           |
| `BTCY_RPC_URL`            | `https://mainnet.infura.io/v3/your_project_id`         | EVM-compatible RPC endpoint for BTCY contract calls         |
| `BTCY_CONTRACT_ADDRESS`   | `0x000...`                                             | Deployed BTCY token contract address                        |
| `APP_URL`                 | `http://localhost:3000`                                | Public URL of this backend (used in email links)            |
| `FRONTEND_URL`            | `http://localhost:5173`                                | Frontend origin (used for CORS and redirect URLs)           |

---

## Architecture Overview

```
src/
├── auth/                   # JWT auth, register/login, password reset
├── users/                  # User profile and preferences
├── stations/               # Station CRUD, settings, templates
├── members/                # Station membership and role management
├── agents/                 # Agent catalog + per-station installation
├── task-orchestrator/      # Task lifecycle, steps, approval, SSE
├── reports/                # Report generation and export
├── memory/                 # Vector memory store (create, search, CRUD)
├── schedules/              # Cron-based task scheduling
├── integrations/           # Telegram / Discord / Slack / Email connectors
├── notifications/          # In-app notification inbox
├── usage/                  # Token and compute usage metrics
├── billing/                # Stripe subscriptions, invoices, payments
├── btcy/                   # BTCY blockchain wallet and payments
├── webhooks/               # Outbound webhook registry
├── admin/                  # Admin-only monitoring and audit endpoints
├── agent-runtime/          # Internal agent execution engine
├── audit/                  # Audit log schema and service
└── common/                 # Guards, decorators, interceptors, pipes
```

All routes are versioned under `/api/v1`. The global prefix is `api` and the version is set via NestJS URI versioning (`@Version('1')`).

### Request / Response Conventions

- All requests that carry a body use `Content-Type: application/json`.
- All authenticated endpoints require `Authorization: Bearer <access_token>`.
- Paginated list responses return `{ data: T[], total: number }`.
- Error responses follow `{ statusCode, message, error }`.
- Public endpoints (no auth required): `POST /auth/register`, `POST /auth/login`, `POST /auth/refresh-token`, `POST /auth/forgot-password`, `POST /auth/reset-password`, `POST /auth/verify-email`, and the three integration webhook endpoints.

---

## Complete API Reference

### Auth

| Method | Endpoint                    | Auth Required | Description                                      |
|--------|-----------------------------|---------------|--------------------------------------------------|
| POST   | `/auth/register`            | No            | Create a new user account                        |
| POST   | `/auth/login`               | No            | Authenticate and receive JWT tokens              |
| POST   | `/auth/logout`              | Yes           | Invalidate the current session                   |
| POST   | `/auth/refresh-token`       | No            | Exchange a refresh token for a new access token  |
| POST   | `/auth/forgot-password`     | No            | Send a password reset email                      |
| POST   | `/auth/reset-password`      | No            | Reset password with a token from email           |
| POST   | `/auth/verify-email`        | No            | Verify email address with a token                |

#### Example — Register

**Request**
```http
POST /api/v1/auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "StrongP@ss123",
  "firstName": "Jane",
  "lastName": "Doe"
}
```

**Response** `201 Created`
```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiJ9...",
  "refreshToken": "eyJhbGciOiJIUzI1NiJ9...",
  "user": {
    "_id": "664abc123def456789",
    "email": "user@example.com",
    "firstName": "Jane",
    "lastName": "Doe"
  }
}
```

#### Example — Login

**Request**
```http
POST /api/v1/auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "StrongP@ss123"
}
```

**Response** `200 OK`
```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiJ9...",
  "refreshToken": "eyJhbGciOiJIUzI1NiJ9...",
  "user": { "_id": "664abc123def456789", "email": "user@example.com" }
}
```

---

### Users

| Method | Endpoint                  | Auth Required | Description                          |
|--------|---------------------------|---------------|--------------------------------------|
| GET    | `/users/me`               | Yes           | Get the authenticated user's profile |
| PATCH  | `/users/me`               | Yes           | Update profile fields                |
| GET    | `/users/me/preferences`   | Yes           | Get user preferences                 |
| PATCH  | `/users/me/preferences`   | Yes           | Update user preferences              |

---

### Stations

| Method | Endpoint                              | Auth Required | Description                            |
|--------|---------------------------------------|---------------|----------------------------------------|
| POST   | `/stations`                           | Yes           | Create a new station                   |
| GET    | `/stations`                           | Yes           | List the user's stations               |
| GET    | `/stations/:stationId`                | Yes           | Get a single station                   |
| PATCH  | `/stations/:stationId`                | Yes           | Update station metadata                |
| DELETE | `/stations/:stationId`                | Yes           | Delete a station                       |
| POST   | `/stations/:stationId/archive`        | Yes           | Archive a station                      |
| GET    | `/stations/:stationId/summary`        | Yes           | Get station summary (counts, activity) |
| POST   | `/stations/from-template`             | Yes           | Create a station from a template       |

Query parameters for `GET /stations`: `page` (default `1`), `limit` (default `20`).

#### Example — Create Station

**Request**
```http
POST /api/v1/stations
Authorization: Bearer <token>
Content-Type: application/json

{
  "name": "My AI Station",
  "description": "Automates weekly reporting",
  "icon": "robot",
  "color": "#4F46E5"
}
```

**Response** `201 Created`
```json
{
  "_id": "station_abc123",
  "name": "My AI Station",
  "ownerId": "user_xyz",
  "status": "active",
  "createdAt": "2026-03-21T10:00:00.000Z"
}
```

---

### Station Settings

| Method | Endpoint                              | Auth Required | Description                  |
|--------|---------------------------------------|---------------|------------------------------|
| GET    | `/stations/:stationId/settings`       | Yes           | Retrieve station settings    |
| PATCH  | `/stations/:stationId/settings`       | Yes           | Update station settings      |

---

### Templates

| Method | Endpoint                              | Auth Required | Description                 |
|--------|---------------------------------------|---------------|-----------------------------|
| GET    | `/station-templates`                  | Yes           | List all station templates  |
| GET    | `/station-templates/:templateId`      | Yes           | Get a template by ID        |

---

### Station Members

| Method | Endpoint                                        | Auth Required | Description              |
|--------|-------------------------------------------------|---------------|--------------------------|
| GET    | `/stations/:stationId/members`                  | Yes           | List station members     |
| POST   | `/stations/:stationId/members`                  | Yes           | Invite a member by email |
| PATCH  | `/stations/:stationId/members/:memberId`        | Yes           | Update member role       |
| DELETE | `/stations/:stationId/members/:memberId`        | Yes           | Remove a member          |

Query parameters for `GET`: `page`, `limit`.

Body for `POST`: `{ "email": "...", "role": "editor" }`. Valid roles: `owner`, `editor`, `viewer`.

---

### Agent Catalog

| Method | Endpoint                        | Auth Required | Description                    |
|--------|---------------------------------|---------------|--------------------------------|
| GET    | `/agents/catalog`               | Yes           | List all available agents      |
| GET    | `/agents/catalog/:agentId`      | Yes           | Get a single catalog agent     |

Query parameters for `GET /agents/catalog`: `page`, `limit`, `category`.

---

### Station Agents

| Method | Endpoint                                                  | Auth Required | Description                          |
|--------|-----------------------------------------------------------|---------------|--------------------------------------|
| GET    | `/stations/:stationId/agents`                             | Yes           | List installed agents                |
| POST   | `/stations/:stationId/agents/install`                     | Yes           | Install an agent from the catalog    |
| POST   | `/stations/:stationId/agents/:agentId/uninstall`          | Yes           | Remove an installed agent            |
| PATCH  | `/stations/:stationId/agents/:agentId/config`             | Yes           | Update agent configuration           |
| POST   | `/stations/:stationId/agents/:agentId/enable`             | Yes           | Enable a disabled agent              |
| POST   | `/stations/:stationId/agents/:agentId/disable`            | Yes           | Disable an agent                     |
| GET    | `/stations/:stationId/agents/:agentId/health`             | Yes           | Get agent health status              |
| GET    | `/stations/:stationId/agents/:agentId/runs`               | Yes           | List agent execution runs            |

---

### Tasks

| Method | Endpoint                                               | Auth Required | Description                           |
|--------|--------------------------------------------------------|---------------|---------------------------------------|
| POST   | `/stations/:stationId/tasks`                           | Yes           | Create and queue a task               |
| GET    | `/stations/:stationId/tasks`                           | Yes           | List tasks (filterable by status)     |
| GET    | `/stations/:stationId/tasks/:taskId`                   | Yes           | Get a task                            |
| POST   | `/stations/:stationId/tasks/:taskId/cancel`            | Yes           | Cancel a task                         |
| POST   | `/stations/:stationId/tasks/:taskId/retry`             | Yes           | Retry a failed task                   |
| POST   | `/stations/:stationId/tasks/:taskId/approve`           | Yes           | Approve a task awaiting review        |
| POST   | `/stations/:stationId/tasks/:taskId/reject`            | Yes           | Reject a task awaiting review         |
| GET    | `/stations/:stationId/tasks/:taskId/plan`              | Yes           | Get the task's execution plan         |
| GET    | `/stations/:stationId/tasks/:taskId/steps`             | Yes           | List plan steps with status           |
| GET    | `/stations/:stationId/tasks/:taskId/runs`              | Yes           | List agent runs for the task          |
| GET    | `/stations/:stationId/tasks/:taskId/logs`              | Yes           | Get raw execution logs                |
| GET    | `/stations/:stationId/tasks/:taskId/stream`            | Yes (SSE)     | Real-time task progress stream        |
| GET    | `/stations/:stationId/live/events`                     | Yes (SSE)     | Live station-wide event stream        |

Query parameters for `GET /tasks`: `page`, `limit`, `status` (`pending` | `running` | `completed` | `failed` | `cancelled`).

#### Example — Create Task

**Request**
```http
POST /api/v1/stations/station_abc123/tasks
Authorization: Bearer <token>
Content-Type: application/json

{
  "title": "Summarise Q1 sales report",
  "description": "Read the attached CSV and produce an executive summary.",
  "agentId": "agent_xyz",
  "priority": "high",
  "inputs": {
    "fileUrl": "https://cdn.example.com/q1-sales.csv"
  }
}
```

**Response** `201 Created`
```json
{
  "_id": "task_def456",
  "title": "Summarise Q1 sales report",
  "status": "pending",
  "stationId": "station_abc123",
  "createdAt": "2026-03-21T10:05:00.000Z"
}
```

---

### Reports

| Method | Endpoint                                          | Auth Required | Description                    |
|--------|---------------------------------------------------|---------------|--------------------------------|
| POST   | `/stations/:stationId/reports`                    | Yes           | Generate a new report          |
| GET    | `/stations/:stationId/reports`                    | Yes           | List reports                   |
| GET    | `/stations/:stationId/reports/export`             | Yes           | Get a signed export URL        |
| GET    | `/stations/:stationId/reports/:reportId`          | Yes           | Get a single report            |
| DELETE | `/stations/:stationId/reports/:reportId`          | Yes           | Delete a report                |

Query parameters for `GET /reports`: `page`, `limit`, `type`.
Query parameters for `GET /reports/export`: `reportId`.

---

### Memory

| Method | Endpoint                                          | Auth Required | Description                        |
|--------|---------------------------------------------------|---------------|------------------------------------|
| POST   | `/stations/:stationId/memory`                     | Yes           | Add a memory entry                 |
| GET    | `/stations/:stationId/memory`                     | Yes           | List memory entries                |
| POST   | `/stations/:stationId/memory/search`              | Yes           | Semantic search over memory        |
| PATCH  | `/stations/:stationId/memory/:memoryId`           | Yes           | Update a memory entry              |
| DELETE | `/stations/:stationId/memory/:memoryId`           | Yes           | Delete a memory entry              |

Query parameters for `GET /memory`: `page`, `limit`, `scope` (`global` | `agent` | `task`), `tags`.

#### Example — Search Memory

**Request**
```http
POST /api/v1/stations/station_abc123/memory/search
Authorization: Bearer <token>
Content-Type: application/json

{
  "query": "primary contact email address",
  "topK": 5,
  "scope": "global"
}
```

---

### Schedules

| Method | Endpoint                                                  | Auth Required | Description              |
|--------|-----------------------------------------------------------|---------------|--------------------------|
| POST   | `/stations/:stationId/schedules`                          | Yes           | Create a cron schedule   |
| GET    | `/stations/:stationId/schedules`                          | Yes           | List schedules           |
| GET    | `/stations/:stationId/schedules/:scheduleId`              | Yes           | Get a schedule           |
| PATCH  | `/stations/:stationId/schedules/:scheduleId`              | Yes           | Update a schedule        |
| DELETE | `/stations/:stationId/schedules/:scheduleId`              | Yes           | Delete a schedule        |
| POST   | `/stations/:stationId/schedules/:scheduleId/pause`        | Yes           | Pause a schedule         |
| POST   | `/stations/:stationId/schedules/:scheduleId/resume`       | Yes           | Resume a paused schedule |

---

### Integrations

| Method | Endpoint                                                           | Auth Required | Description                          |
|--------|--------------------------------------------------------------------|---------------|--------------------------------------|
| GET    | `/stations/:stationId/integrations`                                | Yes           | List all integrations                |
| POST   | `/stations/:stationId/integrations/telegram/connect`               | Yes           | Connect Telegram bot                 |
| POST   | `/stations/:stationId/integrations/telegram/webhook`               | **No**        | Telegram webhook (called by Telegram)|
| GET    | `/stations/:stationId/integrations/telegram/status`                | Yes           | Telegram connection status           |
| POST   | `/stations/:stationId/integrations/telegram/disconnect`            | Yes           | Disconnect Telegram                  |
| POST   | `/stations/:stationId/integrations/discord/connect`                | Yes           | Connect Discord bot                  |
| POST   | `/stations/:stationId/integrations/discord/webhook`                | **No**        | Discord webhook (called by Discord)  |
| GET    | `/stations/:stationId/integrations/discord/status`                 | Yes           | Discord connection status            |
| POST   | `/stations/:stationId/integrations/discord/disconnect`             | Yes           | Disconnect Discord                   |
| POST   | `/stations/:stationId/integrations/slack/connect`                  | Yes           | Connect Slack app                    |
| POST   | `/stations/:stationId/integrations/slack/webhook`                  | **No**        | Slack events webhook                 |
| GET    | `/stations/:stationId/integrations/slack/status`                   | Yes           | Slack connection status              |
| POST   | `/stations/:stationId/integrations/slack/disconnect`               | Yes           | Disconnect Slack                     |
| POST   | `/stations/:stationId/integrations/email/connect`                  | Yes           | Configure SMTP email                 |
| GET    | `/stations/:stationId/integrations/email/status`                   | Yes           | Email integration status             |
| POST   | `/stations/:stationId/integrations/email/disconnect`               | Yes           | Disconnect email integration         |
| PATCH  | `/stations/:stationId/integrations/:integrationId/settings`        | Yes           | Update integration settings          |

---

### Notifications

| Method | Endpoint                                  | Auth Required | Description                             |
|--------|-------------------------------------------|---------------|-----------------------------------------|
| GET    | `/notifications`                          | Yes           | List the user's notifications           |
| PATCH  | `/notifications/:notificationId/read`     | Yes           | Mark one notification as read           |
| PATCH  | `/notifications/read-all`                 | Yes           | Mark all notifications as read          |
| DELETE | `/notifications/:notificationId`          | Yes           | Delete a notification                   |

Query parameters for `GET /notifications`: `page`, `limit`, `unreadOnly` (boolean).

---

### Usage

| Method | Endpoint                                         | Auth Required | Description                        |
|--------|--------------------------------------------------|---------------|------------------------------------|
| GET    | `/stations/:stationId/usage/summary`             | Yes           | Aggregated usage summary           |
| GET    | `/stations/:stationId/usage/records`             | Yes           | Paginated usage records            |
| GET    | `/stations/:stationId/usage/cost-breakdown`      | Yes           | Cost breakdown by period           |

Query parameters:

- `summary`: `startDate`, `endDate` (ISO date strings)
- `records`: `page`, `limit`, `metricType` (`tokens` | `compute` | `storage`)
- `cost-breakdown`: `period` (`day` | `week` | `month` | `year`, default `month`)

---

### Billing

| Method | Endpoint                     | Auth Required | Description                                 |
|--------|------------------------------|---------------|---------------------------------------------|
| GET    | `/billing/plans`             | Yes           | List available subscription plans           |
| POST   | `/billing/subscription`      | Yes           | Create / activate a subscription            |
| PATCH  | `/billing/subscription`      | Yes           | Upgrade or downgrade subscription           |
| GET    | `/billing/invoices`          | Yes           | List billing invoices                       |
| POST   | `/billing/payment-method`    | Yes           | Add a Stripe payment method                 |

Query parameters for `GET /billing/invoices`: `page`, `limit`.

---

### BTCY

| Method | Endpoint                             | Auth Required | Description                         |
|--------|--------------------------------------|---------------|-------------------------------------|
| POST   | `/billing/btcy/connect-wallet`       | Yes           | Connect an EVM wallet               |
| GET    | `/billing/btcy/balance`              | Yes           | Get BTCY token balance              |
| POST   | `/billing/btcy/pay`                  | Yes           | Make a payment with BTCY tokens     |
| GET    | `/billing/btcy/transactions`         | Yes           | List BTCY transaction history       |

Query parameters for `GET /billing/btcy/transactions`: `page`, `limit`.

---

### Webhooks

| Method | Endpoint                                               | Auth Required | Description                     |
|--------|--------------------------------------------------------|---------------|---------------------------------|
| POST   | `/stations/:stationId/webhooks`                        | Yes           | Register an outbound webhook    |
| GET    | `/stations/:stationId/webhooks`                        | Yes           | List webhooks                   |
| GET    | `/stations/:stationId/webhooks/:webhookId`             | Yes           | Get a webhook                   |
| PATCH  | `/stations/:stationId/webhooks/:webhookId`             | Yes           | Update a webhook                |
| DELETE | `/stations/:stationId/webhooks/:webhookId`             | Yes           | Delete a webhook                |

Supported event types: `task.created`, `task.completed`, `task.failed`, `task.cancelled`, `agent.error`, `member.invited`, `schedule.triggered`.

---

### Admin

All admin endpoints require a JWT issued to a user with role `admin` or `super_admin`.

| Method | Endpoint                   | Auth Required        | Description                              |
|--------|----------------------------|----------------------|------------------------------------------|
| GET    | `/admin/stations`          | Yes (admin)          | List all stations platform-wide          |
| GET    | `/admin/tasks`             | Yes (admin)          | List all tasks platform-wide             |
| GET    | `/admin/agent-runs`        | Yes (admin)          | List all agent run records               |
| GET    | `/admin/failures`          | Yes (admin)          | List all failures (tasks + agent runs)   |
| GET    | `/admin/system-health`     | Yes (admin)          | System health check                      |
| GET    | `/admin/audit-logs`        | Yes (admin)          | Query the audit log                      |

Query parameters:

- `admin/stations`: `page`, `limit`, `search`, `status`
- `admin/tasks`: `page`, `limit`, `status`, `startDate`, `endDate`
- `admin/agent-runs`: `page`, `limit`, `status`
- `admin/failures`: `page`, `limit`
- `admin/audit-logs`: `page`, `limit`, `stationId`, `userId`, `action`, `startDate`, `endDate`

---

## WebSocket / SSE

### Server-Sent Events (SSE)

Two SSE endpoints are available for real-time streaming. Connect with `Accept: text/event-stream` and a valid Bearer token.

| Endpoint                                                 | Description                                     |
|----------------------------------------------------------|-------------------------------------------------|
| `GET /stations/:stationId/tasks/:taskId/stream`          | Real-time updates for a single task             |
| `GET /stations/:stationId/live/events`                   | All live events in a station (heartbeat every 5 s) |

**Sample event payload:**
```json
{
  "taskId": "task_def456",
  "stationId": "station_abc123",
  "timestamp": "2026-03-21T10:06:30.000Z",
  "type": "step.completed",
  "data": {
    "stepId": "step_1",
    "output": "Summary generated successfully."
  }
}
```

### WebSocket (Socket.IO)

The server mounts a Socket.IO server on the same HTTP port (`/`). Clients can connect and join station rooms for push notifications. Namespace and event naming conventions are internal to the `agent-runtime` module.

---

## Sprint Roadmap

| Sprint | Focus Area                                       | Status         |
|--------|--------------------------------------------------|----------------|
| 1      | Auth, Users, Stations, Members                   | Complete       |
| 2      | Agent Catalog, Station Agents, Task Orchestrator | Complete       |
| 3      | Memory, Schedules, Reports                       | Complete       |
| 4      | Integrations (Telegram, Discord, Slack, Email)   | Complete       |
| 5      | Notifications, Usage, Billing, BTCY              | Complete       |
| 6      | Webhooks, Admin, Audit Logs                      | Complete       |
| 7      | WebSocket gateway, SSE streaming                 | Complete       |
| 8      | Multi-model support, agent marketplace           | Planned        |
| 9      | OAuth provider integrations (Google, GitHub)     | Planned        |
| 10     | Fine-tuned model hosting and deployment          | Planned        |
| 11     | Advanced analytics dashboard (time-series data)  | Planned        |
| 12     | Enterprise SSO (SAML / OIDC)                     | Planned        |

---

## How It Works — Plain & Simple

These flows show what actually happens when you or your team uses OpenClaw Station. No technical terms — just people, jobs, and results.

---

### Scenario 1 — One Task, One Agent

> You ask the platform to research a topic and write a summary for you.

```
  YOU
   │
   │  "Research the top 5 competitors in our market
   │   and write me a one-page summary."
   │
   ▼
┌─────────────────────────────────┐
│        YOUR WORKSPACE           │
│  (your Station on the platform) │
└────────────────┬────────────────┘
                 │
                 │  Your request lands in the work inbox
                 │
                 ▼
        ┌─────────────────┐
        │   RESEARCH      │
        │     AGENT       │
        │                 │
        │  1. Reads your  │
        │     request     │
        │                 │
        │  2. Searches    │
        │     the web     │
        │                 │
        │  3. Reads &     │
        │     compares    │
        │     the results │
        │                 │
        │  4. Writes your │
        │     summary     │
        └────────┬────────┘
                 │
                 │  Work is done
                 │
                 ▼
┌─────────────────────────────────┐
│           YOUR REPORT           │
│  Ready in your dashboard        │
│  + notification sent to you     │
└─────────────────────────────────┘
                 │
                 ▼
              YOU
         read the summary
```

**What you see:** A finished one-page competitor summary appears in your dashboard. You get a notification when it is ready. The whole thing ran on its own — you did not need to do anything after pressing "Run".

---

### Scenario 2 — Two Tasks, One Agent (Back to Back)

> You queue two separate jobs. The agent finishes one, then picks up the next.

```
  YOU
   │
   ├── Task 1: "Write a welcome email for new customers"
   │
   └── Task 2: "Summarise last week's sales data"
   │
   ▼
┌─────────────────────────────────┐
│           WORK INBOX            │
│  [ Task 1 ]  [ Task 2 ]         │
│  (waiting in line)              │
└────────────────┬────────────────┘
                 │
        ┌────────▼────────┐
        │   WRITER AGENT  │
        │                 │
        │  picks up       │◀─── working on Task 1
        │  Task 1 first   │
        │                 │
        │  drafts your    │
        │  welcome email  │
        └────────┬────────┘
                 │  Task 1 done → report saved
                 │
        ┌────────▼────────┐
        │   WRITER AGENT  │
        │                 │
        │  picks up       │◀─── now working on Task 2
        │  Task 2 next    │
        │                 │
        │  reads the data │
        │  writes summary │
        └────────┬────────┘
                 │  Task 2 done → report saved
                 │
                 ▼
┌─────────────────────────────────┐
│        YOUR DASHBOARD           │
│  ✓ Welcome email — ready        │
│  ✓ Sales summary — ready        │
└─────────────────────────────────┘
```

**What you see:** Both results appear in your dashboard one after the other. The agent worked through your to-do list automatically, like a staff member clearing their inbox.

---

### Scenario 3 — Two Agents Working Together

> You give a bigger job. A manager agent splits it up and hands pieces to specialist agents.

```
  YOU
   │
   │  "Research our top 3 competitors, then write
   │   a full report with an executive summary."
   │
   ▼
┌──────────────────────────────────────────────────────┐
│                   YOUR WORKSPACE                      │
└──────────────────────────┬───────────────────────────┘
                           │
                           ▼
              ┌────────────────────────┐
              │     MANAGER AGENT      │
              │                        │
              │  Reads your request    │
              │  and makes a plan:     │
              │                        │
              │  Step 1 → Research     │
              │  Step 2 → Write report │
              └─────────┬──────────────┘
                        │
           splits the work into two jobs
                        │
          ┌─────────────┴─────────────┐
          │                           │
          ▼                           ▼
 ┌─────────────────┐       ┌─────────────────────┐
 │  RESEARCH AGENT │       │    WRITER AGENT      │
 │                 │       │                      │
 │  Searches the   │       │  (waiting for the    │
 │  web for info   │       │   research to finish)│
 │  on each        │       │                      │
 │  competitor     │       └──────────┬───────────┘
 │                 │                  │
 │  Gathers facts, │                  │
 │  prices,        │   findings ──────▶  now has everything
 │  strengths,     │   handed over        it needs to write
 │  weaknesses     │
 └────────┬────────┘
          │
          │  research complete
          │
          ▼
 ┌─────────────────────┐
 │    WRITER AGENT     │
 │                     │
 │  Takes the research │
 │  findings           │
 │                     │
 │  Writes the full    │
 │  competitor report  │
 │                     │
 │  Adds an executive  │
 │  summary on top     │
 └──────────┬──────────┘
            │
            ▼
┌──────────────────────────────────────────────────────┐
│                  YOUR DASHBOARD                       │
│  Full competitor report — ready to read or download  │
│  You get a notification the moment it is done        │
└──────────────────────────────────────────────────────┘
```

**What you see:** One finished report in your dashboard. Behind the scenes two agents collaborated — one did the digging, the other did the writing — and the manager made sure they worked in the right order. You just gave one instruction.

---

### Scenario 4 — indexx.ai: Daily Market Index Report

> Your station monitors the indexx.ai market indices and delivers a daily briefing to your team automatically every morning.

```
  SCHEDULE TRIGGER
  (every day at 8:00 AM)
         │
         ▼
┌─────────────────────────────────┐
│        YOUR STATION              │
│   "indexx.ai Daily Monitor"     │
└────────────────┬────────────────┘
                 │
                 ▼
      ┌──────────────────────┐
      │    MANAGER AGENT      │
      │                       │
      │  Wakes up at 8 AM     │
      │  Breaks job into:     │
      │  Step 1 → Research    │
      │  Step 2 → Write       │
      │  Step 3 → Send alert  │
      └──────────┬────────────┘
                 │
       ┌─────────┴─────────┐
       │                   │
       ▼                   ▼
┌────────────────┐  ┌──────────────────────┐
│ RESEARCH AGENT │  │    WRITER AGENT       │
│                │  │  (waits for research) │
│ Pulls today's  │  │                       │
│ index data     │  └──────────┬────────────┘
│ from indexx.ai │             │
│                │   data ─────▶
│ Checks top     │   handed over
│ movers,        │
│ volume,        │
│ % changes      │
└────────┬───────┘
         │
         │  findings ready
         ▼
┌──────────────────────┐
│    WRITER AGENT       │
│                       │
│  Formats findings     │
│  into a daily         │
│  market briefing      │
│                       │
│  Highlights top       │
│  gainers, losers,     │
│  and key signals      │
└──────────┬────────────┘
           │
           ▼
┌─────────────────────────────────────────┐
│         COMMUNICATOR AGENT               │
│                                          │
│  Sends the briefing to your             │
│  Telegram channel or team email          │
│  at exactly 8 AM every morning           │
└──────────────────┬──────────────────────┘
                   │
                   ▼
      YOUR TEAM receives the briefing
         before markets open
```

**What you see:** Every morning, without doing anything, your Telegram or email gets a clean market briefing — index performance, top movers, key signals — all written in plain language, ready before your team starts work.

---

### Scenario 5 — BitcoinYay Mobile: User Opens App, Logs In, Starts Mining

> A new user downloads the BitcoinYay app. The station handles their onboarding, starts their mining session, and keeps them updated automatically.

```
  NEW USER
     │
     │  Opens BitcoinYay app
     │  Creates account / logs in
     │
     ▼
┌─────────────────────────────────┐
│      BITCOINYAY STATION          │
│  (one station per active user)   │
└────────────────┬────────────────┘
                 │
                 ▼
      ┌──────────────────────┐
      │    MANAGER AGENT      │
      │                       │
      │  User just logged in  │
      │  Creates a plan:      │
      │                       │
      │  Step 1 → Onboard     │
      │  Step 2 → Start mine  │
      │  Step 3 → Monitor     │
      │  Step 4 → Notify      │
      └──────────┬────────────┘
                 │
                 ▼
┌─────────────────────────────────┐
│         ONBOARDING AGENT         │
│                                  │
│  Checks if user is new           │
│  Sends welcome message           │
│  via Telegram or in-app          │
│  Explains how mining works       │
│  Answers first-time questions    │
└──────────────┬───────────────────┘
               │  onboarding done
               ▼
┌─────────────────────────────────┐
│          MINING AGENT            │
│                                  │
│  Registers user's mining         │
│  session on the network          │
│  Confirms session is live        │
│  Records session start time      │
└──────────────┬───────────────────┘
               │  mining running
               ▼
┌─────────────────────────────────┐
│          MONITOR AGENT           │
│                                  │
│  Watches the mining session      │
│                                  │
│  Every hour checks:              │
│  • Is session still active?      │
│  • How many rewards earned?      │
│  • Any issues or drops?          │
└──────────────┬───────────────────┘
               │
               ▼
┌─────────────────────────────────┐
│       COMMUNICATOR AGENT         │
│                                  │
│  Sends the user a push or        │
│  Telegram message:               │
│                                  │
│  "Your mining session is live.   │
│   You have earned X BTCY so far. │
│   Keep the app open to continue."│
└──────────────┬───────────────────┘
               │
               ▼
            USER
   stays informed, stays engaged
```

**What you see:** From the moment you log in, the station handles everything — welcome message, session start, hourly check-ins, and reward updates — all without any extra taps or actions from the user.

---

### Scenario 6 — WallStreet.indexx.ai: Investor Buys a Token / Enterprise Launches a Token

> Two separate flows run inside the same station — one for investors researching and buying tokens, one for enterprises raising funds by launching a token project.

```
─────────────────────────────────────────────────────────
  FLOW A: INVESTOR WANTS TO BUY A TOKEN
─────────────────────────────────────────────────────────

  INVESTOR
     │
     │  "Research the XYZ token project
     │   and tell me if it is worth buying."
     │
     ▼
┌──────────────────────────────────┐
│       WALLSTREET STATION          │
└──────────────────┬───────────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │    RESEARCH AGENT    │
        │                      │
        │  Reads the project   │
        │  whitepaper          │
        │                      │
        │  Checks team,        │
        │  tokenomics,         │
        │  use case, risks     │
        │                      │
        │  Looks at similar    │
        │  projects on market  │
        └──────────┬───────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │    ANALYST AGENT     │
        │                      │
        │  Scores the project: │
        │  • Team strength     │
        │  • Market fit        │
        │  • Risk level        │
        │  • Price potential   │
        └──────────┬───────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │    WRITER AGENT      │
        │                      │
        │  Writes a short      │
        │  investment brief:   │
        │  Pros, Cons,         │
        │  Recommendation      │
        └──────────┬───────────┘
                   │
                   ▼
           INVESTOR receives
           a clear buy / hold /
           avoid recommendation

─────────────────────────────────────────────────────────
  FLOW B: ENTERPRISE LAUNCHES A TOKEN TO RAISE FUNDS
─────────────────────────────────────────────────────────

  ENTERPRISE
     │
     │  "We are launching the ABC token.
     │   Help us create the pitch materials
     │   and reach out to investors."
     │
     ▼
┌──────────────────────────────────┐
│       WALLSTREET STATION          │
└──────────────────┬───────────────┘
                   │
                   ▼
        ┌──────────────────────┐
        │    MANAGER AGENT      │
        │                       │
        │  Breaks the job:      │
        │  Step 1 → Research    │
        │  Step 2 → Write docs  │
        │  Step 3 → Outreach    │
        └──────────┬────────────┘
                   │
        ┌──────────┴──────────┐
        │                     │
        ▼                     ▼
┌───────────────┐    ┌────────────────────┐
│ RESEARCH AGENT│    │   WRITER AGENT      │
│               │    │  (waits for data)   │
│ Looks at what │    └──────────┬──────────┘
│ similar token │               │
│ launches did  │   data ───────▶
│               │   handed over
│ Identifies    │
│ target        │
│ investor type │
└───────┬───────┘
        │
        │  research complete
        ▼
┌────────────────────┐
│   WRITER AGENT      │
│                     │
│  Writes:            │
│  • Project summary  │
│  • Token use case   │
│  • Fundraise target │
│  • Investor FAQ     │
└──────────┬──────────┘
           │
           ▼
┌────────────────────────────────────┐
│       COMMUNICATOR AGENT            │
│                                     │
│  Sends the pitch deck summary       │
│  to a list of potential investors   │
│  via email or Telegram              │
│                                     │
│  Follows up automatically           │
│  after 3 days if no reply           │
└──────────────┬──────────────────────┘
               │
               ▼
          ENTERPRISE
      gets investor responses
       tracked in one place
```

**What you see (investor):** You ask one question, get back a clear research brief with a recommendation — no digging through whitepapers yourself.

**What you see (enterprise):** You describe your project, and the station writes your pitch materials and handles investor outreach — your team only steps in to close the conversation.

---

### Scenario 7 — emmm.io: Prediction Market (Polymarket-style)

> Users create prediction questions, other users place positions, and the station monitors real-world events to resolve the market and notify everyone of the outcome.

```
─────────────────────────────────────────────────────────
  FLOW A: MARKET CREATOR
─────────────────────────────────────────────────────────

  MARKET CREATOR
     │
     │  "Will Bitcoin reach $100,000
     │   before the end of this month?"
     │
     ▼
┌──────────────────────────────────┐
│          EMMM.IO STATION          │
└──────────────────┬───────────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │    RESEARCH AGENT    │
        │                      │
        │  Checks if this      │
        │  question is         │
        │  clear and           │
        │  resolvable          │
        │                      │
        │  Finds current BTC   │
        │  price and distance  │
        │  to target           │
        └──────────┬───────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │    WRITER AGENT      │
        │                      │
        │  Formats the market  │
        │  description clearly │
        │  Adds resolution     │
        │  criteria:           │
        │  "Resolves YES if    │
        │   BTC price on       │
        │   Binance exceeds    │
        │   $100,000 by        │
        │   30 April 23:59 UTC"│
        └──────────┬───────────┘
                   │
                   ▼
           MARKET goes live
           on emmm.io for
           others to bet on

─────────────────────────────────────────────────────────
  FLOW B: BETTORS PLACE POSITIONS
─────────────────────────────────────────────────────────

  BETTOR
     │
     │  Browses open markets
     │  Asks: "Give me a quick
     │  analysis of this market
     │  before I decide."
     │
     ▼
┌──────────────────────────────────┐
│          EMMM.IO STATION          │
└──────────────────┬───────────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │    RESEARCH AGENT    │
        │                      │
        │  Pulls current BTC   │
        │  price and trend     │
        │  Checks expert       │
        │  sentiment online    │
        │  Looks at historical │
        │  similar events      │
        └──────────┬───────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │    ANALYST AGENT     │
        │                      │
        │  Gives a probability │
        │  estimate:           │
        │  "Based on current   │
        │   trend: ~38% YES,   │
        │   ~62% NO"           │
        └──────────┬───────────┘
                   │
                   ▼
           BETTOR makes
           an informed decision

─────────────────────────────────────────────────────────
  FLOW C: MARKET RESOLVES
─────────────────────────────────────────────────────────

  MONITOR AGENT
  (runs at market deadline)
         │
         │  Checks the resolution
         │  data source
         │  (Binance BTC price
         │   at 23:59 UTC)
         │
         ▼
  ┌─────────────────────────┐
  │  Price = $97,400         │
  │  Target was $100,000     │
  │  Result: NO              │
  └──────────┬──────────────┘
             │
             ▼
  ┌─────────────────────────┐
  │   COMMUNICATOR AGENT     │
  │                          │
  │  Notifies all bettors    │
  │  via Telegram:           │
  │                          │
  │  "Market resolved: NO    │
  │   BTC closed at $97,400  │
  │   Your payout has been   │
  │   credited to your       │
  │   emmm.io wallet."       │
  └──────────┬───────────────┘
             │
             ▼
      ALL PARTICIPANTS
   notified instantly,
   payouts processed
```

**What you see (creator):** Your question is automatically formatted with clear resolution criteria and goes live — no manual setup.

**What you see (bettor):** Before placing any position you get an instant AI analysis of the market odds based on real data — not just gut feeling.

**What you see (after resolution):** The moment the market closes, every participant gets a Telegram or in-app notification with the outcome and their payout — no waiting, no checking.

---

## LLM Architecture

OpenClaw Station supports two LLM key modes. The same agent runtime handles both — only the key resolution step differs.

### Key Modes

```
┌─────────────────────────────────────────────────────────────────┐
│                      KEY MODE SELECTION                          │
│                                                                 │
│   Mode A: Company-Owned Key           Mode B: BYOK              │
│   Platform holds the API keys         User brings their own key │
│   → Charge users via credits          → Platform charges        │
│   → Mark up LLM usage cost              infra/feature fee only  │
└─────────────────────────────────────────────────────────────────┘
```

### Full Architecture Flow

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                         OPENCLAW STATION PLATFORM                            │
│                                                                              │
│  ┌─────────────┐   ┌────────────────────────────────────────────────────┐   │
│  │  Frontend   │   │               NestJS API Gateway                    │   │
│  │ (React /    │──▶│  auth │ stations │ agents │ tasks │ billing │ usage │   │
│  │  Next.js)   │   └─────────────────────────┬──────────────────────────┘   │
│  └─────────────┘                             │                               │
│        │                                     ▼                               │
│        │                      ┌──────────────────────────┐                  │
│        │                      │      KEY MODE ROUTER      │                  │
│        │                      │  reads: user.llm_key_mode │                  │
│        │                      │  ┌───────────┬──────────┐ │                  │
│        │                      │  │  MODE A   │  MODE B  │ │                  │
│        │                      │  │  Company  │   BYOK   │ │                  │
│        │                      │  │  Key      │  User    │ │                  │
│        │                      │  │           │  Key     │ │                  │
│        │                      │  └─────┬─────┴────┬─────┘ │                  │
│        │                      └────────┼──────────┼───────┘                  │
│        │                               │          │                           │
│        │                   ┌───────────▼──┐  ┌────▼──────────────┐          │
│        │                   │   Platform   │  │ Tencent Secrets   │          │
│        │                   │   Env Vars   │  │ Manager (Vault)   │          │
│        │                   │ CLAUDE_KEY   │  │ user_id→enc_key   │          │
│        │                   │ OPENAI_KEY   │  └────────┬──────────┘          │
│        │                   └──────┬───────┘           │                      │
│        │                          └──────────┬─────────┘                     │
│        │                                     ▼                               │
│        │                      ┌──────────────────────────┐                  │
│        │                      │       LLM GATEWAY         │                  │
│        │                      │  ┌──────────┬──────────┐  │                  │
│        │                      │  │  Claude  │  OpenAI  │  │                  │
│        │                      │  │   API    │   API    │  │                  │
│        │                      │  └──────────┴──────────┘  │                  │
│        │                      │  • token counter           │                  │
│        │                      │  • rate limiter            │                  │
│        │                      │  • cost logger             │                  │
│        │                      └──────────────────────────┘                  │
│        │                                     │                               │
│        │                                     ▼                               │
│   Socket.IO               ┌────────────────────────────────┐                │
│   (live updates) ◀────────│      TASK ORCHESTRATOR          │                │
│        │                  │                                 │                │
│        │                  │  ┌───────────────────────────┐  │                │
│        │                  │  │      Manager Agent         │  │                │
│        │                  │  │  • parses user task        │  │                │
│        │                  │  │  • creates subtask plan    │  │                │
│        │                  │  │  • assigns to workers      │  │                │
│        │                  │  └────────────┬──────────────┘  │                │
│        │                  │               │  BullMQ Queue    │                │
│        │                  │  ┌────────────▼─────────────┐   │                │
│        │                  │  │      Worker Agents        │   │                │
│        │                  │  │  Research │ Writer        │   │                │
│        │                  │  │  Monitor  │ Biz Support   │   │                │
│        │                  │  └──────────────────────────┘   │                │
│        │                  └────────────────────────────────┘                │
│        │                                     │                               │
│        │          ┌──────────────────────────────────────────────┐          │
│        │          │                  DATA LAYER                   │          │
│        │          │  MongoDB       │  Redis        │  COS         │          │
│        │          │  (documents)   │  (queue /     │  (files /    │          │
│        │          │                │   sessions)   │   reports)   │          │
│        │          └──────────────────────────────────────────────┘          │
│        └─────────────────────────────────────────────────────────────────── │
└──────────────────────────────────────────────────────────────────────────────┘
```

### Task Execution Flow

```
[1]  User submits task via frontend
       │
[2]  API validates JWT + RBAC
       │
[3]  Key Mode Router resolves LLM key
       ├── Mode A: reads CLAUDE_KEY / OPENAI_KEY from platform env
       └── Mode B: decrypts user's stored key from Secrets Manager
       │
[4]  Manager Agent invoked
       │  → LLM call: "Break this task into subtasks"
       │  → returns structured plan
       │
[5]  Subtasks pushed to BullMQ queues
       │
[6]  Worker agents consume queue
       │  → each worker makes LLM calls with resolved key
       │  → tool calls: web search, file read, Telegram, etc.
       │
[7]  Results returned to Manager Agent
       │  → LLM aggregates → final output generated
       │
[8]  Usage recorded in usage_records
       ├── Mode A: deduct credits from user billing account
       └── Mode B: log tokens only (user pays their own LLM bill)
       │
[9]  Report saved to MongoDB + COS
       Socket.IO pushes live update to frontend dashboard
```

---

## How BYOK Works

Users paste their own Claude or OpenAI API key in settings. The platform never stores the key in plaintext.

```
User → POST /integrations/llm-key
       body: { provider: "claude", key: "sk-ant-..." }

Backend:
  1. Validates key format
  2. Makes a test call to verify it works
  3. Encrypts with AES-256 using platform master secret
  4. Stores in secrets_metadata collection:
       { user_id, provider, encrypted_key, key_hint: "sk-ant-...xxxx" }
  5. Syncs encrypted blob to Tencent Secrets Manager

At runtime:
  1. Key Mode Router checks user.llm_key_mode === "byok"
  2. Fetches encrypted_key from secrets_metadata
  3. Decrypts in-memory only (never logged, never stored decrypted)
  4. Passes to LLM client for that single request
  5. Cleared from memory after response
```

Required additional env variable for BYOK:

```env
SECRETS_MASTER_KEY=<32-byte hex — used to AES-256 encrypt user API keys>
```

---

## Cost Analysis

### Infrastructure — Tencent Cloud (monthly)

| Component | Spec | Est. Cost/mo |
|-----------|------|-------------|
| CVM — API Server | 4 vCPU / 8 GB RAM | ~$60 |
| CVM — Worker Pool | 2× 2 vCPU / 4 GB | ~$70 |
| TencentDB MongoDB | M10, 10 GB | ~$80 |
| TencentDB Redis | 1 GB Standard | ~$25 |
| COS (object storage) | 100 GB | ~$5 |
| CLB (load balancer) | Standard | ~$15 |
| CLS (log service) | 5 GB/day | ~$10 |
| Secrets Manager | per-secret | ~$5 |
| **Total Infra** | | **~$270/mo** |

### LLM Cost Per Task (Company Key Mode)

A full multi-agent task = 1 Manager call + 2–4 Worker calls ≈ 15k–40k tokens total.

| Model | Input $/1M tok | Output $/1M tok | Cost per task (~25k tok) |
|-------|---------------|-----------------|--------------------------|
| Claude Sonnet 4.6 | $3.00 | $15.00 | ~$0.05 – $0.18 |
| Claude Haiku 4.5 | $0.80 | $4.00 | ~$0.015 – $0.05 |
| GPT-4o | $2.50 | $10.00 | ~$0.04 – $0.15 |
| GPT-4o mini | $0.15 | $0.60 | ~$0.003 – $0.01 |

> **Recommended split:** Claude Haiku 4.5 for all workers + Claude Sonnet 4.6 for manager only — reduces LLM cost by ~60% vs all-Sonnet.

### Micro POC — Cost for 2 Users

To validate OpenClaw Station with minimal cost and risk, start with 2 active users
(internal testing or early adopters).

This is enough to test:

- Agent workflows
- Task execution
- Integrations (Telegram, Slack, Email)
- End-to-end system stability

#### POC Assumptions

| Parameter | Value |
|-----------|-------|
| Users | 2 |
| Tasks per user per day | 5-10 |
| Total tasks/day | 10-20 |
| Tokens per task | 3K-8K |
| Model | `gpt-4o` (or similar) |

#### LLM Cost

| Metric | Range |
|--------|-------|
| Total tokens/day | ~30K-150K |

| Period | Cost |
|--------|------|
| Per day | $0.20-$2 |
| Per month | $6-$60 |

Typical real usage: ~$10-$30/month

#### Infrastructure Cost

| Component | Setup | Cost |
|-----------|-------|------|
| Backend | Small instance / VPS | $5-$15 |
| MongoDB | Free tier | $0 |
| Redis | Optional | $0 |
| Storage | Minimal | ~$1 |

Infra total: ~$5-$20/month

#### Integrations

| Service | Cost |
|---------|------|
| Telegram / Discord / Slack | Free |
| Email (SMTP) | Free or ~$5 |

#### Total Monthly Cost Summary

| Category | Monthly Cost |
|----------|--------------|
| LLM Usage | $10-$30 |
| Infrastructure | $5-$20 |
| Integrations | $0-$5 |
| **Total** | **$15-$50** |

### Monthly Cost by Scale

| Users | Mode | LLM Cost | Infra | Total Cost | Revenue (est.) | Margin |
|-------|------|----------|-------|------------|----------------|--------|
| 50 users, 5 tasks/day | Company key | ~$300 | $270 | ~$570 | ~$1,500 | ~62% |
| 100 users, 5 tasks/day | Company key | ~$600 | $350 | ~$950 | ~$3,000 | ~68% |
| 200 users | BYOK only | $0 | $400 | ~$400 | ~$2,000 | ~80% |
| 200 users mixed | Hybrid | ~$300 | $400 | ~$700 | ~$3,500 | ~80% |

---

## Pricing Models

### Option A — Credit Packs (Company Key)

Platform buys LLM at cost and sells to users at 3–5× markup.

```
Actual LLM cost per task (Haiku workers + Sonnet manager): ~$0.04
Charge user per task:                                       ~$0.15–$0.20
Gross margin:                                               ~70–80%

Credit packs:
  Starter:    100 credits →  $15  (~100 tasks)
  Pro:        500 credits →  $49  (~500 tasks)
  Business:  2000 credits → $149  (~2000 tasks)
```

### Option B — Monthly Subscription + Credits

```
Free:    10 tasks/month · BYOK only
Starter: $19/mo → 200 task credits + company key
Pro:     $49/mo → 800 task credits + priority queue
Team:    $99/mo → 3000 task credits + 5 seats + API access
```

### Option C — BYOK Flat Fee (Zero LLM cost to platform)

```
BYOK Basic:  $9/mo  → unlimited tasks, user pays their own LLM bill
BYOK Pro:   $19/mo  → + memory, schedules, webhooks, multi-channel

Break-even: ~30 paying BYOK users covers full infra cost
```

### V1 Recommendation

**Launch with BYOK + flat fee (Option C).**

- Zero LLM cost risk at launch
- `secrets_metadata` and `usage_records` collections are already in the schema
- Switch to company-key credits later by flipping `user.llm_key_mode` — no schema changes needed
- Only new code needed: a thin **LLM Gateway service** inside `agent-runtime` that resolves the key, proxies the call, and writes token counts to `usage_records`

---

## Postman Collection

A fully pre-configured Postman Collection v2.1 is available at the root of this repository:

```
openclaw-station.postman_collection.json
```

Import it into Postman and set the following collection variables:

| Variable         | Description                              |
|------------------|------------------------------------------|
| `baseUrl`        | API base URL (default: `http://localhost:3000/api/v1`) |
| `token`          | Your JWT access token (from login/register) |
| `stationId`      | ID of the station to test against        |
| `taskId`         | ID of a task                             |
| `agentId`        | ID of a catalog or station agent         |
| `reportId`       | ID of a report                           |
| `memberId`       | ID of a station member                   |
| `scheduleId`     | ID of a schedule                         |
| `notificationId` | ID of a notification                     |
| `stationAgentId` | ID of an installed station agent         |
| `memoryId`       | ID of a memory entry                     |
| `integrationId`  | ID of a station integration              |
| `webhookId`      | ID of a webhook registration             |
| `templateId`     | ID of a station template                 |

---

## License

MIT — see [LICENSE](./LICENSE) for details.
