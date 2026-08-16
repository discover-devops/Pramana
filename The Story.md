# Pramana — building an options system that told me "no"

**A one-day build log: from "what is a covered call?" to a tested null result.**

*Rahul Chaubey · 16 August 2026*

---

## Why this document exists

Here's the rewrite, in your voice:

---

## Why this document exists

Everybody wants a strategy.

We all carry the same belief: that somewhere out there is the right strategy, and the day I find it, my problems are solved. So we watch a hundred YouTube videos looking for the magic thing. 

Especially in option trading. And every few weeks you hear something in a video or a podcast and you think: **yes. This is it. This is the thing I was looking for.**

Then you start implementing it. You take a few losses. And you simply stop.

Why does that happen? Not because the strategy was bad. Sometimes it's a strategy that genuinely works for the person teaching it. It happens because **you never saw the evidence that made it credible.** You inherited someone else's conclusion without their reasoning. So the first time it hurts, you have nothing to hold on to — no reason to believe the drawdown is normal rather than fatal.

That's why I won't teach you a strategy.

What actually transfers is the **process**. Learning to build a system that tells you the truth — including when the truth is disappointing. If you know these things before you place a single real trade, then you've learnt something that survives the drawdown.

That's what I'm trying to share here.

**The honest headline, up front:**

> We built the system, tested it properly on five years of NSE data, and found **no statistically significant edge**. The result turned out to be decided almost entirely by execution cost — a number historical data physically cannot contain.

That's not a failure. That's the system working. A backtest that says yes to everything is a backtest that was never asked a hard question.

What I can share is a **thought process** — how to go from a vague idea to a
system that tells you the truth, including when the truth is disappointing.

This is the log of one day. The honest headline:

> **We built the system, tested it properly on five years of NSE data, and found
> no statistically significant edge.** The result turned out to be decided almost
> entirely by execution cost — a number that historical data physically cannot
> contain.

That's not a failure. That's the system working. A backtest that says "yes" to
everything is a backtest that hasn't been asked hard questions.

---

## Part 1 — The question I started with

I wanted to understand **covered calls**. You own the shares, you sell a call
against them, you keep the premium.

The mechanics were easy. The problem showed up when I described what I actually
wanted:

> *"I want to buy fundamentally strong stocks, hold them for my children, and
> earn premium on top."*

Both halves of that sentence are reasonable. Together they contradict each other.

### The contradiction

If a stock runs to 1700 and my call was at 1550, I get assigned. I keep the
premium, so my statement shows a profit. But I no longer own the shares — and to
own them again I must pay 1700.

**Same rupees, fewer shares.**

A generational holding is bought precisely *for* the big multi-year moves.
Covered calls systematically sell those moves away. Over ten years the premium
income rarely compensates for missing two or three of the large legs up, because
those legs are where nearly all the compounding happens.

Then the tax layer makes it worse in India:
- Every assignment is a sale → triggers LTCG and **resets the holding period**
- A share held untouched for 15 years is taxed **once**
- The same share written and re-bought 40 times is taxed **40 times**

### First real lesson

> **Options income and long-term compounding pull against each other.**
> No clever leg structure fixes this. You have to separate the money.

---

## Part 2 — Checking whether I needed options at all

Before designing anything, one arithmetic check.

**Goal:** ₹1 crore → ₹5 crore for my children.

| Horizon | Required CAGR |
|---|---|
| 10 years | 17.5% |
| 15 years | 11.3% |
| 18 years | 9.4% |

With a 15-year horizon, I need roughly **what a Nifty index fund has historically
delivered**. No options required.

That was worth sitting with. I was about to design a complex machine to reach a
target that a boring one already reaches.

### The structure I settled on

- **~70% generational core** — index funds / quality businesses. **Never write a
  single call against it.** Its job is to capture the big moves.
- **~30% income sleeve** — this is where systematic option selling runs, on
  positions I'm genuinely indifferent about losing.

The two buckets never touch. That separation is the whole risk-management design.

---

## Part 3 — What the research actually says about covered calls

I checked rather than assumed. The evidence is period-dependent and both camps
quote it selectively.

