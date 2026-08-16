# The day my own system told me "no"

*How I stopped looking for a strategy and started looking for the truth*

**Rahul Chaubey · August 2026**

---

## First, let me describe you

It's 11:30 at night. You're on YouTube.

The title says something like *"This options strategy has 90% win rate."* You click.
The man on screen is confident. He shows a chart. He shows a payoff diagram. Green
everywhere. He says he has been doing this for years.

And something inside you goes: **yes. This is it. This is what I was missing.**

You write it down. Maybe you make a note in your phone. You feel good. Tomorrow you
will start.

Next day you take the trade. Maybe it works. You feel like a genius.

Then in the second month, or the third, one trade goes wrong. Then another. You are
down ₹40,000. Your stomach feels tight. You start checking the phone every ten
minutes.

And then you stop. You just... stop. The strategy goes into the same folder as the
last five strategies.

Two weeks later you are back on YouTube at 11:30 at night.

**I am not describing you to insult you. I am describing myself.** I have done this.
Many times.

---

## Why does this keep happening?

For a long time I thought the problem was that I kept picking bad strategies.

It isn't.

Some of those strategies were genuinely good. They worked for the person teaching
them. The problem was something else, and it took me years to see it:

> **I only got the answer. I never got the reasons.**

Think about it. That man on YouTube — he probably lost money for three years before
he found that setup. He has seen it fail fifty times. He knows what a normal bad
month looks like for that strategy. He knows when to hold and when to run.

You got none of that. You got the last slide of his ten-year journey.

So when you are down ₹40,000, you have no way to answer the only question that
matters: **is this normal, or is this the end?**

He knows. You don't. So you stop.

That is not weak. That is sensible. You *should* stop when you don't know what is
happening. The mistake was starting without knowing.

---

## So I stopped asking for strategies

I decided to do something different.

Not "find a strategy." Instead: **build a system that tells me the truth about a
strategy — before I risk one rupee.**

Including when the truth is bad news.

That is what this document is about. One day of work. Five years of NSE data. And an
answer I did not want.

Let me tell you the whole thing honestly, including the mistakes. Especially the
mistakes.

---

## Part 1 — It started with a simple question

I wanted to understand **covered calls**.

Simple idea. You own shares of a company. You sell a call option on them. Somebody
pays you money today. If the stock stays below your strike price, you keep that
money. Do it every month, earn extra income on shares you were going to hold anyway.

Sounds beautiful. Free money on top of your investments.

Then I said out loud what I actually wanted:

> *"I want to buy good companies, hold them forever for my children, and earn some
> premium on top."*

Both halves sound sensible. Read them again together.

They are fighting each other.

---

## Part 2 — The thing nobody tells you about covered calls

Let's use real numbers.

You own Reliance at 1500. You sell the 1550 call and collect ₹25.

Now Reliance runs to 1700.

Your shares get taken away at 1550. You keep the ₹25.

Look at your account. You made money! ₹75 profit. Statement looks green.

**But where are your shares?**

Gone. And if you want them back, you now pay 1700 for what you sold at 1550.

You have the same rupees. You have fewer shares.

### Why this matters so much

Think about why you bought that stock for your children in the first place. You
bought it for the **big move**. The 5x over fifteen years.

Covered calls sell exactly that away. Every single time the stock has a strong
month, you get taken out. Every time you come back, you pay more.

You collect ₹25 forty times. And you miss the one move that was worth ₹800.

### And then tax comes

In India it gets worse.

Every time your shares get called away, that is a **sale**. Tax event. And your
holding period **resets to zero**.

- Hold a share quietly for 15 years → you pay tax **once**, at the end.
- Sell and rebuy it 40 times → you pay tax **40 times**.

That difference compounds against you for fifteen years.

**Lesson one, and it has nothing to do with options:**

> Earning income and building long-term wealth pull in opposite directions.
> No clever adjustment fixes this. You have to keep the two pots of money separate.

---

## Part 3 — Then I asked myself an uncomfortable question

Before designing anything, I sat down and did one small calculation.

My actual goal: turn ₹1 crore into ₹5 crore for my children.

How much return do I need?

| Time I have | Return needed per year |
|---|---|
| 10 years | 17.5% |
| 15 years | 11.3% |
| 18 years | 9.4% |

I have about fifteen years.

**11.3% per year.**

Now here is the uncomfortable part. That is roughly what a plain Nifty index fund
has given historically. No options. No screens. No 2 PM entry. Nothing.

