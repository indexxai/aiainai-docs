# Mining Station — End-to-End Walkthrough

This document covers the complete lifecycle of the Mining Station: referral setup, ad watching, real-time earnings crediting, withdrawal (all three methods), and admin processing.

---

## The Cast

| Person | Role |
|---|---|
| **Alice** | Mining Station owner. Referral code `ALICE2026`. |
| **Bob** | Alice's referred miner. Watches ads in the app. |
| **Carol** | Another referred miner of Alice. |

Current rates used throughout:

| Rate | Value | Source |
|---|---|---|
| Rewarded ad CPM | $0.9785 | AdMob Reporting API (7-day avg, Redis cache 24 h) |
| Interstitial CPM | $0.00 | AdMob Reporting API |
| Revenue share | 30% | `MINING_STATION_REFERRAL_REVENUE_SHARE_PCT` env var |
| BTCY price | ~$0.063 | Live price API (in-memory cache 5 min) |

---

## Stage 1 — Referral Setup

Bob downloads the app and signs up using Alice's referral link.

```
User collection — Bob's record
──────────────────────────────
email:            bob@example.com
referralCodeUsed: ALICE2026
kycStatus:        approved
```

The station API resolves Alice's miners by querying:

```js
User.find({ referralCodeUsed: "ALICE2026" })
```

Alice's station now shows `totalMiners: 1`.

---

## Stage 2 — Bob Watches a Rewarded Ad

Bob opens the app and watches a rewarded ad. The app calls the backend which:

**Step 1 — Writes AdMiningWatch record:**

```json
{
  "email": "bob@example.com",
  "adType": "rewarded",
  "timestamp": "2026-06-05T08:15:00Z",
  "placement": "mining_block"
}
```

**Step 2 — Fires `adRevenueCreditingService.creditAdWatch()` (non-blocking):**

```
1. Find Bob's referralCodeUsed   → "ALICE2026"
2. Find owner of "ALICE2026"     → alice@example.com
3. Get CPM from AdMob API        → $0.9785  (Redis cache 24h)
4. Get BTCY price                → $0.063   (in-memory cache 5min)
5. Calculate per-ad earnings:
     grossUsdPerAd = $0.9785 / 1000          = $0.0009785
     earningsUsd   = $0.0009785 × 30%        = $0.00029355
     earningsBtcy  = $0.00029355 / $0.063    = 0.004659 BTCY

6. Write StationEarningsHistory record:
   {
     ownerEmail:      "alice@example.com",
     minerEmail:      "bob@example.com",
     adType:          "rewarded",
     cpmUsd:          0.9785,
     revenueSharePct: 30,
     grossUsdPerAd:   0.0009785,
     earningsUsd:     0.00029355,
     earningsBtcy:    0.004659,
     usdPerBtcy:      0.063,
     createdAt:       2026-06-05T08:15:00Z
   }

7. userMiningBalance (Alice):
     adRevenueTransferableBalance += 0.004659 BTCY  ← credited immediately
```

This happens for every rewarded or interstitial ad Bob watches. Skipped ads (`rewarded_ad_skipped`) generate no credit.

### Ad Type Revenue Rules

| adType | Counted in adCount display | Generates revenue |
|---|---|---|
| `rewarded` | ✅ | ✅ |
| `interstitial` | ✅ | ✅ |
| `rewarded_ad_skipped` | ✅ | ❌ |
| `rewarded_ad_failed` | ✅ | ❌ |
| `banner` | ✅ (if recorded) | ❌ |

---

## Stage 3 — After Many Ad Watches

By end of the day, Bob and Carol have both watched ads:

| Miner | Rewarded | Skipped | Revenue-Generating |
|---|---|---|---|
| Bob | 4 | 1 | 3 |
| Carol | 2 | 0 | 2 |
| **Total** | **6** | **1** | **5** |

Alice's `adRevenueTransferableBalance`:

```
5 ads × 0.004659 BTCY = 0.02330 BTCY
```

Alice's dashboard (`GET /api/v1/mining/station/earnings`):

```json
{
  "lifetimeEarned":         { "usd": 0.00147, "btcy": 0.02330 },
  "currentMonthEarnings":   { "usd": 0.00147, "btcy": 0.02330 },
  "pendingUnverified":      { "usd": 0.00147, "btcy": 0.02330 },
  "availableWithdrawalBalance": { "btcy": 0.02330, "usd": 0.00147 },
  "allocationRate": 30,
  "growthVsLastMonth": { "percent": 100, "delta": { "usd": 0.00147, "btcy": 0.02330 } },
  "earningsHistory": [
    { "bucket": "2026-01", "adsWatched": 0, "earningsUsd": 0, "earningsBtcy": 0 },
    { "bucket": "2026-02", "adsWatched": 0, "earningsUsd": 0, "earningsBtcy": 0 },
    { "bucket": "2026-03", "adsWatched": 0, "earningsUsd": 0, "earningsBtcy": 0 },
    { "bucket": "2026-04", "adsWatched": 0, "earningsUsd": 0, "earningsBtcy": 0 },
    { "bucket": "2026-05", "adsWatched": 0, "earningsUsd": 0, "earningsBtcy": 0 },
    { "bucket": "2026-06", "adsWatched": 5, "earningsUsd": 0.00147, "earningsBtcy": 0.02330 }
  ]
}
```