**The bull case:** Callan's 18-year study found the BXM buy-write index returned
11.77% vs 11.67% for the S&P — at roughly **two-thirds the volatility**.

**The bear case:** Over the last ten years BXM delivered only about **one-third**
of the S&P's return. And in *every* year the S&P gained 6% or more, you'd have
been better off simply owning the index.

**On collars** (buying a put to protect, funded by selling a call): AQR found OTM
puts are consistently more expensive than the OTM calls you'd sell against them,
so a collar should be *expected* to hurt risk-adjusted performance. The CBOE
95-110 collar index returned ~5.2% annualised vs 7.3% unhedged over 28.5 years.

> **Covered calls are a volatility-harvesting trade.** They win in flat and
> falling markets, lose in strong ones. India over the next 15 years is more
> likely to look like a strong market than a flat one.

---

## Part 4 — The arithmetic that shapes everything

Before writing a line of code, I did the one calculation that determines whether
any premium-selling system can work.

**Setup:** an iron condor 50 points wide, collecting 15.
- Max profit = 15
- Max loss = 35

If I exit at 40% of max profit, I bank 6 per win. For break-even:

```
6 × W = 35 × (1 − W)   →   W = 85%
```

**Eighty-five percent wins just to break even.** Before costs.

Now change the stop. Cap the loss at 1× the credit received:

```
required win rate ≈ 71%
```

Move the profit target from 40% to 50% as well:

```
required win rate ≈ 67%
```

### The lesson that shaped the whole build

> **The stop — not the entry — sets your required accuracy.**
>
> "Small profit, small loss" only works if the losses are genuinely small. In a
> credit spread they aren't, unless you force them to be.

This is why I stopped wanting to trade and started wanting to *build*. I didn't
know my own numbers. Without them, any entry rule is decoration.

---

## Part 5 — Decision: build first, trade later

The commitment:

- Build the full system before placing a single trade
- Every threshold from **data**, not from opinion
- मेरा मन कर रहा है → not a valid input

Hence the name.

> **Pramana (प्रमाण)** — in Indian epistemology, *a valid means of knowledge*.
> Evidence that justifies a belief. The system exists so decisions rest on
> pramana rather than on how I feel at 2pm.

Four "businesses" already existed in my setup (Nifty, Bank Nifty, stocks,
directional). This was Business #3, built clean in its own folder — `~/pramana/` —
deliberately separate from the older codebase, which had accumulated files I no
longer used.

**Discipline that came with it:** a `MANIFEST.md` where every file in `src/` and
`scripts/` gets one line explaining why it exists. No line = deletable. Plus a
`scratch/` directory that can be wiped without a second thought.

---

## Part 6 — The data

### Where it comes from

Historical option chains do **not** come from a broker API. Kite gives candles for
instruments that currently exist; option contracts from 2021 are long expired and
delisted.

The source is **NSE's daily F&O bhavcopy** — a free public archive, no
authentication.

**Window chosen: 1 April 2021 → 31 March 2026** (FY22–FY26).

Not longer, deliberately. Stock options in India were cash-settled until physical
settlement was phased in during 2018-19 — expiry behaviour changed completely.
SEBI's late-2024 F&O measures changed costs again. A backtest stretching to 2014
would mix regimes that don't belong together, and the old data would flatter me.

### Challenge #1 — two file formats

NSE changed the bhavcopy format mid-2024.

| | Legacy (pre 2024-07) | UDiFF (post) |
|---|---|---|
| Symbol column | `SYMBOL` | `TckrSymb` |
| Instrument tag | `OPTSTK` / `FUTSTK` | `STO` / `STF` |
| Turnover | `VAL_INLAKH` (lakhs) | `TtlTrfVal` (rupees) |
| Underlying price | **absent** | `UndrlygPric` |
| Lot size | **absent** | `NewBrdLotQty` |

Both had to be normalised to one schema. Turnover units differ by 10⁵ — easy to
get silently wrong.

**Result:** 1,233 trading days, zero errors, 1.1 GB raw, 629 MB parquet.

### Challenge #2 — settle price is not a real price

