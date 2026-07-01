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
- E = Exit (fully closed, trade completed)
- X = Assigned/Exercised event

### Trade Header (one-time record per trade id)

| trade_id | symbol |  open_date |   contracts | width | quantity | price |                                                     reasoning | status | win_prob |
| -------: | -----: | ---------: | ----------: | ----: | -------: | ----: | ------------------------------------------------------------: | -----: | -------: |
|       T1 |   SPXW | 2026-06-26 | 7250P+7620C |    10 |        2 |  2.00 |                      7250 should be a very safe support level |   OPEN |      73% |
|       T2 |   SPXW | 2026-06-26 |       7310P |    10 |        2 |  2.85 |              flying low, prepared to roll into earning reason | CLOSED |      73% |
|       T3 |   SPXW | 2026-06-29 |       7290P |    10 |        2 |  1.75 | negative gamma selloff, I believe it will quickly bounce back | CLOSED |          |
|       T4 |   SPXW | 2026-06-29 |       7700P |    10 |        2 |  1.75 | negative gamma selloff, I believe it will quickly bounce back | CLOSED |          |

### Event Ledger (append only)

| event_id |       date |  time | legs_before |           action |  legs_after | qty_delta | net_cash_flow | spot |          iv |   vix |                 reasoning | note |
| -------: | ---------: | ----: | ----------: | ---------------: | ----------: | --------: | ------------: | ---: | ----------: | ----: | ------------------------: | ---: |
| T1-01(O) | 2026-06-26 | 14:46 |        flat | open 7250P+7620C | 7250P+7620C |        +2 |          +400 | 7360 | P:20% C:11% | 18~19 |                safe bound |      |
| T2-01(O) | 2026-06-26 | 15:58 |        flat |       open 7310P |       7310P |        +2 |          +570 | 7355 | P:20% C:11% | 18~19 |               risk taking |      |
| T3-01(O) | 2026-06-29 | 09:59 |        flat |       open 7290P |       7290P |        +2 |          +350 | 7380 |       P:19% |  17.7 |              sell the dip |      |
| T4-01(O) | 2026-06-29 | 10:11 |        flat |       open 7270P |       7270P |        +2 |          +390 | 7360 |       P:20% |  17.7 |              sell the dip |      |
| T2-02(E) | 2026-06-30 | 15:58 | 7300P/7310P |             exit |        flat |        -2 |           -50 | 7496 |             |    16 | reward not worth the risk |      |
| T3-02(E) | 2026-06-30 | 15:58 | 7280P/7290P |             exit |        flat |        -2 |           -40 | 7496 |             |    16 | reward not worth the risk |      |
| T4-02(E) | 2026-06-30 | 15:58 | 7260P/7270P |             exit |        flat |        -2 |           -30 | 7496 |             |    16 | reward not worth the risk |      |
