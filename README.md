# Pramana

# Pramana — system handoff brief

*Give this to another assistant so it understands the system without
re-deriving it. Written August 2026.*

---

## 1. What this system is

**Pramana** (प्रमाण, Sanskrit for *valid knowledge / proof*) is a research
and decision-support system for selling defined-risk options spreads on
NSE stocks.

It lives at `~/pramana/` on an Azure Ubuntu VM. Python 3.12 in a venv.

**Its purpose is not to generate signals. Its purpose is to tell the truth
about whether an idea works** — including when the answer is no.

### The result so far (important context)

The system was built and tested on five years of NSE data (FY22–FY26).
The honest finding:

- The originally-specified strategy (iron condor, stop at 1× credit,
  target 50%) **lost money**: −₹154,783 over 207 trades.
- With the stop removed and a tighter volatility gate, it shows a small
  paper profit: roughly **+₹500–1,000 per trade**.
- **But 0 of 12 parameter configurations had a bootstrap confidence
  interval clearing zero.** The edge cannot be proven at this sample size.
- Everything turns out to be **decided by execution cost (slippage)**,
  which historical bhavcopy physically cannot measure — it has no bid-ask.

So the system is now in a **live micro-trial** whose only job is to measure
one number: actual slippage per leg.

**Do not treat the strategy as validated. It is under test.**

---

## 2. How it works

### The data

Free NSE daily **bhavcopy** — every option, every strike, every expiry,
with price, volume and open interest. No broker API needed for history.

Two file formats exist and both are handled:
- **Legacy** (pre ~July 2024): `SYMBOL`, `OPTSTK`, `VAL_INLAKH`, no
  underlying price, no lot size
- **UDiFF** (post): `TckrSymb`, `STO`, `TtlTrfVal`, `UndrlygPric`,
  `NewBrdLotQty`

Window: 1 Apr 2021 → present. Deliberately not longer — stock options were
cash-settled before 2018-19 and SEBI changed F&O costs in late 2024.
Mixing regimes would flatter the backtest.

### The strategy under test

| Parameter | Value |
|---|---|
| Universe | 25 fixed NSE names (see §3) |
| Structure | Iron condor — both sides, **always** a long wing. No naked legs. |
| Short strikes | nearest **0.15 delta** |
| Long wings | nearest **0.05 delta** beyond each short |
| Entry | first session with **35–45 DTE** into a monthly expiry |
| Gate | **IV percentile ≥ 70** |
| Target | close at **35% of credit** |
| Stop | **none** — the wings cap the loss |
| Time exit | hard at **21 DTE** |
| Size | 1 lot, max 2 concurrent |

### Non-obvious decisions, and why

These were learned the hard way. Do not casually reverse them.

1. **Zero-volume rows are dropped for IV.** If an option didn't trade,
   `close` is stale and `settle` is theoretical. Neither is obtainable.
   Real example: same option, same day, close 285.50 vs settle 451.45.

2. **Spot is derived from the near-month future** (`S = F·e^(−rT)`) for
   *all* years, not just years missing `UndrlygPric`. IV rank looks back
   12 months; changing method mid-sample would create a fake signal.
   Validated: median gap vs NSE's own figure is −0.05%.

3. **IV is measured at a constant 30-day maturity.** Near-month ATM IV
   rises mechanically as expiry approaches — that creates a sawtooth that
   tracks the calendar, not fear.

4. **Gate on IV *percentile*, not IV *rank*.** Rank = (today − min)/(max −
   min), so one spike destroys it. ADANIENT hit 231% IV during Hindenburg
   and its rank was ≥50 on only 6% of days afterwards.

5. **No stop loss.** Counterfactual showed **69.8% of trades that breached
   −1× credit recovered**. On a defined-risk position the wing already caps
   the loss; a stop just converts temporary loss into permanent loss.
   *(This does NOT generalise to naked positions.)*

6. **Corporate actions are detected via strike-grid reissue, not price
   gaps.** A price trigger missed RELIANCE's Jio demerger entirely (the
   near-month future had already priced it in, so there was no gap) while
   falsely flagging election day 2024-06-04 on three stocks. Jaccard
   overlap of consecutive days' strike sets separates cleanly: adjustments
   0.000–0.275, normal sessions 0.340–1.000.

