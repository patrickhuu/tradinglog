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

| trade_id | symbol |  open_date |   contracts | width | quantity | price |                                        reasoning | status | win_prob |
| -------: | -----: | ---------: | ----------: | ----: | -------: | ----: | -----------------------------------------------: | -----: | -------: |
|       T1 |   SPXW | 2026-06-26 | 7250P+7620C |    10 |        2 |  2.00 |         7250 should be a very safe support level |   OPEN |      73% |
|       T2 |   SPXW | 2026-06-26 |       7310P |    10 |        2 |  2.85 | flying low, prepared to roll into earning reason |   OPEN |      73% |

### Event Ledger (append only)

| event_id |       date |  time | legs_before |           action |  legs_after | qty_delta | net_cash_flow | spot |          iv |   vix |   reasoning | note |
| -------: | ---------: | ----: | ----------: | ---------------: | ----------: | --------: | ------------: | ---: | ----------: | ----: | ----------: | ---: |
| T1-01(O) | 2026-06-26 | 14:46 |        flat | open 7250P+7620C | 7250P+7620C |        +2 |          +400 | 7360 | P:20% C:11% | 18~19 |  safe bound |      |
| T2-01(O) | 2026-06-26 | 15:58 |        flat |       open 7310P |       7310P |        +2 |          +570 | 7355 | P:20% C:11% | 18~19 | risk taking |      |
