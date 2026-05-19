# BTCY Sell Eligible Users Snapshot

Generated at: `2026-05-19T04:29:19.720Z`

Database: `prod-indexx-exchange`

This report lists users who currently have remaining BTCY sell allowance under the real-buyer sell rule.

## Eligibility Rule

A user can sell BTCY only when they have a completed BTCY buy paid through one of the approved real payment paths:

- USDT
- USDC
- PayPal
- Stripe

PayPal is checked in both places:

- completed BTCY buy orders with PayPal/Stripe-style payment type
- completed PayPal collection records linked to a completed BTCY buy order by `orderId`

The sellable amount is capped to purchased BTCY:

```text
remaining sellable BTCY = eligible BTCY buys - later BTCY sells/converts that consume those buys
```

The allowance is calculated chronologically. BTCY converts that happened before an eligible buy do not consume that later buy's sell allowance.

Users who only received BTCY through mining, airdrop, Alchemy, manual wallet credit, or convert history are not eligible unless they also have an eligible completed BTCY buy.

## Eligible Users

| Email | Purchased BTCY | Used BTCY | Can Sell BTCY | Reason |
| --- | ---: | ---: | ---: | --- |
| jerryhngo@gmail.com | 13314.81506260431 | 0 | 13314.815062604312 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| ms9820639@gmail.com | 1112.29969459 | 0 | 1112.29969459 | Completed BTCY buy paid by USDT |
| sergejkirsch79@gmail.com | 999.802776185477 | 0 | 999.802776185477 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| mahuclo74@gmail.com | 570.103695155046 | 0 | 570.103695155046 | Completed BTCY buy paid by USDT |
| mr.zakimed@gmail.com | 478.4486031 | 0 | 478.4486031 | Completed BTCY buy paid by USDT |
| abbasafvasibi@gmail.com | 473.568934 | 0 | 473.568934 | Completed BTCY buy paid by USDT |
| traylil503@gmail.com | 405.250465 | 0 | 405.250465 | Completed BTCY buy paid by USDT |
| usmanwunti2020@gmail.com | 334.3578394 | 0 | 334.3578394 | Completed BTCY buy paid by USDT |
| alexanderchirino@gmail.com | 314.318496208114 | 0 | 314.318496208114 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| ammanullah60@gmail.com | 300.49 | 30 | 270.49 | Completed BTCY buy paid by USDT |
| lili@azooca.com | 255.01 | 0 | 255.01 | Completed BTCY buy paid by USDC; Completed BTCY buy paid by USDT |
| brsaraivar@gmail.com | 215.833180741617 | 0 | 215.833180741617 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| hatchtage@yahoo.com | 794.551040441456 | 601.99 | 192.561040441456 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order; Completed BTCY buy paid by USDC |
| ekopastikaya95@gmail.com | 174.475074961 | 0 | 174.475074961 | Completed BTCY buy paid by USDT |
| ismiauliamaghfirah@gmail.com | 167.5473644 | 0 | 167.5473644 | Completed BTCY buy paid by USDT |
| yuyunm665@gmail.com | 167.1402304 | 0 | 167.1402304 | Completed BTCY buy paid by USDT |
| nugraha.indra0221@gmail.com | 165.350224 | 0 | 165.350224 | Completed BTCY buy paid by USDT |
| songthanh9999@gmail.com | 162.7481908 | 0 | 162.7481908 | Completed BTCY buy paid by USDT |
| muhammadmarjuki1203@gmail.com | 147.614549 | 0 | 147.614549 | Completed BTCY buy paid by USDT |
| medygalagala388@gmail.com | 132.568595189156 | 0 | 132.568595189156 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| cholidaja45@gmail.com | 123.502224643603 | 0 | 123.502224643603 | Completed BTCY buy paid by USDT |
| ppatel5169@gmail.com | 114.190381629965 | 0 | 114.190381629965 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| gerald_geraldgplj@yahoo.com | 111.214565371254 | 0 | 111.214565371254 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| drone1215@gmail.com | 110.987803661243 | 0 | 110.987803661243 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| taraboi103@gmail.com | 110.461962975359 | 0 | 110.461962975359 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| rakeyteah0@gmail.com | 107.258588463325 | 0 | 107.258588463325 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| meliawaty65@gmail.com | 1740.26885338 | 1640 | 100.26885338 | Completed BTCY buy paid by USDT |
| dilqamshabanov@gmail.com | 93.467037307088 | 0 | 93.467037307088 | completed PayPal payment linked to BTCY buy order |
| lashashervashidze@gmail.com | 92.706587359271 | 0 | 92.706587359271 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| sunkuomkarsai12121@gmail.com | 91.493752623583 | 0 | 91.493752623583 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| gafterjun@gmail.com | 35.962795558619 | 0 | 35.962795558619 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| h4a5i@punkproof.com | 13.957474367098 | 0 | 13.957474367098 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| sunkuomkarsai@gmail.com | 39.853834758357 | 26.97326213962 | 12.880572618736 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| joritakahashi@gmail.com | 8.200080655993 | 0 | 8.200080655993 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| agustinussinaga0@gmail.com | 0.702336894328 | 0 | 0.702336894328 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| alexanderkruus@gmail.com | 127.13 | 127 | 0.13 | Completed BTCY buy paid by USDC |
| iyastopik63@gmail.com | 166.1 | 166 | 0.1 | Completed BTCY buy paid by USDT |
| singjayabandung@gmail.com | 1241.6 | 1241.57 | 0.03 | Completed BTCY buy paid by USDT |
| billyoktria31@gmail.com | 468.37671171 | 468.3767117 | 1e-8 | Completed BTCY buy paid by USDT |