I was about to build a complicated machine to reach a place a boring one already
reaches.

I want you to actually stop and do this calculation for yourself before reading
further. Most people never do it. They assume they need something clever, and then
they go looking for clever.

### What I did instead

I split the money into two buckets that never touch:

**Bucket 1 — about 70%. For my children.**
Index funds and good companies. I will never sell a call on this. Not once. Its job
is to catch the big moves.

**Bucket 2 — about 30%. For income.**
This is where options run. On positions I am completely fine losing.

That separation *is* the risk management. Everything else is detail.

---

## Part 4 — The number that changed everything

Now I could ask the real question. Can I sell options systematically and make money?

Before writing any code, I did one piece of school arithmetic. It reshaped
everything.

Take an iron condor. Don't worry about the name — it just means you sell options on
both sides, above and below the price, and you buy cheaper options further out to
protect yourself.

Say the structure is 50 points wide and you collect 15.

- If everything goes well, you make 15.
- If it goes badly, you lose 35.

Now, most people book profit at 40%. So on a good trade you take home 6.

**How often do you need to be right just to break even?**

```
6 × (win rate) = 35 × (loss rate)
```

Answer: **85%**.

Eighty-five percent. Just to be flat. Before brokerage. Before taxes.

I sat with that for a while.

Then I changed one thing. Instead of letting the loss run to 35, cut it at 15 — the
same amount I collected.

**Now I need 71%.**

Change the profit booking from 40% to 50% as well:

**67%.**

### Read this twice

> **Your stop loss decides how accurate you need to be. Not your entry.**

Everybody spends their life on entry. Which indicator. Which candle. Which level.

The exit rule was quietly deciding whether the whole thing could ever work.

And I had never calculated this for any strategy I had ever traded. Not once.

---

## Part 5 — So I decided: no trading until I build

This is the decision I actually want you to take from this document.

I told myself: **I will not place a single trade until the system is built and
tested.**

Every number must come from data. Not from a video. Not from a friend. And
definitely not from *मेरा मन कर रहा है*.

I named it **Pramana** — प्रमाण.

In our own philosophy, pramana means *valid knowledge*. Proof. The thing that makes
a belief trustworthy instead of just a feeling.

That is exactly what I was missing all those years at 11:30 at night.

---

## Part 6 — Where the data comes from (this part is free)

People assume you need expensive data. You don't.

NSE publishes a file every single day called the **bhavcopy**. Every option, every
strike, every expiry, price, volume, open interest. Free. Public. No login.

I downloaded five years of it. April 2021 to March 2026. About 1,233 trading days.

Why only five years? Because before that, the rules were different. Stock options
used to be cash-settled. Now they are physically settled — you actually deliver
shares. Mixing old rules with new rules would give me a beautiful backtest of a
market that no longer exists.

**Small thing, big lesson:** older data is not always better data.

---

## Part 7 — My first trap (and it's a good one)

I opened the file and found something strange.

Every option has two prices — `close` and `settle`. I assumed they were the same
thing.

Look at this real row:

```
AARTIIND  880 CE     close 285.50     settle 451.45     contracts traded: 0
```

Same option. Same day. Two prices. 58% apart.

Why? **Because nobody traded it that day.** So `close` is an old stale price from
some previous day, and `settle` is NSE's calculated theoretical value.

Neither one is a price you could have got.

If I had used those prices, my backtest would have shown beautiful profits on trades
that were never possible in real life.

**So I made a rule:** if nobody traded that option that day, throw the row away.

> If nobody traded it, I couldn't have traded it either.

Fewer rows. But every remaining row is real.

---

## Part 8 — Choosing which stocks to trade (I got this wrong many times)

There are around 200 stocks with options. I wanted 25.

### Mistake 1: I measured volume in the wrong place

My first idea: pick the stocks with the highest option volume. Obvious, right?

Wrong.

**At-the-money options are liquid in almost every stock.** But I don't sell
at-the-money. I sell 5-8% away from the price.

And out there? Many stocks are dead. Wide spreads. No buyers.

So I had to measure liquidity **at the strikes I would actually use** — not where
the crowd is.

### Mistake 2: I used five-year averages

Look at what happened to these stocks:

| Stock | Volume in FY22 | Volume in FY26 |
|---|---|---|
| DIXON | ₹35 crore/day | ₹1,070 crore/day |
| MCX | ₹24 crore | ₹994 crore |
| HAL | ₹23 crore | ₹831 crore |
| BEL | ₹66 crore | ₹780 crore |

