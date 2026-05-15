# BTCY Sell Eligible Users Snapshot

Generated at: `2026-05-15T12:14:27.964Z`

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
remaining sellable BTCY = completed eligible BTCY buys - previous BTCY sells - completed BTCY converts
```

Users who only received BTCY through mining, airdrop, Alchemy, manual wallet credit, or convert history are not eligible unless they also have an eligible completed BTCY buy.

## Eligible Users

| Email | Purchased BTCY | Used BTCY | Can Sell BTCY | Reason |
| --- | ---: | ---: | ---: | --- |
| jerryhngo@gmail.com | 13314.81506260431 | 0 | 13314.81506260431 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| meliawaty65@gmail.com | 1740.26885338 | 0 | 1740.26885338 | Completed BTCY buy paid by USDT |
| chunk.socket_1g@icloud.com | 1548.5525884 | 0 | 1548.5525884 | completed PayPal payment linked to BTCY buy order |
| singjayabandung@gmail.com | 1241.6 | 0 | 1241.6 | Completed BTCY buy paid by USDT |
| ms9820639@gmail.com | 1112.29969459 | 0 | 1112.29969459 | Completed BTCY buy paid by USDT |
| sangmey14@gmail.com | 1107.98516778 | 0 | 1107.98516778 | Completed BTCY buy paid by USDT |
| sergejkirsch79@gmail.com | 999.802776185477 | 0 | 999.802776185477 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| mahuclo74@gmail.com | 570.103695155046 | 0 | 570.103695155046 | Completed BTCY buy paid by USDT |
| mr.zakimed@gmail.com | 478.4486031 | 0 | 478.4486031 | Completed BTCY buy paid by USDT |
| abbasafvasibi@gmail.com | 473.568934 | 0 | 473.568934 | Completed BTCY buy paid by USDT |
| billyoktria31@gmail.com | 468.37671171 | 0 | 468.37671171 | Completed BTCY buy paid by USDT |
| traylil503@gmail.com | 405.250465 | 0 | 405.250465 | Completed BTCY buy paid by USDT |
| alexanderchirino@gmail.com | 314.318496208114 | 0 | 314.318496208114 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| ammanullah60@gmail.com | 300.49 | 30 | 270.49 | Completed BTCY buy paid by USDT |
| lili@azooca.com | 255.01 | 0 | 255.01 | Completed BTCY buy paid by USDC; Completed BTCY buy paid by USDT |
| brsaraivar@gmail.com | 215.833180741617 | 0 | 215.833180741617 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| hatchtage@yahoo.com | 794.551040441456 | 601.99 | 192.561040441456 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order; Completed BTCY buy paid by USDC |
| ismiauliamaghfirah@gmail.com | 167.5473644 | 0 | 167.5473644 | Completed BTCY buy paid by USDT |
| yuyunm665@gmail.com | 167.1402304 | 0 | 167.1402304 | Completed BTCY buy paid by USDT |
| jatipersadamandiri77@gmail.com | 166.9 | 0 | 166.9 | Completed BTCY buy paid by USDT |
| sugihbanda117@gmail.com | 166.9 | 0 | 166.9 | Completed BTCY buy paid by USDT |
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
| dilqamshabanov@gmail.com | 93.467037307088 | 0 | 93.467037307088 | completed PayPal payment linked to BTCY buy order |
| lashashervashidze@gmail.com | 92.706587359271 | 0 | 92.706587359271 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| gafterjun@gmail.com | 35.962795558619 | 0 | 35.962795558619 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| h4a5i@punkproof.com | 13.957474367098 | 0 | 13.957474367098 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| joritakahashi@gmail.com | 8.200080655993 | 0 | 8.200080655993 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| sunkuomkarsai12121@gmail.com | 91.493752623583 | 90 | 1.493752623583 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| agustinussinaga0@gmail.com | 0.702336894328 | 0 | 0.702336894328 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| alexanderkruus@gmail.com | 127.13 | 127 | 0.13 | Completed BTCY buy paid by USDC |
| iyastopik63@gmail.com | 166.1 | 166 | 0.1 | Completed BTCY buy paid by USDT |

## Notes

- The `Used BTCY` column is BTCY already consumed from the sell allowance. It comes from prior BTCY sell orders that are not cancelled/expired and completed BTCY convert orders where BTCY was the input.
- This is a point-in-time snapshot. New completed BTCY buys increase the allowance.
- New BTCY sell orders or completed BTCY converts reduce the allowance.
- Very small remaining values are kept exactly as returned by the database calculation.

## Used BTCY Sources

Only users with non-zero `Used BTCY` are listed here.

| Email | Used BTCY | Where It Was Used | Order ID | Status | Date |
| --- | ---: | --- | --- | --- | --- |
| adika5480@gmail.com | 1 | Completed BTCY convert from BTCY to USDT | 25747459 | Completed | 2026-02-13T23:16:05.725Z |
| adika5480@gmail.com | 5000 | Completed BTCY convert from BTCY to USDT | 29011127 | Completed | 2026-02-16T20:08:26.618Z |
| adika5480@gmail.com | 100 | Completed BTCY convert from BTCY to USDT | 39707155 | Completed | 2026-04-18T11:22:05.570Z |
| adika5480@gmail.com | 110 | Completed BTCY convert from BTCY to USDT | 87246469 | Completed | 2026-02-14T10:15:01.904Z |
| alexanderkruus@gmail.com | 127 | Completed BTCY convert from BTCY to USDT | 30519820 | Completed | 2026-03-30T03:34:44.366Z |
| alwinwise4@gmail.com | 71771 | Completed BTCY convert from BTCY to USDT | 38306808 | Completed | 2026-01-26T20:28:57.474Z |
| alwinwise4@gmail.com | 830 | Completed BTCY convert from BTCY to USDT | 50696507 | Completed | 2025-12-17T17:17:52.581Z |
| ammanullah60@gmail.com | 10 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778252493557 | Pending | 2026-05-08T15:01:33.557Z |
| ammanullah60@gmail.com | 10 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778606823149 | Pending | 2026-05-12T17:27:03.149Z |
| ammanullah60@gmail.com | 10 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778691677762 | Pending | 2026-05-13T17:01:17.763Z |
| arslandev180@gmail.com | 0.1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | 1776929520106 | Completed | 2026-04-23T07:37:19.274Z |
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
| arslandev180@gmail.com | 300 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778004130108 | Pending | 2026-05-05T18:02:10.108Z |
| arslandev180@gmail.com | 1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778070111705 | Pending | 2026-05-06T12:21:51.705Z |
| arslandev180@gmail.com | 1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778073574025 | Pending | 2026-05-06T13:19:34.025Z |
| arslandev180@gmail.com | 1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778073647024 | Pending | 2026-05-06T13:20:47.024Z |
| arslandev180@gmail.com | 1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778076610119 | Pending | 2026-05-06T14:10:10.119Z |
| arslandev180@gmail.com | 100 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778077156012 | Pending | 2026-05-06T14:19:16.012Z |
| arslandev180@gmail.com | 100 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778077348391 | Pending | 2026-05-06T14:22:28.391Z |
| arslandev180@gmail.com | 100 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778077788879 | Pending | 2026-05-06T14:29:48.880Z |
| arslandev180@gmail.com | 100 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778077875477 | Pending | 2026-05-06T14:31:15.477Z |
| arslandev180@gmail.com | 100 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778078167019 | Pending | 2026-05-06T14:36:07.019Z |
| arslandev180@gmail.com | 1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778078368493 | Pending | 2026-05-06T14:39:28.493Z |
| arslandev180@gmail.com | 1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778078827796 | Pending | 2026-05-06T14:47:07.796Z |
| arslandev180@gmail.com | 1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778078985534 | Completed | 2026-05-06T15:18:25.595Z |
| arslandev180@gmail.com | 1 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778080843551 | Completed | 2026-05-06T15:21:00.362Z |
| arslandev180@gmail.com | 10 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778246878097 | Pending | 2026-05-08T13:27:58.097Z |
| arslandev180@gmail.com | 10 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778247671385 | Pending | 2026-05-08T13:41:11.385Z |
| bitcoinlightening2929@gmail.com | 5000 | Completed BTCY convert from BTCY to USDT | 40942357 | Completed | 2026-02-20T02:25:28.213Z |
| ekopastikaya95@gmail.com | 860 | Completed BTCY convert from BTCY to USDT | 32515660 | Completed | 2025-12-18T14:31:00.591Z |
| gbevougatto@gmail.com | 720 | Completed BTCY convert from BTCY to USDT | 99820823 | Completed | 2025-12-20T08:14:50.980Z |
| gulraiz726@gmail.com | 750 | Completed BTCY convert from BTCY to USDT | 38572748 | Completed | 2025-12-17T18:21:07.757Z |
| halderraj1701@gmail.com | 1000 | Completed BTCY convert from BTCY to IUSD+ | 62216170 | Completed | 2025-07-14T21:10:05.282Z |
| hatchtage@yahoo.com | 601.99 | Completed BTCY convert from BTCY to USDT | 72181825 | Completed | 2026-02-10T05:21:24.326Z |
| iyastopik63@gmail.com | 166 | Completed BTCY convert from BTCY to USDT | 26533346 | Completed | 2026-04-26T09:06:39.286Z |
| kuswantokus884@gmail.com | 1080 | Completed BTCY convert from BTCY to USDT | 88398452 | Completed | 2026-01-23T14:47:48.849Z |
| lambert.nabil@doodrops.org | 19 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778321335506 | Pending | 2026-05-09T10:08:55.506Z |
| muhdbalaweenty712@gmail.com | 1120 | Completed BTCY convert from BTCY to USDT | 52705645 | Completed | 2025-12-18T10:18:36.007Z |
| muhdbalaweenty712@gmail.com | 780 | Completed BTCY convert from BTCY to USDT | 52761079 | Completed | 2026-01-21T13:19:03.691Z |
| omkar@azooca.com | 10 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778265573685 | Pending | 2026-05-08T18:39:33.685Z |
| rongkomodo1990@gmail.com | 830 | Completed BTCY convert from BTCY to USDT | 82467066 | Completed | 2025-12-18T21:53:14.488Z |
| simonazzahraeva1@gmail.com | 1080 | Completed BTCY convert from BTCY to USDT | 12105285 | Completed | 2025-12-17T17:13:41.015Z |
| sujektogayam@gmail.com | 1070 | Completed BTCY convert from BTCY to USDT | 83381856 | Completed | 2025-12-18T16:40:30.336Z |
| sultanthankyou92@gmail.com | 249.38 | Completed BTCY convert from BTCY to USDT | 74294714 | Completed | 2026-05-08T15:15:03.013Z |
| sunkuomkarsai@gmail.com | 1000 | Completed BTCY convert from BTCY to USDT | 18609183 | Completed | 2025-12-16T16:29:38.447Z |
| sunkuomkarsai@gmail.com | 1000 | Completed BTCY convert from BTCY to USDT | 22990211 | Completed | 2025-12-16T14:25:04.177Z |
| sunkuomkarsai@gmail.com | 1000 | Completed BTCY convert from BTCY to USDT | 24427445 | Completed | 2025-12-16T14:19:17.500Z |
| sunkuomkarsai@gmail.com | 1000 | Completed BTCY convert from BTCY to IUSD+ | 25545289 | Completed | 2025-12-16T15:24:04.415Z |
| sunkuomkarsai@gmail.com | 80 | Completed BTCY convert from BTCY to USDT | 44751789 | Completed | 2025-12-16T15:35:12.841Z |
| sunkuomkarsai@gmail.com | 1000 | Completed BTCY convert from BTCY to IUSD+ | 80083460 | Completed | 2025-12-16T14:17:07.680Z |
| sunkuomkarsai12121@gmail.com | 90 | Completed BTCY convert from BTCY to IUSD+ | 79305373 | Completed | 2025-04-18T15:33:17.766Z |
| sunkuomkarsai5@gmail.com | 100 | Completed BTCY convert from BTCY to USDT | 39585035 | Completed | 2025-12-16T16:02:49.288Z |
| sunkuomkarsai5@gmail.com | 500 | Completed BTCY convert from BTCY to IUSD+ | 57925231 | Completed | 2025-12-16T16:03:16.531Z |
| sunkuomkarsai5@gmail.com | 800 | Completed BTCY convert from BTCY to USDT | 85773315 | Completed | 2025-12-16T16:22:25.324Z |
| sunkuomkarsai5@gmail.com | 500 | Completed BTCY convert from BTCY to USDT | 98346029 | Completed | 2025-12-16T15:48:00.398Z |
| usmanwunti2020@gmail.com | 910 | Completed BTCY convert from BTCY to USDT | 76955452 | Completed | 2026-01-21T12:51:19.516Z |
| usmanwunti2020@gmail.com | 800 | Completed BTCY convert from BTCY to USDT | 94642124 | Completed | 2025-12-18T14:39:02.464Z |
| waitun597346@gmail.com | 1 | Completed BTCY convert from BTCY to IUSD+ | 60723416 | Completed | 2025-07-24T17:15:47.525Z |
| waitun597346@gmail.com | 4 | Completed BTCY convert from BTCY to IUSD+ | 77094519 | Completed | 2025-07-24T07:43:49.413Z |
| waitun597346@gmail.com | 10 | Completed BTCY convert from BTCY to IUSD+ | 81942174 | Completed | 2025-07-24T17:16:02.728Z |
| waitun597346@gmail.com | 497 | Completed BTCY convert from BTCY to IUSD+ | 96714235 | Completed | 2025-07-25T00:58:54.057Z |
