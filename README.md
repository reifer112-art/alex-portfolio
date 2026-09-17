# Alex Reifer — Projects

Five things I've built, newest first. Private for now — happy to walk through
any of these live.

---

## omniroute-test — real-time futures execution engine

A risk-gated order-execution system for a funded futures account (MES/NQ,
TopstepX). The interesting engineering problem wasn't the strategy — it was
making sure a bad fill or a stale price *can't* place a bad order:

- **Risk engine**: hard per-trade risk cap, daily-loss lockout, consecutive-loss
  lockout, and a kill switch, all independent of whatever the strategy says.
- **Price-tolerance firewall**: every incoming signal is re-validated against
  a live quote before it can become an order; anything outside tolerance is
  rejected, logged, and never silently retried.
- **Real-time data**: found and implemented a SignalR WebSocket feed
  (a broker capability that wasn't documented anywhere obvious) after the
  REST polling approach proved too laggy for the tolerance check — persistent
  connection, automatic reconnect with backoff.
- **117 passing tests**, including end-to-end webhook → risk-engine → broker
  flows.

No live order has ever been placed by this system — every dangerous step
(flipping a broker connector live, wiring a real alert to the webhook) is
gated behind an explicit, deliberate human action, on purpose.

*Stack: Python, FastAPI, asyncio, pytest.*

---

## overnight-pressure — systematic equities research

Signal research and backtesting from first principles, on a self-imposed
$100 capital / $0 data-budget constraint. The headline result: a
cross-sectional reversal signal with real out-of-sample statistics
(IC 0.0137, t=5.11 across 500 names, 2016–2026), combined with a second,
near-uncorrelated signal (post-earnings drift) into one book.

What I'm proudest of isn't the return number — it's the discipline around
it: every tested idea that *didn't* work is kept in the record instead of
deleted, and every headline result gets an honest counter-check (e.g. a
beta-hedge test showing half the earnings sleeve's raw return was just
market beta, not signal, and saying so plainly).

*Stack: Python, pandas, walk-forward backtesting, Alpaca paper API.*

---

## daytrader-bot — Alpaca paper-trading bot

An earlier, smaller execution bot — moving-average crossover strategy,
position sizing and stop-loss as a percent of equity, daily-loss circuit
breaker. The precursor to the risk-engine design used in the two projects
above.

*Stack: Python, Alpaca paper API.*

---

## life-planner — multi-user planning app

A full-stack life-planning web app: auth, multi-user data isolation, and a
real deployed backend.

*Stack: Next.js, TypeScript, Supabase.*

---

## recomp-planner-legacy — body recomposition tracker

A self-contained single-file planning tool with Google Calendar
integration for scheduling.

*Stack: HTML/CSS/JS, Google Calendar API.*