## Notes

- The `Used BTCY` column is BTCY already consumed from eligible buy allowance. It comes from BTCY sell orders that are not cancelled/expired and completed BTCY convert orders where BTCY was the input, but only when those orders occurred after eligible buys with available allowance.
- This is a point-in-time snapshot. New completed BTCY buys increase the allowance.
- New BTCY sell orders or completed BTCY converts reduce allowance only after there is eligible purchased BTCY available to consume.
- Very small remaining values are kept exactly as returned by the database calculation.

## Used BTCY Sources

Only users with non-zero `Used BTCY` are listed here.

| Email | Used BTCY | Where It Was Used | Order ID | Status | Date |
| --- | ---: | --- | --- | --- | --- |
| alexanderkruus@gmail.com | 127 | Completed BTCY convert from BTCY to USDT | 30519820 | Completed | 2026-03-30T03:34:44.366Z |
| ammanullah60@gmail.com | 10 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778252493557 | Pending | 2026-05-08T15:01:33.557Z |
| ammanullah60@gmail.com | 10 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778606823149 | Pending | 2026-05-12T17:27:03.149Z |
| ammanullah60@gmail.com | 10 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778691677762 | Pending | 2026-05-13T17:01:17.763Z |
| arslandev180@gmail.com | 1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | 1777906289742 | Completed | 2026-05-04T14:58:15.287Z |
| arslandev180@gmail.com | 1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1777908908766 | Completed | 2026-05-04T15:35:34.336Z |
| arslandev180@gmail.com | 1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1777911285084 | Completed | 2026-05-04T16:17:21.256Z |
| arslandev180@gmail.com | 1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1777917818196 | Completed | 2026-05-05T12:12:23.845Z |
| arslandev180@gmail.com | 0.2 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1777983320382 | Completed | 2026-05-05T12:16:04.263Z |
| arslandev180@gmail.com | 1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1777983514482 | Completed | 2026-05-05T12:18:55.392Z |
| arslandev180@gmail.com | 0.1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1777990497122 | Pending | 2026-05-05T14:14:57.123Z |
| arslandev180@gmail.com | 0.1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1777992411645 | Pending | 2026-05-05T14:46:51.645Z |
| arslandev180@gmail.com | 0.1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1777992710725 | Pending | 2026-05-05T14:51:50.725Z |
| arslandev180@gmail.com | 0.1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1777993172222 | Pending | 2026-05-05T14:59:32.222Z |
| arslandev180@gmail.com | 1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778001243149 | Pending | 2026-05-05T17:14:03.149Z |
| arslandev180@gmail.com | 1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778001269502 | Pending | 2026-05-05T17:14:29.502Z |
| arslandev180@gmail.com | 1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778001527658 | Pending | 2026-05-05T17:18:47.658Z |
| arslandev180@gmail.com | 1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778002546043 | Pending | 2026-05-05T17:35:46.043Z |
| arslandev180@gmail.com | 1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778002679755 | Pending | 2026-05-05T17:37:59.755Z |
| arslandev180@gmail.com | 1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778002737818 | Pending | 2026-05-05T17:38:57.818Z |
| arslandev180@gmail.com | 10 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778003358665 | Pending | 2026-05-05T17:49:18.665Z |
| arslandev180@gmail.com | 10 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778003539377 | Pending | 2026-05-05T17:52:19.377Z |
| arslandev180@gmail.com | 10.82 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778004130108 | Pending | 2026-05-05T18:02:10.108Z |
| billyoktria31@gmail.com | 130.47 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778884014836 | Pending | 2026-05-15T22:26:54.836Z |
| billyoktria31@gmail.com | 337.9067117 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1779019344934 | Pending | 2026-05-17T12:02:24.935Z |
| bitcoinlightening2929@gmail.com | 87.333979197221 | Completed BTCY convert from BTCY to USDT | 40942357 | Completed | 2026-02-20T02:25:28.213Z |
| chunk.socket_1g@icloud.com | 1548.5525884 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778871103576 | Pending | 2026-05-15T18:51:43.576Z |
| hatchtage@yahoo.com | 601.99 | Completed BTCY convert from BTCY to USDT | 72181825 | Completed | 2026-02-10T05:21:24.326Z |
| iyastopik63@gmail.com | 166 | Completed BTCY convert from BTCY to USDT | 26533346 | Completed | 2026-04-26T09:06:39.286Z |
| jatipersadamandiri77@gmail.com | 166.9 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778871040953 | Pending | 2026-05-15T18:50:40.953Z |
| meliawaty65@gmail.com | 1000 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1779027575524 | Pending | 2026-05-17T14:19:35.525Z |
| meliawaty65@gmail.com | 300 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1779060732041 | Pending | 2026-05-17T23:32:12.041Z |
| meliawaty65@gmail.com | 200 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1779060871540 | Pending | 2026-05-17T23:34:31.540Z |
| meliawaty65@gmail.com | 140 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1779061147088 | Pending | 2026-05-17T23:39:07.088Z |
| sangmey14@gmail.com | 130.49 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778883221480 | Pending | 2026-05-15T22:13:41.480Z |
| sangmey14@gmail.com | 977.49516778 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1779019199572 | Pending | 2026-05-17T11:59:59.572Z |
| singjayabandung@gmail.com | 130.47 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778883675407 | Pending | 2026-05-15T22:21:15.407Z |
| singjayabandung@gmail.com | 1111.1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1779019199788 | Pending | 2026-05-17T11:59:59.789Z |
| sugihbanda117@gmail.com | 166.9 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778882746647 | Pending | 2026-05-15T22:05:46.647Z |
| sultanthankyou92@gmail.com | 249.379668075662 | Completed BTCY convert from BTCY to USDT | 74294714 | Completed | 2026-05-08T15:15:03.013Z |
| sunkuomkarsai@gmail.com | 26.97326213962 | Completed BTCY convert from BTCY to IUSD+ | 80083460 | Completed | 2025-12-16T14:17:07.680Z |
