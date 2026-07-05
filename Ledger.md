## Trade Lifecycle Log

### State Codes

- O = Open
- A = Add (same thesis, add size)
- TP = Partial Take Profit
- SL = Partial Stop
- R = Roll (same strike structure, later expiry)
- RS = Roll + Shift (change strike and/or structure)
- H = Hedge
- U = Unwind hedge
- E = Expired Worthless
- C = Preemptively Closed
- X = Assigned/Exercised event

### Trade Header (one-time record per trade id)

| trade_id | symbol |  open_date |   contracts | width | quantity | price |                                                     reasoning |            status | win_prob |
| -------: | -----: | ---------: | ----------: | ----: | -------: | ----: | ------------------------------------------------------------: | ----------------: | -------: |
|       T1 |   SPXW | 2026-06-26 | 7250P+7620C |    10 |        2 |  2.00 |                      7250 should be a very safe support level | EXPIRED WORTHLESS |      73% |
|       T2 |   SPXW | 2026-06-26 |       7310P |    10 |        2 |  2.85 |              flying low, prepared to roll into earning reason |            CLOSED |      73% |
|       T3 |   SPXW | 2026-06-29 |       7290P |    10 |        2 |  1.75 | negative gamma selloff, I believe it will quickly bounce back |            CLOSED |          |
|       T4 |   SPXW | 2026-06-29 |       7270P |    10 |        2 |  1.75 | negative gamma selloff, I believe it will quickly bounce back |            CLOSED |          |
|       T5 |   SPXW | 2026-07-02 |       7370P |    10 |        3 |  1.95 |  weak NFP, Earning season around the corner, Chips dip buying |              OPEN |      72% |
|       T6 |   SPXW | 2026-07-02 |       7350P |    10 |        2 |  1.95 |  weak NFP, Earning season around the corner, Chips dip buying |              OPEN |      72% |

### Event Ledger (append only)

| event_id |       date |  time |             legs_before |           action |  legs_after | qty_delta | net_cash_flow | spot |          iv |   vix |                 reasoning | note |
| -------: | ---------: | ----: | ----------------------: | ---------------: | ----------: | --------: | ------------: | ---: | ----------: | ----: | ------------------------: | ---: |
| T1-01(O) | 2026-06-26 | 14:46 |                    flat | open 7250P+7620C | 7250P+7620C |        +2 |          +400 | 7360 | P:20% C:11% | 18~19 |                safe bound |      |
| T2-01(O) | 2026-06-26 | 15:58 |                    flat |       open 7310P |       7310P |        +2 |          +570 | 7355 | P:20% C:11% | 18~19 |               risk taking |      |
| T3-01(O) | 2026-06-29 | 09:59 |                    flat |       open 7290P |       7290P |        +2 |          +350 | 7380 |       P:19% |  17.7 |              sell the dip |      |
| T4-01(O) | 2026-06-29 | 10:11 |                    flat |       open 7270P |       7270P |        +2 |          +390 | 7360 |       P:20% |  17.7 |              sell the dip |      |
| T2-02(C) | 2026-06-30 | 15:58 |             7300P/7310P |             exit |        flat |        -2 |           -50 | 7496 |             |    16 | reward not worth the risk |      |
| T3-02(C) | 2026-06-30 | 15:58 |             7280P/7290P |             exit |        flat |        -2 |           -40 | 7496 |             |    16 | reward not worth the risk |      |
| T4-02(C) | 2026-06-30 | 15:58 |             7260P/7270P |             exit |        flat |        -2 |           -30 | 7496 |             |    16 | reward not worth the risk |      |
| T5-01(O) | 2026-07-02 | 10:00 |                    flat |             open |  7360/7370P |        -3 |          +585 | 7470 |       P:16% |    16 |              sell the dip |      |
| T6-01(O) | 2026-07-02 | 11:00 |                    flat |             open |  7340/7350P |        -2 |          +390 | 7450 |       P:16% |    16 |              sell the dip |      |
| T1-01(E) | 2026-07-02 | 24:00 | 7240/7250P + 7620/7630C |             flat | 7250P+7620C |        -2 |             0 | 7483 |             |    16 |                           |      |