Every row has both `close` and `settle`. For an option that **didn't trade**,
`close` is a stale carry-forward and `settle` is NSE's theoretical mark.

Real example from the data:

```
2021-04-05  AARTIIND 880 CE   close 285.50   settle 451.45   contracts 0
```

Same option, same day, two prices differing by 58%.

**Decision:** drop every zero-volume row for IV computation. If nobody traded it,
I couldn't have traded it either — its IV is fictional. Fewer data points, all
real. (Settle is kept separately, but only as a mark-to-market for positions
already open.)

---

## Part 7 — Building the universe (this took several attempts)

### Challenge #3 — measuring liquidity in the wrong place

My first instinct was to rank stocks by option volume. Wrong.

**At-the-money is liquid on almost everything.** The strikes I'd actually sell —
5–8% out of the money — often aren't. That's exactly where slippage lives.

So the ranking metric became **median daily notional turnover at 5–8% OTM
strikes**, not ATM.

A synthetic test proved the point: a stock that trades heavily at ATM but not at
5–8% OTM is correctly rejected, despite looking excellent on a naive screen.

### Challenge #4 — liquidity moved 30× in five years

Ranking on a five-year median seemed obvious. The per-year table killed that idea:

| Stock | FY22 | FY26 |
|---|---|---|
| DIXON | ₹35 cr/day | ₹1,070 cr/day |
| MCX | ₹24 cr | ₹994 cr |
| HAL | ₹23 cr | ₹831 cr |
| BEL | ₹66 cr | ₹780 cr |
| TRENT | ₹13 cr | ₹603 cr |

These aren't drifts. Averaging a 2021 market that no longer exists with the 2026
market I'll actually trade is meaningless.

**Fix:** rank on the most recent financial year.

### Challenge #5 — dead tickers vs newborn ones

A coverage filter (must be present 80% of days) correctly excluded HDFC — which
merged into HDFCBANK in July 2023. Seeing that corporate event appear
*unprompted* in the data was the first real confirmation the parsing was sound.

But the same filter also excluded BSE, JIOFIN, DIXON, CDSL — names that are
*extremely* liquid now, just recently listed in F&O. The filter couldn't tell
"died" from "was born late."

**Fix:** require qualification in the trailing 2 financial years, not all 5.

### Challenge #6 — the IDEA trap

Vodafone Idea showed ₹364 cr/day of turnover and failed every year.

Why: it trades near ₹8, with strikes ₹0.50 apart. The 5–8% OTM band spans about
40 paise — often **zero strikes land in it**.

> **Percentage-based strike selection breaks down on low-priced stocks,
> regardless of how much money flows through them.**

**Fix:** a minimum price filter (₹150).

### Also noticed, then also observed in the data

ZOMATO at 0.07 coverage and ETERNAL at 0.19 are the same company before and after
its 2025 rename. TATAMOTORS at 0.91 with TMPV at 0.09 is the demerger. All
visible in the raw liquidity table without being told.

### The final 25

Ranked on FY26 OTM liquidity, requiring two qualifying years, above ₹150:

```
RELIANCE  MARUTI  DIXON  INFY  MCX  SBIN  TCS  HAL  HDFCBANK  BEL
BAJFINANCE  TRENT  M&M  BHARTIARTL  TATASTEEL  ICICIBANK  AXISBANK
INDIGO  HEROMOTOCO  BAJAJ-AUTO  ADANIENT  VEDL  KOTAKBANK  LT  HINDALCO
```

Every filter is in a config file. Every rejected name is printed with the reason
it was rejected — so the universe can be *audited*, not trusted.

---

## Part 8 — Implied volatility, and why the obvious approach is wrong

### Deriving spot

Pre-2024 files have no underlying price. Futures do exist, so:

```
S = F × e^(−r·T)
```

**Decision that mattered:** use this for **all five years**, not just the years
missing the column.

Why: IV rank looks back 12 months. If the method changed in July 2024, then for
~12 months either side the system would be comparing IVs computed two different
ways — a fake signal exactly where it's most dangerous.

**Validation:** median gap vs NSE's own published underlying price: **−0.04%**,
range −0.16% to +0.04%. The assumption held.