7. **Lot sizes before mid-2024 are reconstructed** as
   `turnover / (contracts × close)`, pooled per regime and snapped to a
   multiple of 25. A silent default of `1` had previously sized three
   years of trades at one share instead of one lot, producing 0% win rates.

---

## 3. The scripts

All in `~/pramana/scripts/`. Modules in `~/pramana/src/`.

### Pipeline — run in this order

| # | Script | Purpose |
|---|---|---|
| 1 | `download_bhavcopy.py` | Fetch NSE bhavcopy → `data/parquet/fo_stocks/`. Handles both formats. Caches raw zips. |
| 2 | `rank_universe.py` | Rank stocks by liquidity **at 5–8% OTM strikes** (not ATM — ATM is liquid on everything). |
| 3 | `stability_by_year.py` | Per-financial-year liquidity. Exists because liquidity moved **30×** on some names since FY22. |
| 4 | `build_universe.py` | Applies config filters → `output/universe.json`. |
| 5 | `compute_iv.py` | Black-Scholes implied vol for every **traded** option → `data/parquet/iv/`. ~2.5M rows, 96% solved. |
| 6 | `build_iv30.py` | Constant-30-day ATM IV + rank + percentile → `output/iv30.csv`. |
| 7 | `detect_corp_actions.py` | Strike-grid reissue detector → `output/excluded_days.csv`. |
| 8 | `build_lot_sizes.py` | Reconstructs pre-2024 lot sizes → `output/lot_sizes.csv`. |
| 9 | `build_chains.py` | Per-symbol option chains with settle fallback → `data/parquet/chains/`. |

`refresh.sh` runs steps 5–9. Run it after downloading new days.
**It does NOT rebuild the universe** — that's fixed on FY26 liquidity and
should be rebuilt deliberately, about once a year.

### Research — already run, kept for reference

| Script | Purpose |
|---|---|
| `backtest.py` | Iron condor engine over the full history |
| `sweep.py` | Grid over exit rules + the stop-loss counterfactual |
| `analyse.py` | Bootstrap CIs, slippage sensitivity, IV gate variants |

### Live — used daily

| Script | Purpose |
|---|---|
| `daily.py` | **The only forward-looking script.** Scans the universe, applies the gate, prints exact strikes. Default posture is NO TRADE. |
| `generate_token.py` | Daily Kite login. Tokens expire each morning. |
| `quote.py` | Live bid/ask on the four legs via Kite instrument master. `--list` shows what strikes actually exist. |
| `journal.py` | Logs every fill against the mid at order time. **This is where slippage gets measured.** |

### Key files

```
output/universe.json        the 25 names
output/universe.csv         with rank, lot size, price
output/iv30.csv             daily IV + percentile per stock
output/excluded_days.csv    corporate-action contamination
output/lot_sizes.csv        per symbol-month
output/trades.csv           backtest trades
output/analysis.csv         the 12-config significance table
output/live/fills.csv       live trial — the real data now
MANIFEST.md                 every file justified in one line
.env                        Kite credentials, chmod 600
```

### The universe (fixed, ranked on FY26 OTM liquidity)

```
RELIANCE  MARUTI  DIXON  INFY  MCX  SBIN  TCS  HAL  HDFCBANK  BEL
BAJFINANCE  TRENT  M&M  BHARTIARTL  TATASTEEL  ICICIBANK  AXISBANK
INDIGO  HEROMOTOCO  BAJAJ-AUTO  ADANIENT  VEDL  KOTAKBANK  LT  HINDALCO
```

---

## 4. Daily routine

### Pre-market (~9:00 AM IST)

```bash
cd ~/pramana && source .venv/bin/activate
python scripts/generate_token.py      # only if quote.py will be used
python scripts/daily.py --show-all
```

- `daily.py` uses the **previous** session's bhavcopy. IV percentile moves
  slowly, so this is acceptable, but be aware of the one-day lag.