Thirty times bigger. Not 30% — **30 times**.

Taking a five-year average means averaging a market that is dead with a market that
is alive. Meaningless.

**Fix:** rank on the most recent year only. I'm going to trade tomorrow, not in 2021.

### Mistake 3: I threw away good stocks

I made a rule: the stock must have data for at least 80% of the five years.

This correctly removed HDFC — which merged into HDFC Bank in 2023. Good. That
actually proved my data was reading correctly.

But it also removed BSE, JIOFIN, DIXON, CDSL. These are *very* liquid stocks today.
They were just added to F&O recently.

My filter could not tell the difference between **"this stock died"** and **"this
stock was born recently."**

**Fix:** must be liquid in the last two years. Not all five.

### Mistake 4: the Vodafone Idea trap

This one is my favourite.

IDEA showed ₹364 crore of daily option turnover. Huge. And it failed my test every
single year.

I thought my code was broken. It wasn't.

**Vodafone Idea trades around ₹8. Strikes are 50 paise apart.**

So when I say "sell 5-8% away from the price," that entire zone is about 40 paise
wide. Often *no strike exists there at all.*

> A stock can have enormous volume and still be completely untradeable for your
> particular strategy.

Volume is not liquidity. Where the volume sits matters more than how much there is.

---

## Part 9 — Measuring fear correctly

To sell options you need to know one thing: **is fear high right now, or low?**

That is what IV — implied volatility — means. It is simply the market's guess about
how much a stock will move. High IV means fat premiums. Low IV means thin ones.

And here is why we sell when it's high: **on average, the market is more scared than
it needs to be.** That gap is the whole business. But it only opens up when fear is
already elevated. Selling in a quiet market means you take the same risk for peanuts.

### But the obvious way to measure it is wrong

Here is the trap. As expiry gets closer, IV goes **up automatically**. Not because
anything scary happened. Just because of how the maths works.

So if I measured IV the simple way, my "fear meter" would go up and down with the
calendar, not with actual fear.

I would be entering trades because it was the 20th of the month, while believing I
was reading the market.

**Fix:** always measure IV at a fixed distance — exactly 30 days out — no matter
where we are in the expiry cycle. Now the number only moves when real fear moves.

### And then a second trap

I use something called **IV Rank**. It compares today's fear to the last one year.
0 means calmest all year, 100 means most fearful.

Then I looked at Adani Enterprises:

| Stock | Days when IV Rank was above 50 |
|---|---|
| ADANIENT | **6.3%** |
| DIXON | 24.3% |
| HDFCBANK | 20.5% |

Six percent? For a volatile stock like Adani?

Here's why. During the Hindenburg crash, Adani's IV touched **231%**. Insane
number.

IV Rank works like this: *where is today, between the lowest and highest of the
year?*

That one crazy day became "the highest." So for the next full year, **every normal
day looked calm by comparison.** The measurement was dead for that stock. And the
summary table looked perfectly healthy — nothing shouted at me.

**Fix:** use **IV Percentile** instead — simply *what percentage of days in the last
year were calmer than today?* One crazy day cannot break it.

---

## Part 10 — The mistake I want to confess

This one is embarrassing, and it is the most useful part of this document.

I found that Reliance showed a minimum IV of 3.4%. Impossible. Reliance is never
that quiet.

I looked at the dates: 11 to 19 July 2023.

That is the **Jio Financial demerger**.

When a company splits, or gives a bonus, or demerges, NSE **changes all the option
contracts**. Strike prices get adjusted. And in my data, everything around those
days becomes garbage.

So I decided to write code that automatically finds these events.

**Attempt 1:** Find days when the price jumped more than 15%.

It found 8 real splits. It also flagged 4 June 2024 — **election results day** —
across three different stocks.

My code was about to delete the single most important high-volatility day in five
years. The exact kind of day I most need to learn from.

**Attempt 2:** I added smarter checks. Better. But it *still* missed Reliance,
because the demerger only moved the price 9% — under my 15% threshold.

**Attempt 3:** I finally realised something. When a stock splits, NSE **reissues
every strike price**. Kotak's ₹2000 strike becomes ₹400. The old strikes simply
vanish.

So instead of watching the price, watch the **strike list**. If yesterday's strikes
and today's strikes have nothing in common, something happened.

