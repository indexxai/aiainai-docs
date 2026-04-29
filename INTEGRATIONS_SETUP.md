# AI Station — Integrations Setup Guide

This document covers every integration supported by AI Station: what credentials you need, where to get them, and which integrations require **app publishing / verification** before they work for other users in production.

---

## Table of Contents

1. [Telegram](#1-telegram)
2. [Discord](#2-discord)
3. [WhatsApp](#3-whatsapp)
4. [BTCY Chat](#4-btcy-chat)
5. [Slack](#5-slack)
6. [Signal](#6-signal)
7. [iMessage](#7-imessage)
8. [Anthropic Claude API](#8-anthropic-claude-api)
9. [Obsidian](#9-obsidian)
10. [OpenAI GPT](#10-openai-gpt)
11. [Spotify](#11-spotify)
12. [Philips Hue](#12-philips-hue)
13. [X Corp (Twitter/X)](#13-x-corp-twitterx)
14. [Browser (Playwright)](#14-browser-playwright)
15. [Google Gmail](#15-google-gmail)
16. [GitHub](#16-github)
17. [Email (SMTP)](#17-email-smtp)
18. [Google Calendar](#18-google-calendar)
19. [Environment Variables Reference](#environment-variables-reference)
20. [App Publishing Requirements Summary](#app-publishing-requirements-summary)

---

<a id="1-telegram"></a>
## 1. Telegram

**Requires app publishing?** No — Telegram bots work without any review.

### Backend env vars
```env
# Required: used to auto-register webhooks per station on connect
APP_URL=https://yourdomain.com

# Optional global fallback (per-station tokens are stored in the DB)
TELEGRAM_BOT_TOKEN=your_telegram_bot_token
```

### Setup
1. Open Telegram and message **@BotFather**.
2. Send `/newbot`, choose a name, and copy the **Bot Token** (format: `123456789:AAF...xYZ`).
3. (Optional) Add the bot to a group/channel and get the **Chat ID** by forwarding a message to **@userinfobot**.
4. In AI Station Settings → Integrations → Telegram, paste the token and chat ID.
5. Click **Connect** — the webhook is **registered automatically** for that station's bot token.

> **Multi-user / multi-station:** Every station stores its own bot token independently. On connect, AI Station calls `setWebhook` with the URL `APP_URL/api/v1/stations/<STATION_ID>/integrations/telegram/webhook` so each bot routes to the correct station. On disconnect, `deleteWebhook` is called automatically. No manual webhook setup is required.

> **Local development:** Set `APP_URL=https://<your-ngrok-id>.ngrok.io` (or any public tunnel) in the backend `.env` so Telegram can reach your local server. If `APP_URL` is not set, the webhook step is silently skipped and you can set it manually.

---

<a id="2-discord"></a>
## 2. Discord

**Requires app publishing?** No — Discord bots don't need review for basic usage.

### Setup
1. Go to [discord.com/developers/applications](https://discord.com/developers/applications).
2. Click **New Application**, then go to **Bot** → **Add Bot**.
3. Copy the **Bot Token**. Reset it if you can't see it.
4. Go to **OAuth2 → URL Generator**, select `bot` scope + `Send Messages`, `Read Message History` permissions. Use the URL to invite the bot to your server.
5. Enable **Developer Mode** in Discord (Settings → Advanced). Right-click your server → **Copy Server ID** (Guild ID). Right-click a channel → **Copy Channel ID**.
6. In AI Station, paste the Bot Token, Guild ID, and Channel ID.

### Interactions Endpoint
After connecting, copy the **Interactions Endpoint URL** shown in the UI and paste it in: Discord Developer Portal → Your App → **General Information → Interactions Endpoint URL**.

---

<a id="3-whatsapp"></a>
## 3. WhatsApp

**Requires app publishing?** **Yes** — for Meta Cloud API, your Meta app must go through App Review for production use. For personal use (test accounts), you can test with up to 5 phone numbers without review.

### Option A: Meta Cloud API
1. Go to [developers.facebook.com](https://developers.facebook.com) → **My Apps → Create App**.
2. Add the **WhatsApp** product.
3. Under WhatsApp → API Setup, find your **Phone Number ID** and generate a **System User Token** (permanent) in Business Settings.
4. Set **WHATSAPP_VERIFY_TOKEN** in your backend `.env` to a random string.
5. Register the webhook: `https://yourdomain.com/api/v1/stations/<STATION_ID>/integrations/whatsapp/webhook`

### Option B: QR Linked Device (Personal)
- Uses [Baileys](https://github.com/WhiskeySockets/Baileys) to link a personal WhatsApp account via QR code — no Meta account needed.
- The backend env var `WHATSAPP_QR_AUTH_DIR` sets where session files are stored (default: `/tmp/ai-station-whatsapp`).

### Backend env vars
```env
WHATSAPP_VERIFY_TOKEN=your_custom_verify_token
WHATSAPP_APP_SECRET=your_meta_app_secret
WHATSAPP_QR_AUTH_DIR=/tmp/ai-station-whatsapp
```

---

<a id="4-btcy-chat"></a>
## 4. BTCY Chat

**Requires app publishing?** No — internal platform.

### Setup
- Enter the email address registered on [bitcoinyay.com](https://bitcoinyay.com).
- No additional credentials required; the station uses internal BTCY APIs.

---

<a id="5-slack"></a>
## 5. Slack

**Requires app publishing?** **Yes** — to distribute the app to other Slack workspaces, you must submit for Slack App Review. For personal/internal use in your own workspace, no review needed.

### Setup
1. Go to [api.slack.com/apps](https://api.slack.com/apps) → **Create New App → From Scratch**.
2. Under **OAuth & Permissions**, add Bot Token Scopes: `channels:history`, `channels:read`, `chat:write`, `groups:history`.
3. Click **Install App to Workspace**. Copy the **Bot User OAuth Token** (starts with `xoxb-`).
4. Under **Basic Information → App Credentials**, copy the **Signing Secret**.
5. Invite the bot to your channel: `/invite @YourBotName`.
6. Enable **Event Subscriptions** and set the Request URL to your webhook URL (shown after connecting in AI Station).

### Backend env vars
```env
SLACK_BOT_TOKEN=xoxb-your-token
SLACK_SIGNING_SECRET=your-signing-secret
```

---

<a id="6-signal"></a>
## 6. Signal

**Requires app publishing?** No — self-hosted via signal-cli.

### ⚠️ Important
Signal has **no official API**. This integration uses [signal-cli](https://github.com/AsamK/signal-cli), a community-maintained CLI tool.

### Setup

#### Install signal-cli (Java required)
```bash
# Download from https://github.com/AsamK/signal-cli/releases
# Example:
wget https://github.com/AsamK/signal-cli/releases/download/v0.13.0/signal-cli-0.13.0.tar.gz
tar xf signal-cli-0.13.0.tar.gz
sudo ln -sf $(pwd)/signal-cli-0.13.0/bin/signal-cli /usr/local/bin/signal-cli
```

#### Register your number
```bash
signal-cli -a +15551234567 register
signal-cli -a +15551234567 verify 123456   # SMS code
```

#### Start the REST daemon
```bash
signal-cli -a +15551234567 daemon --http   # Starts on http://localhost:8080
```

### In AI Station
- **signal-cli REST URL**: `http://localhost:8080`
- **Phone Number**: Your registered number (E.164 format, e.g. `+15551234567`)

---

<a id="7-imessage"></a>
## 7. iMessage

**Requires app publishing?** No — local macOS only.

### ⚠️ Limitations
- **macOS only** — the backend server must run on macOS.
- Send only — iMessage read access is not available through AppleScript in modern macOS.
- The Apple ID signed into the **Messages** app is used automatically.

### Setup
1. Ensure the backend runs on macOS with Messages.app open and signed in.
2. In AI Station, enter your Apple ID as a reference (no credentials are actually stored).

---

<a id="8-anthropic-claude-api"></a>
## 8. Anthropic Claude API

**Requires app publishing?** No — API key based.

### Setup
1. Go to [console.anthropic.com](https://console.anthropic.com).
2. Navigate to **API Keys** → **Create Key**.
3. Copy the key (starts with `sk-ant-api03-`).
4. In AI Station, paste the key and set your preferred default model.

### Available models
- `claude-opus-4-7` — Most capable
- `claude-sonnet-4-6` — Balanced
- `claude-haiku-4-5-20251001` — Fast and lightweight

### Pricing
Pay-as-you-go per token. See [anthropic.com/pricing](https://www.anthropic.com/pricing).

---

<a id="9-obsidian"></a>
## 9. Obsidian

**Requires app publishing?** No — local network only.

### ⚠️ Requirements
The [Local REST API](https://github.com/coddingtonbear/obsidian-local-rest-api) community plugin must be installed in Obsidian.

### Setup
1. Open Obsidian → **Settings → Community Plugins → Browse**.
2. Search for and install **"Local REST API"**.
3. Enable the plugin. An API key will be generated — copy it.
4. Note the port (default HTTPS: `27124`, HTTP: `27123`).
5. In AI Station:
   - **Base URL**: `https://127.0.0.1:27124` (or `http://127.0.0.1:27123` for HTTP mode)
   - **API Key**: Copied from plugin settings

### Note on HTTPS
The plugin uses a self-signed certificate by default. If you get SSL errors, enable HTTP mode in the plugin settings or configure your server to trust the certificate.

---

<a id="10-openai-gpt"></a>
## 10. OpenAI GPT

**Requires app publishing?** No — API key based.

### Setup
1. Go to [platform.openai.com](https://platform.openai.com).
2. Navigate to **API Keys** → **Create new secret key**.
3. Copy the key (starts with `sk-`).
4. In AI Station, paste the key and set your preferred default model.

### Recommended models
- `gpt-4o` — Best capability
- `gpt-4o-mini` — Fast and affordable
- `gpt-4-turbo` — High capability

### Backend env vars (global fallback)
```env
OPENAI_API_KEY=sk-your-key
OPENAI_MODEL=gpt-4o
```

---

<a id="11-spotify"></a>
## 11. Spotify

**Requires app publishing?** **Yes** — for other users to connect their Spotify. For your own account (development mode), no review needed.

### Setup
1. Go to [developer.spotify.com/dashboard](https://developer.spotify.com/dashboard).
2. Click **Create App**. Set a name and description.
3. Under **Settings**, add a Redirect URI:
   ```
   https://yourdomain.com/auth/spotify/callback
   http://localhost:3001/auth/spotify/callback   (for development)
   ```
4. Copy the **Client ID** and **Client Secret**.

### Backend env vars
```env
SPOTIFY_CLIENT_ID=your_client_id
SPOTIFY_CLIENT_SECRET=your_client_secret
```

### Frontend env vars
```env
NEXT_PUBLIC_SPOTIFY_CLIENT_ID=your_client_id
```

### Production publishing
To allow any Spotify user (not just yourself) to connect, you must apply for **Extended Quota Mode** in the Spotify Developer Dashboard, which requires a brief review.

---

<a id="12-philips-hue"></a>
## 12. Philips Hue

**Requires app publishing?** No — local network API.

### Setup

#### Find your Bridge IP
- Visit [discovery.meethue.com](https://discovery.meethue.com) — it returns your bridge IP.
- Or check your router's DHCP client list.

#### Create an API username
1. Open `http://<BRIDGE_IP>/debug/clip.html` in your browser.
2. Set URL to `/api` and body to `{"devicetype":"aistation#hub"}`.
3. **Press the physical button on the Hue Bridge** within 30 seconds.
4. Click **POST**. The response contains your `username` (API token).

Or use curl:
```bash
# Press bridge button first, then:
curl -X POST http://<BRIDGE_IP>/api -d '{"devicetype":"aistation#hub"}'
```

### In AI Station
- **Bridge IP**: e.g. `192.168.1.2`
- **Username**: The long token string generated above

---

<a id="13-x-corp-twitterx"></a>
## 13. X Corp (Twitter/X)

**Requires app publishing?** **Yes** — Twitter/X API v2 requires applying for developer access. Write access (posting tweets) requires **Basic** or higher tier.

### Setup
1. Go to [developer.twitter.com/en/portal/dashboard](https://developer.twitter.com/en/portal/dashboard).
2. Apply for a developer account if you don't have one.
3. Create a new **Project** and **App** within it.
4. Under **Keys & Tokens**:
   - Copy the **Bearer Token** (for reading tweets — free tier is sufficient).
   - For posting tweets, generate **API Key**, **API Secret**, **Access Token**, and **Access Token Secret** (requires Basic tier, $100/month).

### Free tier limitations
- Read-only operations (timeline, search) work on the free tier.
- Posting tweets requires the **Basic** tier ($100/month) or higher.

### In AI Station
- **Bearer Token** is required for all read operations.
- **API Key, API Secret, Access Token, Access Token Secret** are needed only for posting tweets.

---

<a id="14-browser-playwright"></a>
## 14. Browser (Playwright)

**Requires app publishing?** No — local backend tool.

### ⚠️ Requirements
[Playwright](https://playwright.dev) with Chromium must be installed on the backend server.

### Setup
```bash
# In the backend project directory:
npx playwright install chromium
```

### In AI Station
- **Headless mode**: Recommended for servers (no display required).
- **Proxy URL**: Optional — set to route browser traffic through a proxy.

### Capabilities
- Open any URL and extract page text (first 5000 characters)
- Take full-page screenshots (returned as base64 PNG)
- Wait for specific CSS selectors before extracting content

---

<a id="15-google-gmail"></a>
## 15. Google Gmail

**Requires app publishing?** **Yes** — Gmail API requires **Google app verification** for production use with non-test users.

### Setup
1. Go to [console.cloud.google.com](https://console.cloud.google.com).
2. Create a new project or select an existing one.
3. Enable the **Gmail API**: APIs & Services → Enable APIs → search "Gmail API" → Enable.
4. Go to **APIs & Services → Credentials → Create Credentials → OAuth 2.0 Client ID**.
5. Application type: **Web application**.
6. Add Authorized Redirect URIs:
   ```
   https://yourdomain.com/auth/gmail/callback
   http://localhost:3001/auth/gmail/callback   (for development)
   ```
7. Copy the **Client ID** and **Client Secret**.
8. Configure the **OAuth Consent Screen** (required):
   - Add your app name, logo, support email.
   - Add scopes: `gmail.readonly`, `gmail.send`.
   - Add test users (for development — up to 100 test users without verification).

### Backend env vars
```env
GOOGLE_CLIENT_ID=your_client_id.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=your_client_secret
```

### Frontend env vars
```env
NEXT_PUBLIC_GOOGLE_CLIENT_ID=your_client_id.apps.googleusercontent.com
```

### ⚠️ App Verification Required for Production
- While in **Testing** mode, only users you add as test users can connect.
- To allow any Google user to connect, you must submit for **Google Verification** (OAuth App Verification), which involves a security review.
- Verification is required if you use sensitive scopes like `gmail.send`.
- Process: APIs & Services → OAuth Consent Screen → **Publish App** → Submit for verification.

---

<a id="16-github"></a>
## 16. GitHub

**Requires app publishing?** No — Personal Access Token based, no review needed.

### Setup
1. Go to [github.com/settings/tokens](https://github.com/settings/tokens).
2. Click **Generate new token (classic)**.
3. Select scopes: `repo`, `notifications`, `read:user`.
4. Copy the token (starts with `ghp_`).
5. In AI Station, paste the token and optionally set a default owner/repo.

### Fine-grained tokens (recommended)
Use [fine-grained personal access tokens](https://github.com/settings/tokens?type=beta) for more precise permission control:
- Repository permissions: **Issues** (read/write), **Pull requests** (read), **Metadata** (read)
- Account permissions: **Notifications** (read)

---

<a id="17-email-smtp"></a>
## 17. Email (SMTP)

**Requires app publishing?** No — SMTP server connection only.

### Setup
Enter your SMTP server credentials directly:
- **SMTP Host**: e.g. `smtp.gmail.com`, `smtp.sendgrid.net`
- **SMTP User**: Your login username (usually your email address)
- **SMTP Password**: Your password or app-specific password
- **SMTP Port**: `587` (STARTTLS), `465` (SSL), or `25`

### Gmail SMTP
If using Gmail:
1. Enable 2FA on your Google account.
2. Go to [myaccount.google.com/apppasswords](https://myaccount.google.com/apppasswords).
3. Generate an **App Password** for "Mail".
4. Use `smtp.gmail.com`, port `587`, and the app password.

### Backend env vars (global defaults)
```env
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your@email.com
SMTP_PASS=your_app_password
```

---

<a id="18-google-calendar"></a>
## 18. Google Calendar

**Requires app publishing?** **Yes** — same as Gmail. Google Calendar API requires app verification for production use.

### Setup
Follow the same steps as [Google Gmail](#15-google-gmail) but enable the **Google Calendar API** instead (or in addition).

Add the Calendar API:
1. APIs & Services → Enable APIs → search "Google Calendar API" → Enable.
2. Add the scope `https://www.googleapis.com/auth/calendar.readonly` to your OAuth consent screen.
3. Add the Redirect URI for the Calendar callback:
   ```
   https://yourdomain.com/auth/google-calendar/callback
   http://localhost:3001/auth/google-calendar/callback
   ```

The same `NEXT_PUBLIC_GOOGLE_CLIENT_ID` and `GOOGLE_CLIENT_SECRET` are used for both Gmail and Calendar.

---

<a id="environment-variables-reference"></a>
## Environment Variables Reference

### Backend (`.env`)

```env
# App URL (required for Telegram auto-webhook registration)
APP_URL=https://yourdomain.com

# Google OAuth (Gmail + Calendar)
GOOGLE_CLIENT_ID=your_client_id.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=your_client_secret

# Spotify
SPOTIFY_CLIENT_ID=your_spotify_client_id
SPOTIFY_CLIENT_SECRET=your_spotify_client_secret

# Telegram (optional global fallback)
TELEGRAM_BOT_TOKEN=your_bot_token

# WhatsApp
WHATSAPP_VERIFY_TOKEN=your_verify_token
WHATSAPP_APP_SECRET=your_app_secret
WHATSAPP_QR_AUTH_DIR=/tmp/ai-station-whatsapp

# Slack (optional global fallback)
SLACK_BOT_TOKEN=xoxb-...
SLACK_SIGNING_SECRET=your_signing_secret

# Discord (optional global fallback)
DISCORD_BOT_TOKEN=your_bot_token

# Email/SMTP (optional global defaults)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your@email.com
SMTP_PASS=your_app_password

# OpenAI (optional global fallback)
OPENAI_API_KEY=sk-...
OPENAI_MODEL=gpt-4o
```

### Frontend (`.env.local`)

```env
NEXT_PUBLIC_API_BASE_URL=http://localhost:3000/api/v1

# Spotify OAuth
NEXT_PUBLIC_SPOTIFY_CLIENT_ID=your_spotify_client_id
SPOTIFY_CLIENT_SECRET=your_spotify_client_secret   # server-side only

# Google OAuth (Gmail + Calendar)
NEXT_PUBLIC_GOOGLE_CLIENT_ID=your_client_id.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=your_client_secret            # server-side only
```

---

<a id="app-publishing-requirements-summary"></a>
## App Publishing Requirements Summary

| Integration | Requires Publishing / Review? | Notes |
|---|---|---|
| **Telegram** | ❌ No | Bots work immediately, no review |
| **Discord** | ❌ No | Bots work in your server without review |
| **WhatsApp (Meta Cloud)** | ⚠️ For production | Test with 5 numbers without review; App Review for public |
| **WhatsApp (QR)** | ❌ No | Personal account, no review |
| **BTCY Chat** | ❌ No | Internal platform |
| **Slack** | ⚠️ For distribution | Internal use OK; App Review for Slack App Directory |
| **Signal** | ❌ No | Self-hosted signal-cli, no review |
| **iMessage** | ❌ No | Local macOS only |
| **Anthropic Claude API** | ❌ No | API key, no review |
| **Obsidian** | ❌ No | Local plugin, no review |
| **OpenAI GPT** | ❌ No | API key, no review |
| **Spotify** | ⚠️ For other users | 25 users in dev mode; Extended Quota Mode review for production |
| **Philips Hue** | ❌ No | Local bridge API |
| **X / Twitter** | ⚠️ Always | Developer account required; Basic tier ($100/mo) for write access |
| **Browser (Playwright)** | ❌ No | Local server tool |
| **Google Gmail** | ✅ Required for production | Testing mode: 100 test users max; must publish and verify for production |
| **GitHub** | ❌ No | Personal Access Token, no review |
| **Email (SMTP)** | ❌ No | Direct SMTP connection |
| **Google Calendar** | ✅ Required for production | Same as Gmail — requires Google app verification |

### What "requires publishing" means in practice

**Google (Gmail + Calendar):**
- During development: add up to 100 Google accounts as test users — they can connect immediately.
- For production (any Google account): submit for OAuth App Verification. This involves security assessment, privacy policy review, and may take 4–6 weeks. Sensitive scopes like `gmail.send` undergo stricter review.

**Spotify:**
- Development mode: 25 users can connect. Add them manually in the Spotify Dashboard.
- Production (any Spotify user): Apply for **Extended Quota Mode** in the dashboard — basic review, usually approved within a few days.

**WhatsApp Meta Cloud:**
- Development: test up to 5 phone numbers without App Review.
- Production: Submit for WhatsApp App Review in Meta's Developer Portal.

**Slack:**
- Your own workspace: install without review.
- Public Slack App Directory: full App Review required.

**X / Twitter:**
- A developer account is always required (free tier for reading, Basic $100/mo for writing).
- No separate "app review" but Basic/Pro tier purchase is mandatory for write access.
