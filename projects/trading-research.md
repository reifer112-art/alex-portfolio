# Trading Research Lab

**Paper only research that tests trading ideas honestly against buy and hold.**

Source code is private.

## The problem
Short videos and online posts pitch trading strategies, usually without rules you can test. I wanted a fair way to check them.

## What I built
- A Python backtesting setup that compares each idea with simply holding the S&P 500 ETF (SPY), after costs.
- A paper trading simulator with pretend money and a loss limit that shuts it off.
- Research notes on prediction market ideas tested on real recorded trades.
- A rule, enforced by an automated test, that no brokerage code is added.

## What I found
In my tests, a 200 day trend rule, 12 month momentum, and a short term dip rule did not clearly beat holding SPY. The trend rules mostly reduced drawdowns. A sector momentum plus volatility target mix gave a smoother ride, not more money. Results depend on the period tested.

## What works today
Backtests and paper trading run from the command line. A test run on October 22, 2026 passed 347 tests and failed 2, both in a prediction market sign in check that I am fixing. This repository has no brokerage code and places no orders. This is research code, not production ready, and it is not fully tested.

## At a glance
- **My contribution:** chose the ideas to test, set the buy and hold yardstick and the paper only rule, and reviewed every result. The code was written with AI coding tools.
- **Key decision:** compare every idea with buy and hold after costs, and write down the failures.
- **Verified example:** an automated test fails if brokerage code is added to this repository.
- **Scope:** this page covers only this research repository.

## What I learned
A rejected idea is a useful result. I keep failed ideas written down so the good ones are not cherry picked. I do not present backtests as proof of future profit.