**Free bonus:** the future already embeds expected dividends, so a discounted
future is a *dividend-adjusted* spot. Cleaner than raw cash price.

### Solver quality

Newton's method, then bisection for stragglers. First version accepted
convergence in *price* terms — max error 0.57 vol points. Too loose: where vega
is small, a tiny price error implies a large vol error.

**Fix:** acceptance test in **vol** terms (scaled by vega).

```
solve rate 99.09%   max error 1e-5 vol points
```

Result on real data: **2.3 million IV points, 96% solved.**

### Challenge #7 — the sawtooth

The obvious "IV of the stock today" is near-month ATM IV. It's wrong.

**As expiry approaches, ATM IV rises mechanically** — from time, not fear. The
resulting series has a sawtooth that tracks the *calendar*, and IV rank would
fire on the position in the expiry cycle rather than on real volatility.

**Fix:** interpolate to a constant 30-day maturity, in **total variance**
(σ² × t, the additive quantity), between the two expiries straddling 30 days.

Hand-verified: expiries at 20 DTE @ 30% and 51 DTE @ 20% → iv30 = 0.25016. ✓

**A bug this caught:** when only one expiry was available, my code rescaled its
variance to 30 days — turning a genuine 30% IV into 24%. A silent vol signal
manufactured from nothing. Fixed to use that expiry's IV directly.

### Challenge #8 — IV rank is broken by single spikes

Sanity table of 30-day ATM IV by stock:

| Stock | Median IV | Days with rank ≥50 |
|---|---|---|
| ADANIENT | 39.9% | **6.3%** |
| DIXON | 37.9% | 24.3% |
| HDFCBANK | 19.1% | 20.5% |

The levels are right (banks low, Adani/Dixon high). But look at ADANIENT's last
column.

**Cause:** IV rank = (today − min) / (max − min). Adani hit **231% IV** during
Hindenburg. That single spike sets `max` for a full year, crushing every
subsequent day toward zero. The signal is dead for that name.

**Fix:** gate on **IV percentile** (what fraction of the past year was lower) —
immune to spikes. Keep IV rank as a display metric only.

---

## Part 9 — Corporate actions: three attempts to get one thing right

This was the longest detour of the day, and worth documenting honestly as a
lesson in *how not to* diagnose a problem.

### The symptom

RELIANCE showed a minimum IV of **3.4%**. Impossible — its true floor is ~12-14%.

The bad days clustered 11–19 July 2023. That's the **Jio Financial demerger**,
ex-date 20 July.

### Why it happened

The July future expired on the 27th — *after* the ex-date. So from the moment it
became the near month, it was already pricing the post-demerger stock (~₹2,545)
while option strikes still referenced the cash price (~₹2,800).

Derived spot inherits the future's view → moneyness off by ~9% → the ATM band
selects the wrong strikes → garbage IV.

### Attempt 1 — trigger on price gaps (>15%)

Found 8 real splits. Also produced **8 false positives**, including
**2024-06-04 — election results day** — flagged on ADANIENT, BEL and HAL
simultaneously.

> It was proposing to delete the single most informative high-volatility day in
> five years, from three stocks at once.

### Attempt 2 — add breadth + strike-grid checks

Two discriminators:
1. A corporate action hits **one** stock; a market event hits many the same day
2. After a split the **entire strike grid is reissued** — KOTAK's ₹2000 strike
   becomes ₹400, so before/after strike sets share almost nothing

Result: clean separation.

```
confirmed splits   : strike overlap 0.000
rejected as market : strike overlap 0.42 – 1.00
```

Election day sat at 0.90 overlap → auto-rejected. Hindenburg preserved.

**But RELIANCE still wasn't caught** — its demerger moved the price only ~9%,
under the 15% trigger.

### Attempt 3 — invert the design

The realisation: **the strike grid was a far better signal than the price.** So
why was price the trigger at all?

Scanning grid overlap on *every* symbol-day:

```
contract adjustments : 0.000 – 0.275   (29 events)
---- empty gap ----
normal sessions      : 0.340 – 1.000   (30,471 sessions)
```

The threshold picks itself.