This worked instantly and perfectly. It even caught 25 events the price method could
*never* find — like Vedanta, nine separate times, because NSE also adjusts contracts
when a company pays a very large dividend. The price barely moves. The strikes all
change.

### Now the confession

**I spent forty minutes writing three versions of this detector.**

Corporate actions are **published information**. NSE announces them. They are on the
internet. I could have simply looked them up in two minutes.

Why didn't I? Because I was comfortable inside my own data and I didn't want to
leave it. I kept trying to be clever instead of just checking.

> When you find yourself trying the fifth version of something — stop.
> Go and find the real answer instead of trying to guess it.

I do this in life too. Maybe you do as well.

---

## Part 11 — The bug that nearly fooled me completely

I ran the backtest. Here are the results, year by year:

| Year | Win rate |
|---|---|
| FY22 | **0.0%** |
| FY23 | **0.0%** |
| FY24 | **0.0%** |
| FY25 | 31.1% |
| FY26 | 63.5% |

Zero percent. Three years in a row. Then suddenly 63%.

No strategy on earth behaves like that.

### What had happened

The bhavcopy only started publishing **lot size** from mid-2024. Before that, the
column is empty.

My code, when it found nothing, quietly used **1**.

So every trade before 2024 was calculated as if I had bought **one share** instead
of one lot of 500 shares.

Profits divided by 500. Brokerage unchanged. Every old trade became a guaranteed
small loss.

### Why this is the scariest part

**The bug did not crash. It gave me a normal-looking table.**

I caught it only because 0% is obviously impossible.

If the error had been 15% instead of 100%, I would never have noticed. I would have
traded real money on a broken number.

> The dangerous mistakes don't announce themselves. They look reasonable.
>
> Every "default value" in your system is a silent assumption. Make it shout at
> you instead of whispering.

I fixed it using simple maths that was already in the data — turnover divided by
(contracts × price) gives you the lot size. Then I checked it against my Zerodha
screen. It matched.

---

## Part 12 — The system gave its answer

Everything fixed. Everything tested. I ran it properly.

**Result: it loses money.**

```
207 trades
Win rate: 60.9%
Needed to break even: 67.8%
Net: −₹1,54,783
```

I was winning 6 out of 10 trades and still losing money.

Exactly what Part 4 predicted. My wins were small and my losses were big, so 61%
was simply not enough.

The arithmetic warned me before I wrote a single line of code. Now the data was
saying the same thing in a louder voice.

---

## Part 13 — And then the system taught me something

Instead of changing settings one by one, I asked one clean question:

> **Every trade where I hit my stop loss — what would have happened if I had just
> held on?**

The answer:

```
Trades that hit the stop:              63
How many recovered afterwards:      69.8%
```

**Seven out of ten stopped trades would have been fine.**

I removed the stop loss completely and reran everything:

```
With stop loss:      −₹1,54,783
Without stop loss:      +₹36,653
```

### Why this happens (and why it is not reckless)

I need to be careful here, because "remove your stop loss" is terrible advice in
most situations. Please read this part properly.

In this structure I am **already protected**. I buy a further option that caps my
maximum loss. Whatever happens overnight — war, budget, anything — my loss cannot
exceed a known number.

When protection is already built in, a stop loss doesn't reduce risk. It just turns
a temporary loss into a permanent one.

My instinct said the opposite. Because every serious loss in my trading life came
from **naked** positions with no protection. In that world, a stop is the only thing
standing between you and disaster.

I had carried a correct lesson from one situation into a situation where it was
wrong.

> This is the whole value of testing. It told me my instinct was backwards.
> No YouTube video was ever going to do that for me.

---

## Part 14 — But I did not celebrate

+₹36,653 over 205 trades. About **₹179 per trade**.

Before believing that number, I did something I now think everybody should do.

**I tested my test.**

I made fake data where I already knew the answer, and checked whether my analysis
could find it.

| Real edge I put in | What my test reported |
|---|---|
| ₹0 | −₹138 |
| ₹179 | −₹390 |
| ₹1,500 | +₹743 |

Look at the last row carefully.

Even when I *deliberately put in* a strong edge of ₹1,500 per trade, my test —
with 205 trades — could barely see it.

**Which means my test simply cannot tell the difference between ₹179 and zero.**

To prove something that small I would need around **6,000 trades**. I have 205. And
25 stocks trading once a month for five years is the maximum this approach can ever
give.

That is not a bug in my code. That is a hard limit of what I can ever know from
history.

