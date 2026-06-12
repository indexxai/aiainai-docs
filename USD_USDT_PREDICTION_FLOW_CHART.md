# USD, USDT & Indexx Token — Deposit, Withdrawal & Prediction Flow

> Documents the intended funds flow after separating USDT from USD cash.

---

## Currency Support at a Glance

| Currency | Deposit Method | Withdrawal Method | Single Prediction | Lottery |
|----------|---------------|-------------------|:-----------------:|:-------:|
| **USD**  | Stripe card · Bank transfer | Bank transfer | ✅ | ❌ |
| **USDT** | On-chain (ETH / SOL) | On-chain (ETH / SOL) | ❌ | ✅ |
| **BTCY** | Indexx wallet | — | ✅ | ✅ |
| **INEX** | Indexx wallet | — | ✅ | ✅ |
| **WIBS** | Indexx wallet | — | ✅ | ✅ |

---

## 1 · Deposit Flow

How money enters the EMMM wallet.

```mermaid
flowchart LR
    classDef user   fill:#4A90D9,stroke:#2171B5,color:#fff
    classDef proc   fill:#F0F4F8,stroke:#B0BEC5,color:#333
    classDef admin  fill:#F39C12,stroke:#D68910,color:#fff
    classDef auto   fill:#8E44AD,stroke:#6C3483,color:#fff
    classDef bal    fill:#27AE60,stroke:#1E8449,color:#fff

    U(["👤 User"]):::user

    subgraph USD_DEP ["── USD Deposits ──"]
        direction TB
        Stripe["Stripe card\npayment"]:::proc
        BankReq["Bank transfer\nrequest"]:::proc
        AdminUSD(["🔐 Admin\napproves"]):::admin
    end

    subgraph USDT_DEP ["── USDT Deposit (Automatic) ──"]
        direction TB
        CryptoReq["1. User sends USDT\non-chain (ETH / SOL)"]:::proc
        TxSubmit["2. User submits txHash\nPOST /deposits/:id/verify-tx"]:::proc
        AutoVerify(["3. System queries\nEtherscan / Solana RPC\nauto-verifies & settles"]):::auto
        AdminFallback(["🔐 Admin fallback\nif tx stays unconfirmed"]):::admin
    end

    subgraph TOKEN_DEP ["── Indexx Token Deposit ──"]
        Indexx(["Indexx\nAsset Wallet"]):::proc
    end

    USD["💵 USD\ncashBalance"]:::bal
    USDT["💲 USDT\nusdtBalance"]:::bal
    Tokens["🪙 BTCY · INEX · WIBS\ntoken balances"]:::bal

    U -->|"card payment"| Stripe
    Stripe -->|"webhook auto-settles"| USD

    U -->|"initiate transfer"| BankReq
    BankReq -->|"pending review"| AdminUSD
    AdminUSD -->|"approved → credit"| USD

    U -->|"send on-chain"| CryptoReq
    CryptoReq --> TxSubmit
    TxSubmit --> AutoVerify
    AutoVerify -->|"confirmed ≥ 3 blocks\nauto-credit"| USDT
    AutoVerify -->|"pending / unconfirmed\nstored for review"| AdminFallback
    AdminFallback -->|"manually approved → credit"| USDT

    U -->|"use BTCY/INEX/WIBS"| Indexx
    Indexx -->|"debit from Indexx wallet"| Tokens
```

---

## 2 · Withdrawal Flow

How money leaves the EMMM wallet.

```mermaid
flowchart LR
    classDef user   fill:#4A90D9,stroke:#2171B5,color:#fff
    classDef bal    fill:#27AE60,stroke:#1E8449,color:#fff
    classDef proc   fill:#F0F4F8,stroke:#B0BEC5,color:#333
    classDef admin  fill:#F39C12,stroke:#D68910,color:#fff
    classDef dest   fill:#E74C3C,stroke:#C0392B,color:#fff

    USD["💵 USD\ncashBalance"]:::bal
    USDT["💲 USDT\nusdtBalance"]:::bal
    U(["👤 User"]):::user

    subgraph BANK_W ["── USD Bank Withdrawal ──"]
        direction TB
        BankReq["Withdrawal recorded\nin DB as pending"]:::proc
        AdminB(["🔐 Admin\napproves & processes"]):::admin
    end

    subgraph USDT_W ["── USDT On-Chain Withdrawal ──"]
        direction TB
        USDTReq["Withdrawal recorded\nin DB as pending"]:::proc
        AdminU(["🔐 Admin\napproves & sends on-chain"]):::admin
    end

    BankDest["🏦 User's\nBank Account"]:::dest
    ChainDest["🔑 User's Crypto\nWallet (ETH / SOL)"]:::dest

    USD -->|"balance held"| BankReq
    U   -->|"request"| BankReq
    BankReq --> AdminB
    AdminB -->|"debit cashBalance\nthen transfer"| BankDest

    USDT -->|"balance held"| USDTReq
    U    -->|"request"| USDTReq
    USDTReq --> AdminU
    AdminU -->|"debit usdtBalance\nthen send on-chain"| ChainDest
```