**And it found 25 events the price trigger never could:**
- VEDL **nine times** — NSE adjusts F&O contracts for any dividend above 2% of
  market price. Vedanta pays enormous special dividends. Price barely moves;
  whole grid reissued.
- TATASTEEL 4×, TCS buyback, HEROMOTOCO, BHARTIARTL's 2021 rights issue

One more subtlety: for the demerger, the **futures gap was 1.00** — no gap at
all. So the adjustment ratio had to come from **strike medians**, not the futures
price. Reliance: 2800 → 2550 = ratio 0.91.

### The lesson I'd tell anyone

> **I spent 40 minutes iterating a detector when corporate actions are published
> facts I could have simply looked up.**
>
> The detector existed because I didn't want to leave the data I already had, not
> because inference was the right method. When you're stuck: fetch the ground
> truth instead of inferring it.

Final: 27 events, 224 excluded days (0.7% of all symbol-days).

---

## Part 10 — The bug that would have destroyed everything silently

First backtest run:

```
FY22   21 trades   win 0.0%
FY23   34 trades   win 0.0%
FY24   28 trades   win 0.0%
FY25   61 trades   win 31.1%
FY26   63 trades   win 63.5%
```

**Zero percent wins for three years, then 63%.** No strategy does that.

**Cause:** `lot_size` only exists in files from mid-2024. Before that it's `NaN`,
and my fallback quietly used **1**. So every pre-2024 trade was sized at *one
share* instead of a full lot. Gross P&L divided by ~500, fees unchanged → every
old trade a small guaranteed loss.

### The fix — reconstruct from arithmetic already in the data

Futures turnover = contracts × lot × price, therefore:

```
lot_size = turnover / (contracts × close)
```

Two refinements were needed:
1. The quotient carries ~0.25% noise (turnover reflects the *average* traded
   price; I divide by the close). Pooling estimates across a lot-size *regime*
   fixes it.
2. Snap to a multiple of 25 when within 1.5% — NSE lot sizes almost always are.
   ADANIENT's 309 (from a corporate-action adjustment) correctly falls through.

**Validation:** against published values in the overlap period, and against a
live Kite screen —

| Stock | Reconstructed change | Confirmed by |
|---|---|---|
| KOTAKBANK | 400 → 2000, Jan 2026 | 1:5 split (Kite) |
| MCX | 125 → 625, Jan 2026 | 1:5 split (Kite) |
| RELIANCE | 250 → 500, Nov 2024 | 1:1 bonus |
| TATASTEEL | 425 → 4250, Aug 2022 | 1:10 split |

### The lesson

> **The dangerous bugs don't crash. They produce plausible-looking numbers.**
>
> A 0% win rate was obvious. A 15% error would have passed unnoticed and I'd have
> traded on it. Every default value is a silent assumption — make it fail loudly
> instead.

Also worth noting: I initially concluded from bad lot data that KOTAKBANK was
"too chunky to trade" at ₹42 lakh notional. A Kite screenshot showed the real
figure was ₹7.9 lakh — the stock had *split*, and I was multiplying a pre-split
price by a post-split lot. **Live data corrected the analysis.**

---

## Part 11 — The backtest

### Specification

| Parameter | Value |
|---|---|
| Structure | Iron condor, both sides, **always defined risk** |
| Short strikes | 0.15 delta |
| Long wings | ~0.05 delta |
| Entry | First session at 35–45 DTE into monthly expiry |
| Gate | IV percentile ≥ 50 |
| Target | 50% of credit |
| Stop | 100% of credit |
| Time exit | 21 DTE |

**Why delta, not fixed percentage:** at 18% vol the 0.15-delta call sits ~8% OTM;
at 30% vol it's ~14% OTM. A fixed band would sell TCS far too close and VEDL far
too wide. Delta adapts to each stock's regime automatically.

### What could not be tested

I wanted to *leg in* — bull put spread near support, add the bear call at
resistance. **That can't be backtested.** The result depends on judgement calls
that don't exist in the data. So v1 tests the mechanical version, which gives a
baseline that legging can later be measured *against*.

### Result (after the lot fix)

