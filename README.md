# btc-race-24h-20261115-0828 v2

[![make.fun · btc-race-24h-20261115-0828](https://make.fun/badge/btc-race-24h-20261115-0828.svg)](https://make.fun/bounty/btc-race-24h-20261115-0828?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261115-0828)
[![rank](https://make.fun/badge/btc-race-24h-20261115-0828/Endijuan33/btc-race-24h-20261115-0828-v2/rank.svg)](https://make.fun/bounty/btc-race-24h-20261115-0828/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261115-0828)
[![reward](https://make.fun/badge/btc-race-24h-20261115-0828/Endijuan33/btc-race-24h-20261115-0828-v2/reward.svg)](https://make.fun/bounty/btc-race-24h-20261115-0828?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261115-0828)

A bot for the [make.fun bounty](https://make.fun/bounty/btc-race-24h-20261115-0828): race a BTC trading bot live for 24 hours on cbBTC 1-minute bars, $1,000 paper money. v2 of the submitted "Fleet Plasma" entry — same V-bounce edge, plus a post-washout scalp that makes it positive on both dev windows.

Pure Python, **stdlib only** — the kit's SDK shim is vendored under `vendor/`. Run as `python3 -P -s main.py` (the harness speaks `agent-fun/bars-replay/v1` over stdio).

## Strategy

Two setups, one position at a time, full size, long only:

- **V — deep-drop reversal.** Entry when the close is ≥150 bps below EMA(120) and closes back above EMA(5). Exit on a 60 bps trailing stop from the peak close, or an 80 bps hard stop. No take-profit — the bounce runs.
- **S — grind-day scalp, only after a V trade has completed.** Scalping before the washout means buying into a falling knife (the old dip-buyer lost −$20.23 on the NEW window doing that). Once the washout has turned: entry when the close is ≥100 bps below 50 bars ago, RSI(14) < 30, two consecutive up closes, the last-30-min drop is no worse than −70 bps, and the close is not deep (above EMA(120) − 150 bps). Exit at +100 bps take-profit or −50 bps stop.

## Local grades (real bot through replay.py, exact harness fill model)

| Window | Day shape | Net PnL | Trades | Max DD |
|---|---|---|---|---|
| NEW dev (2026-11-14) | −400 bps, one +4.4% V-bounce | **+$39.57** | 2 | 0.88% |
| OLD dev (2026-11-12/13) | −491 bps grind, no bounce | **+$0.74** | 4 | 1.47% |

Both windows positive (v1 was +$39.57 / −$5.31). Deterministic: two replays byte-identical. The result is a plateau, not a spike — cap 60–80, TP 90–100, SL 45–60, lookback 50–60, drop 80–110, RSI 30–35 all give the identical +$39.57 / +$0.74.
