# Alex Reifer — Projects

Five things I've built, explained plainly — what each one does, what data
it runs on, and what the actual numbers are. Private for now.

---

## omniroute-test — automated futures trading system

**What it is:** a program that watches the market and automatically buys and
sells small S&P 500 / Nasdaq futures contracts (MES/MNQ) on a funded trading
account, following one fixed set of rules — no gut decisions, no
overriding it mid-trade.

**What data it uses:** to build and test it, I used 6 months of real,
minute-by-minute historical price data (about 190,000 individual price bars,
from a market-data provider called Databento). Once running for real, it
also connects to the broker's live price feed so it always knows the
current price before it acts.

**The result, in plain terms:** before ever risking real money, I tested
the rules against those 6 months of real price history — the *first* 70%
of it to tune the rules, then the *remaining, untouched* 30% to check it
still worked on data it hadn't seen (this second check is the important
one — it's the difference between "looks good because I tuned it to look
good" and "actually holds up"). Risking $500 per trade on a simulated
$50,000 account: it made about **$4,946 on the tuning data and $2,106 on
the untouched data** — roughly **14% over the ~8 months tested**, and it
held up on both halves instead of just the one it was fitted to.

**Important honesty:** that's a backtest, not real trading — no real order
has been placed with this system yet. Past performance on historical data
doesn't guarantee anything about the future; it just means the idea
survived a fair test instead of just looking good on paper.

---

## overnight-pressure — systematic stock strategy research

**What it is:** research into *which* stocks to hold overnight, based on
the finding that almost all of the stock market's long-term gain happens
between the close and the next day's open — not during the trading day
itself.

**What data it uses:** 10 years (2016–2026) of daily price data across the
500 largest US stocks.

**The result, in plain terms:** picking stocks with this method and
holding them overnight beat just holding the market by about **8.9% per
year** (measured with real statistics, not a lucky-looking chart — the
signal held up with high statistical confidence across 500 stocks over 10
years). Adding a second, unrelated strategy — buying stocks right after a
strong earnings report — pushed the combined result to about **15% per
year** in testing. Being honest about that second number: roughly half of
the earnings strategy's extra return is just "the market was going up,"
not real skill, so a realistic estimate is closer to **14–15%/year**, not
the full 15.1% headline. It's running with a small amount of real money
now ($100 growing toward $400), with the same "don't lie to yourself"
principle: every idea that got tested and *didn't* work is kept in the
project's notes, not deleted.

---

## daytrader-bot — early trading bot (Alpaca paper account)

**What it is:** the first version of an automated trading bot — a simple
moving-average strategy with position sizing and a stop-loss, all running
against a paper (fake-money) Alpaca brokerage account.

**What data it uses:** live paper-account price data from Alpaca — nothing
historical here, this one was about proving the mechanics work, not
optimizing returns.

This is the project that led directly to the risk-management approach used
in `omniroute-test` above.

---

## life-planner — full life-planning web app

**What it is:** a web app for planning and tracking day-to-day life —
built to support multiple separate users, each with their own private
data.

**What data it uses:** a real hosted database (Supabase/Postgres) storing
each user's own planning data, kept isolated per account.

*Stack: Next.js, TypeScript, Supabase.*

---

## recomp-planner-legacy — body recomposition tracker

**What it is:** a single self-contained tool for planning and tracking
body-recomposition progress (weight/strength goals over time).

**What data it uses:** connects to Google Calendar to schedule check-ins
and reminders directly on your calendar.

*Stack: HTML/CSS/JS, Google Calendar API.*