```
trades              207
win rate            60.9%
total net           Rs −154,783
payoff ratio        0.47
break-even winrate  67.8%   (actual 60.9%)
```

**It loses.** And the reason is visible in the payoff ratio: target at 50% of
credit and stop at 100% forces wins to be half a credit and losses a full credit.
That structure *demands* ~68% accuracy. It's the arithmetic from Part 4, showing
up exactly as predicted.

---

## Part 12 — The finding: the stop was the problem

Rather than tune one parameter at a time, I swept the whole grid — and added the
counterfactual that actually mattered.

**Question:** for every trade that hit its stop, what would it have made if held?

```
STOP COUNTERFACTUAL — 63 trades breached −1× credit
  realising the stop:      Rs −532,981
  holding to target/time:  Rs −375,457
  recovered after breach:  69.8%
```

**Nearly 70% of stopped trades recovered.**

### Why this makes sense

On a **defined-risk** position, the long wing already caps the loss. A stop
doesn't reduce risk — it converts a temporary drawdown into a realised loss.

That's different from naked selling, where a stop is the only thing standing
between you and an unbounded loss. Historically all my own losses came from naked
positions with no alerts. That experience made a stop feel mandatory. **On a
hedged position, it isn't.**

### The sweep

| Target | Stop | Win % | Total |
|---|---|---|---|
| 35% | none | 80.0% | **+₹36,653** |
| 65% | none | 71.8% | +₹33,835 |
| 50% | none | 73.9% | +₹26,320 |
| 35% | 1.5× | 76.4% | +₹2,270 |
| 50% | 1.0× | 60.9% | **−₹154,783** |

Removing the stop moved −₹154,783 → +₹36,653.

---

## Part 13 — But is it real? (the part most people skip)

+₹36,653 over 205 trades = ₹179 per trade. Time to test whether that's an edge or
noise.

### Bootstrap on synthetic data first

Before trusting the test, I checked it against known distributions:

| True edge | Sample of 205 gave | 95% CI |
|---|---|---|
| ₹0 | −₹138 | [−729, +468] |
| ₹179 | −₹390 | [−1050, +283] |
| ₹1,500 | +₹743 | [+58, +1460] |

**Look at the last row.** Even with a *genuine* ₹1,500 edge, a 205-trade sample
barely clears zero.

> **This backtest can only detect large edges.** Distinguishing ₹179 from zero
> would need roughly **6,000 trades**. I have 205 — and 25 stocks trading monthly
> for five years is the ceiling. That's a structural limit of the design, not a
> coding flaw.

### The real result

| IV gate | Slippage | Trades | Per trade | 95% CI |
|---|---|---|---|---|
| 50 | 0.5% | 206 | ₹731 | [−231, +1575] |
| 50 | 2.0% | 205 | ₹251 | [−735, +1149] |
| 50 | 3.5% | 205 | −₹89 | [−1138, +851] |
| 70 | 0.5% | 128 | ₹1,062 | [−200, +2094] |
| 70 | 2.0% | 127 | ₹666 | [−666, +1802] |
| 70 | 3.5% | 127 | ₹496 | [−855, +1665] |
| 80 | 2.0% | 89 | ₹648 | [−1123, +2101] |

**Zero of twelve configurations had a confidence interval clearing zero.**

I also reported this deliberately: evaluating 12 combinations and picking the best
is itself a bias — the maximum of 12 noisy estimates is inflated even when no edge
exists.

### Three patterns that are structurally coherent

**1. Slippage is decisive and monotonic.** ₹731 → ₹571 → ₹251 → −₹89 as slippage
goes 0.5% → 3.5%. Break-even sits near **3%**.

**2. The IV gate does real work.** Tightening 50 → 70 lifts per-trade from ₹251
to ₹666. More importantly, gates 70 and 80 stay *positive even at 3.5% slippage*
where gate 50 goes negative. Robust to pessimistic assumptions rather than
needing optimistic ones.

**3. FY24 lost ₹60,790** against +₹112,000 across the other four years. That's
the strong trending rally of Apr 2023 – Mar 2024. Iron condors get run over by
sustained directional moves — the textbook failure mode, appearing exactly where
theory predicts.