---

## 3 · Prediction Market Flow

How balances are used in prediction markets and how winnings are returned.

```mermaid
flowchart TD
    classDef bal    fill:#27AE60,stroke:#1E8449,color:#fff
    classDef indexx fill:#4A90D9,stroke:#2171B5,color:#fff
    classDef market fill:#8E44AD,stroke:#6C3483,color:#fff

    subgraph BALANCES ["── EMMM Wallet Balances ──"]
        direction LR
        USD["💵 USD\ncashBalance"]:::bal
        USDT["💲 USDT\nusdtBalance"]:::bal
        BTCY["🪙 BTCY"]:::bal
        INEX["🪙 INEX"]:::bal
        WIBS["🪙 WIBS"]:::bal
    end

    Indexx(["Indexx\nAsset Wallet"]):::indexx

    BTCY <-->|"debit on entry\ncredit on win"| Indexx
    INEX <-->|"debit on entry\ncredit on win"| Indexx
    WIBS <-->|"debit on entry\ncredit on win"| Indexx

    subgraph SINGLE ["── Single Prediction ──"]
        SP["Supports:\nUSD · BTCY · INEX · WIBS"]:::market
    end

    subgraph LOTTERY ["── Prediction Lottery ──"]
        LT["Supports:\nUSDT · BTCY · INEX · WIBS"]:::market
    end

    USD    -->|"entry cost"| SP
    Indexx -->|"token entry cost"| SP
    SP -->|"USD profit / loss"| USD
    SP -->|"token profit / loss"| Indexx

    USDT   -->|"entry cost"| LT
    Indexx -->|"token entry cost"| LT
    LT -->|"USDT prize / refund"| USDT
    LT -->|"token prize / refund"| Indexx
```

---

## Rules Enforced

- **USD deposits** from Stripe (auto via webhook) and bank transfer (admin approval) credit `cashBalance`.
- **USDT deposits** on Ethereum or Solana use automatic on-chain verification:
  1. User sends USDT on-chain and receives a deposit address.
  2. User submits the transaction hash via `POST /api/deposits/:depositId/verify-tx`.
  3. The system queries Etherscan (Ethereum) or Solana RPC to verify the tx.
  4. If ≥ 3 confirmations (ETH) / ≥ 20 confirmations (SOL) and amount matches → `usdtBalance` is credited automatically.
  5. If the tx is still unconfirmed, the deposit is marked `awaiting_confirmation` and admin can approve manually as a fallback.
- **USDT withdrawals** are recorded as pending in `withdrawals`; admin manually sends on-chain and then debits `usdtBalance`.
- **Bank withdrawals** are recorded as pending in `withdrawals`; admin processes the bank transfer and then debits `cashBalance`.
- **Single Prediction** supports `USD`, `BTCY`, `INEX`, and `WIBS`. USD uses `cashBalance`; tokens use the Indexx debit/credit flow.
- **Prediction Lottery** supports `USDT`, `BTCY`, `INEX`, and `WIBS`. USDT entry and prizes use `usdtBalance` only — never `cashBalance`.
- Indexx token balances (`BTCY`, `INEX`, `WIBS`) are always debited/credited through the Indexx Asset Wallet, not directly.

## API Reference — USDT Deposit (Automatic On-Chain)

| Step | Method | Endpoint | Body |
|------|--------|----------|------|
| 1. Get deposit address | `GET` | `/api/deposits/crypto-address?token=usdt&chain=ethereum` | — |
| 2. Create deposit record | `POST` | `/api/deposits` | `{ method: "crypto", token: "usdt", chain: "ethereum", amount: 100 }` |
| 3. Send USDT on-chain | — | User action in wallet | Send to the address returned in step 1 |
| 4. Submit txHash | `POST` | `/api/deposits/:depositId/verify-tx` | `{ txHash: "0x..." }` |

**Verify-tx response examples:**

```json
// Auto-settled (confirmed)
{ "deposit": { "status": "settled", ... }, "verification": { "status": "confirmed", "confirmations": 5, "txAmount": 100, "blockchainUrl": "https://etherscan.io/tx/0x..." } }

// Still pending
{ "deposit": { "status": "awaiting_confirmation", ... }, "verification": { "status": "pending", "confirmations": 1, "message": "Transaction has 1 of 3 required confirmations. Try again shortly." } }

// Wrong address or amount
{ "deposit": { ... }, "verification": { "status": "wrong_recipient" | "amount_mismatch", "message": "..." } }
```