### The honest result

I tested 12 different settings. **Not one of them could be proven to make money.**

```
0 out of 12 settings showed a result I can trust.
```

---

## Part 15 — But three things were clearly true

The system couldn't prove profit. But it showed me three things that make complete
sense:

**1. Everything depends on execution cost.**

| Cost per leg | Profit per trade |
|---|---|
| 0.5% | ₹731 |
| 2.0% | ₹251 |
| 3.5% | −₹89 |

Break-even is around 3%. **The whole business lives or dies on how well you get
filled.** Not on the strategy.

**2. Being patient works.**

When I only traded when fear was in the top 30% of the year, results improved — and
stayed positive even assuming bad fills. Fewer trades. Better trades.

**3. FY24 was terrible — and that makes sense.**

FY24 lost ₹60,790 while the other four years together made ₹1,12,000.

FY24 was the big trending rally. This strategy makes money when the market goes
sideways and gets destroyed when it trends hard.

Now — I could add a rule to avoid trending markets, and my backtest would suddenly
look beautiful.

**I didn't.** Because I would only be adding that rule *after seeing* FY24. That is
not learning. That is decorating history so it flatters me.

> If you keep adjusting your rules until the past looks good, you have not built a
> strategy. You have built a very expensive story.

---

## Part 16 — The one thing my data could never tell me

Here is where it gets interesting.

Everything came down to **slippage** — the gap between the price you see and the
price you actually get.

And bhavcopy **cannot** tell me this. It has no bid-ask. It only records what
happened, not what was on the screen.

My backtest assumed I always got the perfect middle price on all four legs. Nobody
gets that.

Real spreads I found:
- On the options I sell: about 5-6% wide
- **On the far options I buy for protection: about 13% wide**

The protection is where the money quietly leaks out.

### So I am going live. Small.

Not paper trading. Paper trading gives you imaginary fills — and imaginary fills are
exactly the thing I need to measure.

**1 lot. Maximum 2 positions at a time.**

Worst case if everything goes wrong: about ₹40,000 to ₹60,000.

I want to be very clear about what that money is. **It is not an attempt to earn.
It is the fee for finding out the truth.**

And I am not measuring profit. Two months gives maybe 6 trades — that tells me
nothing about profit.

I am measuring **one number**: how much I actually lose on each fill.

Under 3% → there is something here.
Over 3% → there isn't, and I walk away.

Either answer is worth ₹50,000 to me. Because right now I don't know, and not
knowing is what makes people quit at 11:30 at night.

---

## What I actually want you to take from this

Not my settings. Please don't copy my settings.

**1. Do the small calculation first.**
₹1 crore to ₹5 crore in fifteen years needs 11.3% a year. Find out what you actually
need before deciding you need something clever.

**2. Keep your long-term money and your trading money completely separate.**
Different accounts. They fight each other otherwise.

**3. Calculate your break-even win rate before you trade anything.**
Take any strategy you're using right now. Work out how often you must be right just
to be flat. Most people have never done this and are shocked by the answer.

**4. Your exit rule decides your fate, not your entry.**
We all spend our time on entries. It's the wrong place to spend it.

**5. Be careful about carrying lessons from one situation to another.**
My stop loss habit was correct for naked positions and wrong for protected ones.

**6. The dangerous mistakes look normal.**
A missing lot size showed up as three bad years, not an error message.

**7. Test whether your test can even work.**
I found out mine could never detect a small edge. That changed what I could
honestly claim.

**8. When you're stuck, go find the real answer.**
Don't build a clever guess. I wasted forty minutes on this.

**9. Know what your data can never tell you.**
Mine could never tell me about slippage. So I stopped pretending and went to find
out live.

**10. "No" is a real answer.**

That last one matters most.

My system told me no. And I am genuinely glad — because the other option was to
find out with ₹10 lakh instead of ₹50,000, three years from now, with much less
patience left.

---

## Please don't copy this

I am not giving you a strategy. I don't have one to give.

What I want is for you to build your own version of this. Your own questions. Your
own data. Your own uncomfortable answers.

Because the next time you're down ₹40,000 — and you will be — the only thing that
will keep you steady is knowing *why* you believed in the first place.

That is the difference between trading and hoping.

You will never get that from a video. Not even this one.

---

### The tools (all free)

Free NSE data · Python · one ordinary computer · one day

Nothing here needed money. It needed the willingness to be told "no."
