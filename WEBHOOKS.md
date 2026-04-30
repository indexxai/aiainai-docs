# Webhook Registration Guide

**Base URL:** `https://api.aiainai.com/api/v1`

All global webhooks (WhatsApp, Twitter, Signal) use a single shared URL that routes to the correct station automatically using fields stored in the integration config.  
Station-scoped webhooks (Telegram, Discord, Slack, Google Calendar) use a URL containing the `stationId`.

---

## Global Webhooks

### WhatsApp (Meta Business Platform)
**Portal:** https://developers.facebook.com → Your App → WhatsApp → Configuration

| Method | URL |
|--------|-----|
| GET (verification) | `https://api.aiainai.com/api/v1/webhooks/whatsapp` |
| POST (messages) | `https://api.aiainai.com/api/v1/webhooks/whatsapp` |

- **Verify Token:** set to the value of `WHATSAPP_VERIFY_TOKEN` in your `.env`
- **Webhook Fields to subscribe:** `messages`
- Routes to station by matching `config.phoneNumberId`

---

### Twitter / X (Account Activity API)
**Portal:** https://developer.twitter.com → Your Project → App → Account Activity API

| Method | URL |
|--------|-----|
| GET (CRC challenge) | `https://api.aiainai.com/api/v1/webhooks/twitter` |
| POST (DMs & events) | `https://api.aiainai.com/api/v1/webhooks/twitter` |

Register the webhook using the Twitter API:
```
POST https://api.twitter.com/1.1/account_activity/all/:env_name/webhooks.json
  ?url=https%3A%2F%2Fapi.aiainai.com%2Fapi%2Fv1%2Fwebhooks%2Ftwitter
```
Then subscribe each user account to receive their DM events.

- **Signing secret:** `TWITTER_CONSUMER_SECRET` in `.env`
- Routes to station by matching `config.twitterUserId`

---

### Signal (signal-cli REST API)
**No portal — configure in signal-cli daemon**

| Method | URL |
|--------|-----|
| POST (inbound messages) | `https://api.aiainai.com/api/v1/webhooks/signal` |

In your signal-cli REST API config file, set:
```json
{ "webhook": "https://api.aiainai.com/api/v1/webhooks/signal" }
```
Routes to station by matching `config.phoneNumber` against the envelope destination.

---

## Station-Scoped Webhooks

Replace `{stationId}` with the MongoDB ObjectId of the station.

### Telegram
**No portal UI — use the Bot API directly**

| Method | URL |
|--------|-----|
| POST (inbound messages) | `https://api.aiainai.com/api/v1/stations/{stationId}/integrations/telegram/webhook` |

After connecting a bot token, set the webhook via:
```
POST https://api.telegram.org/bot{BOT_TOKEN}/setWebhook
  ?url=https://api.aiainai.com/api/v1/stations/{stationId}/integrations/telegram/webhook
```

---

### Discord
**Portal:** https://discord.com/developers/applications → Your App → General Information

| Method | URL |
|--------|-----|
| POST (interactions) | `https://api.aiainai.com/api/v1/stations/{stationId}/integrations/discord/webhook` |

Set this URL as the **Interactions Endpoint URL** in the Discord Developer Portal.

---

### Slack
**Portal:** https://api.slack.com/apps → Your App → Event Subscriptions

| Method | URL |
|--------|-----|
| POST (events) | `https://api.aiainai.com/api/v1/stations/{stationId}/integrations/slack/events` |

- Enable **Event Subscriptions** and set the Request URL above
- Subscribe to bot events: `message.im`, `message.channels`, `app_mention`
- **Signing secret:** `SLACK_SIGNING_SECRET` in `.env`

---

### Google Calendar
**Not a portal setting — call Google Calendar API directly after connecting**

| Method | URL |
|--------|-----|
| POST (push notifications) | `https://api.aiainai.com/api/v1/stations/{stationId}/integrations/google-calendar/webhook` |

Set up a watch channel via the Google Calendar API:
```
POST https://www.googleapis.com/calendar/v3/calendars/{calendarId}/events/watch
{
  "id": "unique-channel-id",
  "type": "web_hook",
  "address": "https://api.aiainai.com/api/v1/stations/{stationId}/integrations/google-calendar/webhook"
}
```

---

## Stripe (Billing)
**Portal:** https://dashboard.stripe.com → Developers → Webhooks

| Method | URL |
|--------|-----|
| POST (payment events) | `https://api.aiainai.com/api/v1/billing/webhooks/stripe` |

Events to subscribe:
- `checkout.session.completed`
- `invoice.payment_succeeded`
- `invoice.payment_failed`
- `customer.subscription.updated`
- `customer.subscription.deleted`

- **Signing secret:** `STRIPE_WEBHOOK_SECRET` in `.env`