And the temptation was obvious: *add a trend filter to fix FY24*. That would be
fitting to a single year. **I didn't.**

---

## Part 14 — Where the answer actually lives

The backtest reached its limit. It cannot resolve a ₹500–1,000 per-trade edge
with 127 trades, and further parameter tuning would only overfit.

The binding unknown is **slippage** — and it is *unanswerable from historical
bhavcopy, because settlement files contain no bid-ask.*

Every fill in the backtest assumed the midpoint on four legs. In reality:
- Short strikes showed ~5-6% bid-ask spreads
- **The far-OTM wings showed ~13%**

The legs you buy for protection are where most of your execution cost lives.

### Paper trading vs live micro-trial

I chose a **live 1-lot trial** over paper trading. Virtual fills don't reflect
order-book queue priority — the exact thing being measured.

**Config under test:**

| Parameter | Value |
|---|---|
| Size | 1 lot, max 2 concurrent |
| Entry | IV percentile ≥ 70, 0.15 delta shorts, defined wings |
| Target | 35% of credit |
| Stop | **None** (wings cap risk) |
| Exit | Hard at 21 DTE |

**The cost of this measurement:** ~₹40,000–60,000 maximum loss if both positions
go fully against me. That's a research expense, not an income attempt.

**What the trial measures:** *not* whether the strategy works. Two months is
4–8 trades — pure noise on P&L. It measures **slippage per leg as a fraction of
mid**. That single number decides everything.

Which is why the journal tool logs **bid and ask at order time** as mandatory
fields. Without the mid there's no benchmark and the trial produces nothing.

---

## Part 15 — What I'd want a friend to take from this

**1. Check whether you need the complex thing.**
₹1cr → ₹5cr in 15 years needs 11.3% CAGR. An index fund does that. I nearly built
a machine to reach a target a boring one already reaches.

**2. Separate incompatible goals into separate money.**
Generational holding and premium income fight each other. Two buckets, never
touching.

**3. Do the break-even arithmetic before anything else.**
A 40% target with a full-width loss needs 85% wins. Knowing that reshapes the
entire design before you write code.

**4. The stop sets your required accuracy — not the entry.**

**5. On defined-risk positions, a stop may be actively harmful.**
70% of my stopped trades recovered. The wing is the risk control.

**6. The dangerous bugs produce plausible numbers.**
A silent default of `lot_size = 1` looked like three bad years, not a bug. Make
defaults fail loudly.

**7. Test your test.**
Running the bootstrap on synthetic data with a *known* edge revealed that my
sample size couldn't detect anything under ~₹1,000/trade. That reframed what the
whole project could ever prove.

**8. Fetch ground truth instead of inferring it.**
I burned 40 minutes building a corporate-action detector for facts that are
published.

**9. Know what your data physically cannot contain.**
No bid-ask in bhavcopy → slippage is unknowable historically → only live fills
answer it.

**10. A null result is a real result.**
"No measurable edge, and it's decided by execution cost" is far more useful than
a backtest that flatters itself. I now know exactly what to measure next.

---

## Appendix — Build sequence

```
1.  download_bhavcopy.py    NSE archive → parquet (both formats)
2.  rank_universe.py        liquidity at 5-8% OTM, not ATM
3.  stability_by_year.py    per-FY (liquidity moved 30×)
4.  build_universe.py       the 25 names
5.  compute_iv.py           2.3M IV points, 96% solved
6.  build_iv30.py           constant-30d IV + rank/percentile
7.  detect_corp_actions.py  strike-grid reissue detector
8.  build_lot_sizes.py      reconstructed from turnover
9.  build_chains.py         per-symbol chains
10. backtest.py             iron condor engine
11. sweep.py                exit grid + stop counterfactual
12. analyse.py              bootstrap CI, slippage, IV gate
13. journal.py              live trial logger
```

**Stack:** Python, pandas, numpy, scipy, pyarrow on an Azure Ubuntu VM.
**Data:** free, public, no broker API required for history.
**Time:** one day.

---

*The system's job was never to make me money. It was to tell me the truth about
whether an idea works. It did — and the truth was "not measurably, and here is
the one number that would settle it."*
