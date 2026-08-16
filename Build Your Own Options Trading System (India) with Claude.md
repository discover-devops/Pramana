# Prompt Chain: Build Your Own Options Trading System (India) with Claude

**How to use this:** 

Copy Prompt 1 into a new Claude chat. Answer what it asks. Then paste Prompt 

2 into the *same conversation* (don't start a new chat — Claude needs the earlier context). 

Keep going through Prompt 6. Each prompt builds on the last, so skipping ahead will give you a weaker, more generic system.

---

## Prompt 1 — Trader Profile & Objective

```
I want to build a personalized options trading system for the Indian market (NSE F&O) with your help, step by step. Before you suggest anything, I'll answer a series of questions across several messages so the system you design actually fits me.

For now, just ask me the following and wait for my answers — don't design anything yet:

1. Trading capital (approx. amount you're allocating specifically to options)
2. Experience level with options trading (beginner / intermediate / advanced) and how long you've been trading
3. Primary objective: regular income, capital growth, hedging an existing portfolio, or learning/skill-building
4. Time you can actively dedicate: full-time, part-time during market hours, or only pre/post market
5. Risk appetite: conservative, moderate, or aggressive — and how you'd react to a 10% drawdown in a month

Once I answer, summarize my profile back to me in 3-4 lines and confirm before moving on.
```

---

## Prompt 2 — Strategy Preference & Market Approach

```
Good. Now, based on my profile above, ask me about my strategy leanings:

1. Preferred underlyings: index options (Nifty/BankNifty/FinNifty) vs stock options vs both
2. Directional stance: do I prefer directional bets, or non-directional/theta strategies (credit spreads, iron condors, straddles/strangles, calendars)?
3. Trading style: intraday, positional (few days), or swing (weeks, held across expiries)
4. Technical or fundamental leaning: do I trade off charts/indicators, options chain data (OI, PCR, max pain), news/events, or a mix?
5. Do I currently follow any specific strategy already, even informally? If yes, describe it.

Don't suggest a strategy yet — just gather this and summarize it back to me.
```

---

## Prompt 3 — Historical Data & Track Record (if any)

```
Now let's ground this in real data if I have it.

1. Do I have a trading journal, broker statement, or past trade log I can share (as text, screenshot, or file)?
2. If yes — approximate win rate, average win vs average loss, and largest drawdown I've experienced
3. If I'm new and have no track record — do I have any backtested data or a strategy I've read about that I want to validate?
4. Any specific months/events (e.g. Budget day, election results, RBI policy) where my approach did unusually well or badly?

If I have no historical data at all, that's fine — note that we'll design conservatively and build in a paper-trading phase before going live. Summarize what you now know.
```

---

## Prompt 4 — Risk Management Rules

```
This is the most important part — ask me about risk management specifically, since this is where most options traders in India blow up their accounts:

1. Maximum capital I'm willing to risk on a single trade (% of total capital)
2. Maximum number of open positions at once
3. Do I want a hard stop-loss rule (e.g. exit at 30-50% of premium) or a mental/manual stop?
4. Maximum acceptable drawdown before I pause trading and reassess (e.g. -10% of capital in a month)
5. How do I currently size positions — fixed lots, % of capital, or something else?
6. Am I trading with margin/leverage, and if so, how much?

Summarize these as explicit numeric rules, not vague statements — I want concrete numbers I can hold myself to.
```

---

## Prompt 5 — Practical & Regulatory Constraints

```
Now the practical layer — ask me about:

1. My broker/platform (Zerodha, Upstox, Angel One, Fyers, etc.) and whether I have API/algo access or trade manually via the app
2. Whether I want a fully manual system, a semi-automated one (alerts + manual execution), or fully automated (algo)
3. Any liquidity constraints — am I okay only trading liquid strikes near ATM, or do I need to also trade illiquid far strikes?
4. Tax treatment awareness — do I understand F&O is treated as non-speculative business income under Indian tax law, and am I accounting for STT, brokerage, and other charges in my P&L expectations?
5. Any hard rules I must follow — e.g. no overnight positions, no trading in the last 15 minutes, no trading on expiry day, etc.

Summarize my constraints, then tell me you now have everything needed to design my system.
```

---

## Prompt 6 — Build the System

```
Based on everything I've shared across the last five messages (profile, strategy preference, historical data, risk rules, and practical constraints), now design my complete options trading system. Include:

1. **Strategy definition** — the specific strategy/strategies suited to my profile, with entry conditions
2. **Entry rules** — what triggers a trade (technical, options-chain-based, or event-based, as relevant to me)
3. **Exit rules** — profit target, stop-loss, and time-based exits
4. **Position sizing formula** — tied to the risk numbers I gave you
5. **Risk management checklist** — daily/weekly rules to prevent overtrading or revenge trading
6. **A paper-trading or small-capital validation plan** before scaling up, if I don't already have a proven track record
7. **A simple trade log template** (columns I should track) so I can review performance over time

Present it as a structured playbook I can follow, not generic options theory. Flag any part of my inputs that seems inconsistent or high-risk (e.g. risk per trade too high for my drawdown tolerance) before finalizing.
```

---

### Notes for you (not part of the shareable prompts)

- **This produces a personalized *framework*, not investment advice** — worth telling your friends that upfront, since Claude will design a system based on their inputs but isn't a SEBI-registered advisor and won't recommend they buy/sell specific instruments.
- If someone wants to skip the back-and-forth, they can technically paste all 6 prompts' answers at once in a single message — the chain is for people who want it to feel guided, but it's not mandatory to do it one at a time.
- Consider adding a 7th prompt later for "review my last 20 trades against this system and tell me where I deviated" — that's a strong retention/follow-up hook once people have used the system for a few weeks.
- If any of your friends are complete beginners to options (not just to systems), they may need a primer *before* Prompt 1 — otherwise Prompt 2's strategy question will confuse them.