**Data source:** `earningsHistory`, `lifetimeEarned`, `currentMonthEarnings` all read from `StationEarningsHistory` collection — rates are locked at the time each ad was watched. Historical values do not change when CPM or BTCY price changes.

---

## Stage 4 — Alice Withdraws (Three Methods)

Minimum withdrawal: **$50 USD**. Maximum: **$1,000 USD**. Amount is always in USD.

### Method 1 — USDT (Solana)

```bash
POST /api/v1/mining/station/withdrawals
{
  "amount": 50,
  "method": "usdt",
  "walletAddress": "ALICE_SOLANA_ADDRESS"
}
```

```
Fee: 10%
feeAmountUsd       = $50 × 10%  = $5
approvedAmountUsd  = $50 - $5   = $45
amountBtcy         = $50 / $0.063 = 793.65 BTCY  ← deducted from adRevenueTransferableBalance
requestedAmountBtcy = 793.65 BTCY  ← stored for restore on reject

WithdrawRequest created:
  requestedAmount:     $50
  approvedAmount:      $45
  requestedAmountBtcy: 793.65
  payoutAmount:        $45
  payoutCurrency:      USDT
  withdrawalMethod:    USDT
  walletAddress:       ALICE_SOLANA_ADDRESS
  network:             Solana Network
  source:              ad_revenue
  status:              Pending

DB changes (immediate):
  adRevenueTransferableBalance -= 793.65 BTCY
```

Admin sends $45 USDT to Alice's Solana wallet externally, then approves:

```bash
PUT /api/v1/mining/station/admin/withdrawals/:id/approve
{ "txHash": "solana_tx_hash" }
```

```
DB changes on approval:
  migratedBalance      += 793.65 BTCY
  WithdrawRequest.status = "Approved"
  WithdrawRequest.txHash = "solana_tx_hash"
```

---

### Method 2 — USDC (Solana)

Identical to USDT. Fee: **10%**. Admin sends USDC externally then approves.

```bash
POST /api/v1/mining/station/withdrawals
{
  "amount": 50,
  "method": "usdc",
  "walletAddress": "ALICE_SOLANA_ADDRESS"
}
```

---

### Method 3 — BTCY (Internal — Ying Yang Chain)

No external wallet address needed. BTCY is credited immediately on submission.

```bash
POST /api/v1/mining/station/withdrawals
{
  "amount": 50,
  "method": "btcy"
}
```

```
Fee: 3%
feeAmountUsd       = $50 × 3%  = $1.50
approvedAmountUsd  = $50 - $1.50 = $48.50
amountBtcy         = $50 / $0.063    = 793.65 BTCY  ← deducted from adRevenueTransferableBalance
payoutBtcy         = $48.50 / $0.063 = 769.84 BTCY  ← credited to wallet immediately

DB changes (immediate):
  adRevenueTransferableBalance -= 793.65 BTCY
  userWallets[coinSymbol=BTCY, coinNetwork="Ying Yang Chain"].coinBalance += 769.84
  WithdrawRequest { status: "Pending", payoutCurrency: "BTCY" }
```

Admin approves (BTCY already in wallet — just marks approved):

```bash
PUT /api/v1/mining/station/admin/withdrawals/:id/approve
{ "txHash": "" }
```

```
DB changes on approval:
  migratedBalance      += 793.65 BTCY
  WithdrawRequest.status = "Approved"
```

---

### Rejection (Any Method)

```bash
PUT /api/v1/mining/station/admin/withdrawals/:id/reject
{ "reason": "Invalid wallet address" }
```

```
ad_revenue source:
  adRevenueTransferableBalance += 793.65 BTCY  ← fully restored
  WithdrawRequest.status = "Rejected"
```

---

## Stage 5 — Admin Withdrawal Management

### List All Pending

```bash
GET /api/v1/mining/station/admin/withdrawals?status=Pending&page=0&pageSize=20
```

Filters available: `email`, `status` (Pending/Approved/Rejected), `method`, `from`, `to`

Response includes `source` field (`ad_revenue` or `mining_balance`) to distinguish station withdrawals from mining withdrawals.

---

## Balance Fields Explained

```
userMiningBalance collection (Alice):
─────────────────────────────────────
transferableBalance:          72 BTCY   ← from Alice's own mining sessions only
adRevenueTransferableBalance: 0.02330   ← from ad revenue credits only (station earnings)
migratedBalance:              0         ← total lifetime withdrawn (all sources)
unverifiedBalance:            0
```

**These two balance fields never mix:**

