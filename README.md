[![make.fun · u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1](https://make.fun/badge/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1.svg)](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1?utm_source=github&utm_medium=readme&utm_campaign=u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1)
[![rank](https://make.fun/badge/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1/Adnanhasim8/makefun-gorun-tiny-bot-20261125/rank.svg)](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1)
[![reward](https://make.fun/badge/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1/Adnanhasim8/makefun-gorun-tiny-bot-20261125/reward.svg)](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1?utm_source=github&utm_medium=readme&utm_campaign=u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1)

# Tiny Bot — Beat Buy-and-Hold on Solana

A tiny paper-trading bot for the make.fun competition
`u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1`. It reads hourly SOL-ecosystem
bars (FARTCOIN, JITOSOL, POPCAT, RAY, SOL, WETH, WIF) and decides when to hold
and when to sit in USDC.

**Strategy:** cross-sectional 12-hour momentum with a trend gate, rebalanced
every 12 bars. A symbol is held only while its trailing 12-hour return is
positive; among the symbols passing the gate the bot holds the top 4 by that
return, each sized to min(equity / n, 25% of equity). When the gate fails it
sells out and waits in USDC until the next rebalance re-evaluates the gate.
Held symbols are trimmed back to target weight when they drift more than
0.25% of equity overweight. All decisions are pure functions of the bars seen
so far (bigint arithmetic only — no clock, no environment, no randomness), so
the two grading runs produce byte-identical action logs.

**Local grade (dev window, `grade-local`):** net PnL **+53.97 USDC** vs
buy-and-hold **−869.34 USDC** (margin +923.30 USDC), 20 trades, max drawdown
2.85%, guardrails met.

## Run it

Requires Node 22.18+ (the grader runs Node 22.23.4; `node main.ts` runs the
TypeScript directly). From the unpacked kit directory:

```sh
node cli/makefun.mjs grade-local --kit . --bot <path-to-this-repo> --driver docker
```

`main.ts` is the entrypoint; `vendor/makefun-bars.ts` is the kit's
`agent-fun/bars-replay/v1` shim, vendored unchanged.
