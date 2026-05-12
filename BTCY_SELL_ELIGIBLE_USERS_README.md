# BTCY Sell Eligible Users Snapshot

Generated at: `2026-05-12T12:52:42.408Z`

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
| meliawaty65@gmail.com | 1261 | 0 | 1261 | Completed BTCY buy paid by USDT |
| singjayabandung@gmail.com | 1008.8 | 0 | 1008.8 | Completed BTCY buy paid by USDT |
| sergejkirsch79@gmail.com | 999.8027761854773 | 0 | 999.8027761854773 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| ms9820639@gmail.com | 956.7527819 | 0 | 956.7527819 | Completed BTCY buy paid by USDT |
| sangmey14@gmail.com | 913.01516778 | 0 | 913.01516778 | Completed BTCY buy paid by USDT |
| mahuclo74@gmail.com | 570.1036951550464 | 0 | 570.1036951550464 | Completed BTCY buy paid by USDT |
| mr.zakimed@gmail.com | 478.4486031 | 0 | 478.4486031 | Completed BTCY buy paid by USDT |
| abbasafvasibi@gmail.com | 473.568934 | 0 | 473.568934 | Completed BTCY buy paid by USDT |
| traylil503@gmail.com | 405.250465 | 0 | 405.250465 | Completed BTCY buy paid by USDT |
| alexanderchirino@gmail.com | 314.3184962081141 | 0 | 314.3184962081141 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| ammanullah60@gmail.com | 300.49 | 10 | 290.49 | Completed BTCY buy paid by USDT |
| lili@azooca.com | 255.01 | 0 | 255.01 | Completed BTCY buy paid by USDC and USDT |
| brsaraivar@gmail.com | 215.8331807416175 | 0 | 215.8331807416175 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| hatchtage@yahoo.com | 794.5510404414563 | 601.99 | 192.56104044145627 | Completed BTCY buy paid by PayPal and USDC; completed PayPal payment linked to BTCY buy order |
| ismiauliamaghfirah@gmail.com | 167.5473644 | 0 | 167.5473644 | Completed BTCY buy paid by USDT |
| yuyunm665@gmail.com | 167.1402304 | 0 | 167.1402304 | Completed BTCY buy paid by USDT |
| jatipersadamandiri77@gmail.com | 166.9 | 0 | 166.9 | Completed BTCY buy paid by USDT |
| sugihbanda117@gmail.com | 166.9 | 0 | 166.9 | Completed BTCY buy paid by USDT |
| nugraha.indra0221@gmail.com | 165.350224 | 0 | 165.350224 | Completed BTCY buy paid by USDT |
| songthanh9999@gmail.com | 162.7481908 | 0 | 162.7481908 | Completed BTCY buy paid by USDT |
| billyoktria31@gmail.com | 156.54561797 | 0 | 156.54561797 | Completed BTCY buy paid by USDT |
| muhammadmarjuki1203@gmail.com | 147.614549 | 0 | 147.614549 | Completed BTCY buy paid by USDT |
| medygalagala388@gmail.com | 132.56859518915638 | 0 | 132.56859518915638 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| cholidaja45@gmail.com | 123.50222464360301 | 0 | 123.50222464360301 | Completed BTCY buy paid by USDT |
| ppatel5169@gmail.com | 114.19038162996493 | 0 | 114.19038162996493 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| gerald_geraldgplj@yahoo.com | 111.21456537125424 | 0 | 111.21456537125424 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| drone1215@gmail.com | 110.98780366124346 | 0 | 110.98780366124346 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| taraboi103@gmail.com | 110.46196297535926 | 0 | 110.46196297535926 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| rakeyteah0@gmail.com | 107.25858846332473 | 0 | 107.25858846332473 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| dilqamshabanov@gmail.com | 93.46703730708754 | 0 | 93.46703730708754 | Completed PayPal payment linked to user BTCY buy order |
| lashashervashidze@gmail.com | 92.70658735927141 | 0 | 92.70658735927141 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| gafterjun@gmail.com | 35.96279555861899 | 0 | 35.96279555861899 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| h4a5i@punkproof.com | 13.957474367098326 | 0 | 13.957474367098326 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| joritakahashi@gmail.com | 8.200080655993332 | 0 | 8.200080655993332 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| sunkuomkarsai12121@gmail.com | 91.49375262358335 | 90 | 1.4937526235833474 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| agustinussinaga0@gmail.com | 0.7023368943277079 | 0 | 0.7023368943277079 | Completed BTCY buy paid by PayPal; completed PayPal payment linked to BTCY buy order |
| alexanderkruus@gmail.com | 127.13 | 127 | 0.12999999999999545 | Completed BTCY buy paid by USDC |
| iyastopik63@gmail.com | 166.1 | 166 | 0.09999999999999432 | Completed BTCY buy paid by USDT |

## Notes

- The `Used BTCY` column is BTCY already consumed from the sell allowance. It comes from prior BTCY sell orders that are not cancelled/expired and completed BTCY convert orders where BTCY was the input.
- This is a point-in-time snapshot. New completed BTCY buys increase the allowance.
- New BTCY sell orders or completed BTCY converts reduce the allowance.
- Very small remaining values are kept exactly as returned by the database calculation.

## Used BTCY Sources

Only users with non-zero `Used BTCY` are listed here.

| Email | Used BTCY | Where It Was Used | Order ID | Status | Date |
| --- | ---: | --- | --- | --- | --- |
| ammanullah60@gmail.com | 10 | BTCY sell order reserved against allowance, because it is not cancelled or expired | CRYPTO_SELL1778252493557 | Pending | 2026-05-08T15:01:33.557Z |
| hatchtage@yahoo.com | 601.99 | Completed BTCY convert from BTCY to USDT | 72181825 | Completed | 2026-02-10T05:21:24.326Z |
| sunkuomkarsai12121@gmail.com | 90 | Completed BTCY convert from BTCY to IUSD+ | 79305373 | Completed | 2025-04-18T15:33:17.766Z |
| alexanderkruus@gmail.com | 127 | Completed BTCY convert from BTCY to USDT | 30519820 | Completed | 2026-03-30T03:34:44.366Z |
| iyastopik63@gmail.com | 166 | Completed BTCY convert from BTCY to USDT | 26533346 | Completed | 2026-04-26T09:06:39.286Z |