- Most days output is **NO TRADE**. That is the expected outcome —
  historically about one month in four qualified per stock.
- If a name passes: **manually check its results calendar.** Earnings
  inside the holding window = skip. This is not automated and is a known gap.

### During market (~2:00–2:30 PM IST, only if something passed)

```bash
python scripts/quote.py --symbol XXXX --list          # confirm strikes exist
python scripts/quote.py --symbol XXXX --expiry YYYY-MM-DD \
    --sc .. --lc .. --sp .. --lp .. --trade TRADE-ID
```

- Capture **bid/ask at the moment of the order** — mandatory, this is the
  whole measurement.
- Execution is **manual** on Sensibull. The system never places orders.
- Then log all four legs with `journal.py fill`.

### Post-market (after ~6:00 PM IST)

```bash
python scripts/download_bhavcopy.py --start YYYY-MM-DD --end YYYY-MM-DD
./scripts/refresh.sh
python scripts/journal.py report      # watch slippage accumulate
```

- Bhavcopy publishes after close.
- `refresh.sh` currently rebuilds all months (~15 min). Making it
  incremental is a known todo.

### Managing open positions

**Nothing monitors open trades.** No alerts, no auto-exit. The user tracks
this manually. Both exit levels are fixed at entry:
- target: 35% of the credit received
- hard exit: 21 DTE, regardless of P&L

---

## 5. Known gaps and planned enhancements

### Gaps (be honest about these)

1. **No earnings filter.** Needs real results dates; must not be inferred
   from volatility patterns.
2. **Daily data only.** Intraday moves are invisible, so the backtest
   cannot see a stop breach that recovered by the close.
3. **`refresh.sh` is not incremental** — rebuilds everything each run.
4. **Nothing monitors open positions.**
5. **Residual IV contamination**: VEDL shows a 2.4% minimum IV and
   RELIANCE 3.4% — both impossible, both leftovers from corporate-action
   windows the mask didn't fully cover.
6. **One-day lag**: decisions use the previous close.
7. **Sample size is a hard ceiling.** 25 stocks × monthly = ~200 trades in
   5 years. Detecting an edge below ~₹1,000/trade would need ~6,000
   trades. This is a limit of the design, not a bug.

### Planned enhancements, in priority order

1. **Measure slippage** (in progress — the live trial). Everything else
   is secondary until this number exists.
2. **Earnings calendar integration** — fetch real dates, gate on them.
3. **Incremental `refresh.sh`** — only process new months.
4. **Position monitor** — alert on target hit or 21 DTE.
5. **Dashboard** — local React page on the VM, deferred until the trial
   produces data worth displaying.
6. **Legging-in variant** — the user prefers entering one spread near
   support and the other near resistance. This *cannot* be backtested
   (it depends on discretionary judgement), so it can only be tested
   against the mechanical baseline once live data exists.

---

## 6. How to be useful on this project

**The user's working style**

- Wants conclusions first, then reasoning. Dislikes long preamble.
- Pushes back hard on verbose or circular answers, and is right to.
- Prefers being told "I don't know, let's check" over a confident guess.
- Builds a pilot, confirms the format, then scales.
- Uses `vi`, not `nano`.

**Principles this system was built on — please respect them**

- **Default posture is NO TRADE.** Many observation points, one decision
  point.
- **Never tune parameters to fix a specific bad year.** FY24 lost ₹60,790
  in a trending rally. Adding a trend filter *after seeing that* would be
  fitting to history, not learning.
- **Fetch ground truth rather than inferring it.** Forty minutes were lost
  building a corporate-action detector for facts NSE publishes.
- **Silent defaults are dangerous.** The lot-size bug produced a plausible
  table, not an error.
- **Distinguish "paper profit" from "proven edge."** They are not the same
  thing and the difference is the entire point of this project.

**What NOT to do**

- Do not present the strategy as validated. It isn't.
- Do not suggest removing the long wings. No naked positions, ever.
- Do not generalise "no stop loss" beyond defined-risk structures.
- Do not lower the IV gate because an entry window is closing.