| Field | Credited by | Debited by |
|---|---|---|
| `transferableBalance` | Mining stop/claim process | Mining withdrawal |
| `adRevenueTransferableBalance` | `adRevenueCreditingService` (per ad watch) | Station ad revenue withdrawal |

---

## Full Flow Diagram

```
Alice shares referral code ALICE2026
         │
         ▼
Bob & Carol sign up with referralCodeUsed = ALICE2026
         │
         ▼
Bob watches a rewarded ad in the app
  AdMiningWatchService.record() writes AdMiningWatch document
         │
         ├─ Response returned to Bob immediately (non-blocking)
         │
         └─ adRevenueCreditingService.creditAdWatch() [async]
               │
               ├─ Find referrer → alice@example.com
               ├─ Get CPM from AdMob API (Redis 24h cache)
               ├─ Get BTCY price (in-memory 5min cache)
               ├─ Calculate earningsBtcy per ad
               ├─ Write StationEarningsHistory record
               └─ adRevenueTransferableBalance += earningsBtcy
         │
         ▼
Alice opens earnings dashboard
  StationEarningsHistory queried for:
    lifetimeEarned, currentMonthEarnings, earningsHistory (6 months)
  userMiningBalance.adRevenueTransferableBalance → availableWithdrawalBalance
         │
         ▼
Alice submits withdrawal (USDT / USDC / BTCY)
  Validate: adRevenueTransferableBalance × usdPerBtcy >= requestedUsd
  Calculate: amountBtcy = requestedUsd / usdPerBtcy
  Fee: 10% (USDT/USDC) or 3% (BTCY)
  adRevenueTransferableBalance -= amountBtcy (immediate)
  If BTCY: userWallets[Ying Yang Chain].coinBalance += payoutBtcy (immediate)
  WithdrawRequest created { status: "Pending", source: "ad_revenue" }
         │
         ▼
Admin reviews pending withdrawals
  GET /admin/withdrawals?status=Pending
         │
    ┌────┴────┐
    ▼         ▼
 Approve    Reject
    │         │
    │         └─ adRevenueTransferableBalance += amountBtcy (restored)
    │            WithdrawRequest.status = "Rejected"
    │
    └─ USDT/USDC: admin sends tokens externally, then approves
       BTCY: already in wallet, just marks approved
       migratedBalance += amountBtcy
       WithdrawRequest.status = "Approved"
```

---

## Key Numbers Reference

| Variable | Value | Source |
|---|---|---|
| Rewarded CPM | ~$0.9785 | AdMob Reporting API, 7-day avg, refreshed daily |
| Interstitial CPM | $0 | AdMob Reporting API |
| Revenue share | 30% | `MINING_STATION_REFERRAL_REVENUE_SHARE_PCT` |
| BTCY price | ~$0.063 | Live price API, refreshed every 5 min |
| Min withdrawal | $50 USD | Hard-coded |
| Max withdrawal | $1,000 USD | Hard-coded |
| USDT fee | 10% | Hard-coded |
| USDC fee | 10% | Hard-coded |
| BTCY fee | 3% | Hard-coded |
| BTCY wallet network | Ying Yang Chain | Hard-coded |
| CPM cache TTL | 24 hours | Redis |
| BTCY price cache TTL | 5 minutes | In-memory |
| Station response cache TTL | 5 minutes | Redis |
| Earnings history window | Last 6 months | Always all 6 buckets shown (0-filled) |

---

## Collections Involved

| Collection | Purpose |
|---|---|
| `AdMiningWatch` | Raw ad watch events per miner |
| `StationEarningsHistory` | Per-ad credit records for station owners (locked rates) |
| `userMiningBalance` | Running balances: `transferableBalance` (mining) + `adRevenueTransferableBalance` (ad revenue) |
| `WithdrawRequest` | Withdrawal requests with `source: "ad_revenue"` or `"mining_balance"` |
| `User` | User profiles, referral codes, `userWallets` array |
| `Mining` | Miner's active mining plan and `totalMined` |

---

## API Endpoints

| Method | Path | Access | Purpose |
|---|---|---|---|
| GET | `/api/v1/mining/station/overview` | Auth | Station dashboard overview |
| GET | `/api/v1/mining/station/earnings` | Auth | Earnings page with 6-month history |
| GET | `/api/v1/mining/station/referrals` | Auth | Paginated referral list |
| GET | `/api/v1/mining/station/miners` | Auth | Paginated miner list |
| GET | `/api/v1/mining/station/analytics` | Auth | Charts and engagement metrics |
| GET | `/api/v1/mining/station/withdrawals` | Auth | Owner's withdrawal page |
| POST | `/api/v1/mining/station/withdrawals` | Auth | Submit withdrawal (USDT/USDC/BTCY) |
| GET | `/api/v1/mining/station/admin/withdrawals` | Admin | List all withdrawals with filters |
| PUT | `/api/v1/mining/station/admin/withdrawals/:id/approve` | Admin | Approve and mark txHash |
| PUT | `/api/v1/mining/station/admin/withdrawals/:id/reject` | Admin | Reject and restore balance |
