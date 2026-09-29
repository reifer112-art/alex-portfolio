# Alex Reifer, Projects

Eight things I've built. For each one: why I started it, what I actually
did, and what happened. Most of the actual code stays private, a couple
of these touch a real trading account or personal financial data, so
this file is the honest summary either way. `alumni-network` is the one
exception, live and public at
[github.com/reifer112-art/alumni-network](https://github.com/reifer112-art/alumni-network)
and running at [branchdin.com](https://www.branchdin.com), if you want
to see real working code instead of just a writeup.

---

## RegimeShield: portfolio risk and behavioral diversification engine

**Why I built it:** every portfolio tool I'd seen assumes an emotionless
investor who calmly holds through a 50% drawdown. Real people don't.
Morningstar's own "Mind the Gap" research puts the real world cost of
panic selling at market bottoms and re entering late at 1.0 to 1.5% of
return per year, every year, compounding. I wanted to quantify that cost
directly instead of treating diversification as just a math exercise.

**The core idea, in plain wealth management terms:** the real value of
holding an uncorrelated "ballast" sleeve in a portfolio isn't that it
mathematically lowers variance. It's that a *smaller* drawdown never
crosses the psychological point where a real investor gives up and
sells. Model it out: an unhedged portfolio might drop 48% and take 4
years to recover, fine, if you actually hold on. But if that same
investor panics and sells at 30%, they lock in the loss and the real
recovery stretches past 9 years once you account for re entering late.
Add a ballast sleeve that caps the drawdown around 28%, never
triggering the panic point at all, and the recovery drops back to about
3 years. The insurance isn't the math. It's keeping the investor in
their seat.

**On alpha versus beta, plainly:** this is deliberately a **risk** tool,
not an **alpha** tool. It doesn't try to pick winning stocks or time the
market, and I want to be direct about that distinction rather than blur
it. What it does measure carefully is **beta**, not just market beta,
but a full decomposition of every holding onto 8 real macro risk factors
(market, size, value, momentum, term premium, credit spread,
commodities, dollar strength), so a stress test against a real
historical crisis is grounded in actual factor exposure instead of a
guess. The "edge" this tool claims is entirely behavioral. It doesn't
promise better returns from security selection. It quantifies how much
return a real investor keeps by not blowing themselves up emotionally,
a genuinely different, and I think more honest, kind of value
proposition than most retail portfolio tools make.

**What I did, technically:**
1. **Effective Number of Bets** (Meucci, 2009). A naive holdings count
   overstates diversification: ten tech stocks "count" as ten positions
   while the real economic risk sits in one dimension. This decomposes
   the correlation matrix's eigenvalues and reports diversification as
   spectral entropy across them, with exact boundary cases (perfect
   collinearity gives 1.0, perfect independence gives N) locked down as
   tests.
2. **Random Matrix Theory denoising** (Laloux, Cizeau, Bouchaud and
   Potters, 1999). On any finite daily return window, part of the
   correlation signal is just sampling noise, not a real risk factor.
   Eigenvalues below the theoretical noise boundary get replaced before
   the diversification score is computed, with a bootstrapped confidence
   interval attached, so the tool never lets a decimal point difference
   get over read as a real signal.
3. **Factor mapped historical stress testing**, honest about assets that
   didn't exist yet during a given crisis. Rather than inventing 2008
   era prices for a fund launched in 2019 (a real look ahead bias trap),
   holdings are stress tested through their actual factor betas against
   long history benchmarks, with each modern ticker's real launch date
   disclosed in the output.
4. **Behavioral capitulation model**. Simulates a real investor who
   liquidates at a chosen drawdown threshold and re enters only after a
   real recovery has already started, quantifying the exact return
   penalty a smaller, well placed ballast sleeve avoids.

**What happened:** the assumptions doc names its own weak points
directly rather than hiding them in fine print (factor loadings assumed
stable even though real crises break that; diversification score is
basis dependent, addressed by always showing the confidence interval,
not a bare number). Tested against a real portfolio of about 20
positions, not a demo, and in that process, found and fixed two real
bugs: the core analysis was silently falling back to one fixed synthetic
data random walk for most real holdings while displaying a specific
looking score with no visible warning, and a separate tool was
hardcoding a 50/30/20 account split and never actually checking whether
a recommendation fit an investor's real account capacity. Both fixed and
verified against live market data pulled fresh for the actual holdings
involved.

*Stack: Python, FastAPI, HTML and Chart.js, pytest. Built with Google's
Antigravity (Gemini based), the one project here not built with Claude.*

---

## alumni-network: collegiate alumni networking app (live, public repo)

**Why I built it:** LinkedIn is built for the whole world, which makes
it useless for the connections that actually matter most to a student:
alumni from your exact major, or your specific fraternity, sorority, or
club. I wanted to turn "cold outreach to a stranger" into "warm intro to
someone who shares real, verified history with you."

**What I did, step by step:**
1. Designed the trust layer first. Students verify instantly with their
   .edu email. Alumni verify their identity via LinkedIn, but I caught
   early that LinkedIn's API doesn't actually expose education history,
   so alumni claims are self reported and shown honestly as such (a
   "self reported" badge) until a real person, a current officer of that
   org, vouches for them, at which point it upgrades to "verified."
2. Built the vouching system and a single use invite loop so a verified
   alumnus can pull in people they actually know from their own chapter.
   That's the real growth mechanism, not a cold campus wide launch.
3. Solved the two hardest engineering problems for real, not just on
   paper. A **time capped coffee chat scheduler**, where claiming an
   alumnus's last monthly slot is race safe through a database level row
   lock (two students clicking at the same millisecond can't both win),
   and an **AI warm intro drafter**, where the model only ever sees
   shared facts computed on the server from real database joins. It's
   structurally incapable of inventing a connection that isn't real.
4. Deliberately did **not** build the cash referral bounty version of
   the idea. Matching alumni to a paid corporate referral bonus through
   the app edges into employment agency licensing and money transmission
   regulations. Kept the discovery and intro value without turning the
   app into a financial intermediary.

**What happened:** it's live in production at branchdin.com, a real
domain, with real students using it: real orgs, real coffee chats booked
onto real calendars, real messages sent. The harder problem turned out
to be the one no amount of code review would have caught: getting the
first real alumnus onto a two sided network when the alumni side starts
empty. Rather than assume that away, I built directly for it, a way for
a student to send a specific alumnus a real, personal invite email, and
a claimable placeholder profile system so the directory has real names
in it before those people have signed up. This is the one project here
with the actual code public, not just this writeup, since it's the most
complete and the safest to share.

---

## second brain: a personal knowledge system (Claude Code and Obsidian)

**Why I built it:** I read everything from lecture notes to research
papers to random articles, and none of it stayed connected. A month
later I'd half remember a good idea and have no way to find where it
came from. I wanted one system that actually keeps growing smarter
instead of just piling up notes.

**What I did:** paired an AI agent (Claude Code) with Obsidian, a note
taking app built on markdown, and gave the agent a strict schema instead
of letting it write freeform notes.
1. Anything I drop in, an article, a class reading, a syllabus, a
   source, goes into an untouched "raw" folder, never edited after the
   fact.
2. The agent reads it, tells me the key takeaways *before* writing
   anything, then builds out linked wiki pages: one page per source, one
   per person, tool, or company it mentions, one per real idea or
   method, and deeper synthesis pages when multiple sources connect.
3. Every claim in the wiki has to trace back to an actual source. If the
   agent doesn't know something, the page says so outright instead of
   guessing.
4. When a new source disagrees with something already written down,
   both sides get kept, flagged as a contradiction, not silently
   overwritten.
5. On top of the wiki, it connects to my email and class schedule to put
   together a daily brief of what's actually due or coming up. So it's
   not just a note archive, it's something that proactively tells me
   what I need to know each morning.

**What happened:** it's been running and growing for a few weeks now,
over 70 linked pages and counting, self organizing as more goes in. I'm
not publishing the actual vault here since it has personal and academic
information in it, but the system itself, the schema, the ingest, query,
and lint workflow, is the interesting part, and that's what's described
above.

---

## omniroute-test: automated futures trading system

**Why I built it:** I wanted to see if I could build a real automated
trading system that could pass a funded futures evaluation (Topstep,
$50,000 account). Not just pick a strategy that looks good, but actually
solve the harder problem underneath it: making sure the *system* can't
hurt itself, even if a signal is wrong or the price data lies to it.

**The logic, in plain terms:** trade the New York morning session on
S&P 500 and Nasdaq futures. Wait for price to sweep a key liquidity
level, the prior day's high or low, since that's where a lot of stop
orders cluster. A sweep alone is a common false signal, so the system
also requires a Fair Value Gap to confirm a real shift in direction
before it actually enters.

**The math:** the stop is tight, about 4 times the recent average true
range, against a target 5 times that distance. With that kind of payoff
shape, the strategy does not need to win most of its trades. It needs
the wins, when they come, to be big enough to cover a string of small,
controlled losses along the way.

**What I did, step by step:**
1. Picked a strategy (a liquidity sweep and reversal setup on S&P 500
   and Nasdaq futures) and tested it against 6 months of real, minute by
   minute price history, about 190,000 individual price bars.
2. Split that history in two. Tuned the rules on the first 70%, then
   checked them against the *other* 30%, which the tuning never saw.
   That second check is the whole point. It's the difference between
   "looks good because I shaped it to look good" and "actually holds
   up."
3. Built a risk engine that sits *between* the strategy and the broker.
   A hard dollar cap per trade, a daily loss limit, a rule that stops
   trading after 3 losses in a row, and a kill switch, all independent
   of what the strategy itself says, so a bad signal physically can't
   turn into a bad trade.
4. Hit a real problem while getting ready to go live. The account's
   price feed lagged real time by minutes, which meant my own safety
   check (is this price still accurate?) kept correctly rejecting
   things. Instead of loosening that check to make it "work," I found a
   different, faster data connection the broker offered (a live
   streaming feed instead of the slow one) and rebuilt the price check
   around that.
5. Deliberately made the last step, actually connecting a real trading
   signal to the live broker, something that can't happen by accident.
   It requires a specific, manual decision, every time.

**What happened:** tested against real data, the best version of the
strategy made about $4,946 on the tuning half and $2,106 on the
untouched half of the 6 months, roughly 14% over that period, and it
worked on data it hadn't seen, not just the data it was shaped around.
No real order has been placed yet. That's intentional, not a limitation
I ran out of time to fix.

---

## overnight-pressure: systematic stock strategy research

**Why I built it:** I wanted to find out if there's a real, provable
reason to hold certain stocks *overnight* specifically, not day trade
them, just buy near the close and sell near the next open, with a hard
constraint of $0 to spend on data.

**What I did, step by step:**
1. Started from a real, measurable fact: almost all of the stock
   market's long run gain happens overnight, not during the actual
   trading day.
2. Built a way to rank which of the 500 biggest US stocks are worth
   holding overnight, based on a statistical model of price movement,
   and tested it against 10 years of real daily data (2016 to 2026).
3. Found a second, mostly unrelated idea, buying stocks right after a
   strong earnings report, a well documented pattern called
   post earnings announcement drift, where the market takes days or
   weeks to fully price in a big surprise. On its own that signal
   catches some false starts, so I added a filter: only take the
   earnings gap if the stock is also in the top 20% of 12 month minus
   1 month momentum that same day. Requiring both together, the
   earnings surprise and the broader trend, tested meaningfully better
   than either signal alone.
4. For every idea that didn't hold up (and there were several), kept it
   written down in the project instead of deleting it, specifically so
   the good numbers below aren't quietly cherry picked from a pile of
   hidden failures.

**What happened:** the overnight holding strategy alone beat just
holding the market by about 8.9% a year, with real statistical
confidence behind it, not a lucky looking chart. Adding the earnings
report idea pushed the tested number to about 15% a year, but about half
of that second piece's extra return turned out to just be "the market
was going up" rather than real skill, so I say 14 to 15% a year is the
honest number, not the bigger one. It's running now with real, small,
money in the market.

---

## daytrader-bot: early trading bot (Alpaca paper account)

**Why I built it:** before building anything with real evaluation money
on the line, I wanted to prove the basic mechanics of an automated
trading bot actually work: placing orders, sizing a position correctly,
stopping out on a loss, with zero real risk while I got it right.

**What I did:** built a simple moving average crossover strategy, wired
it to Alpaca's paper (fake money) trading account, and added the same
kind of risk rules, position size caps, a stop loss, a daily loss
circuit breaker, that later became the design basis for the real risk
engine in `omniroute-test`.

**What happened:** it worked. It proved the mechanics were solid, and
became the direct predecessor to the funded account system above.

---

## life-planner: meal planning and macro tracking app

**Why I built it:** I wanted a meal planning app that wouldn't quietly
lie to me. Every app I tried would guess a cooking time it didn't
actually know, or show a default value when it didn't have real data,
and present it exactly like a real number.

**What I did:** built a full meal planning app. Plan a week, see the
macros and points update live, get a grocery list generated straight
from the plan, cook with a step by step mode, with one rule enforced
everywhere in the code: if the app doesn't actually know something, it
says so on screen, instead of showing a confident looking guess.

**What happened:** it's live and I use it myself
(life-planner-liart.vercel.app), multi user, with your data synced to a
real hosted database in the background so a bad connection never loses
your plan.

---

## recomp-planner-legacy: body recomposition tracker

**Why I built it:** wanted something dead simple to track body
recomposition progress and schedule check ins, without standing up a
whole app for it.

**What I did:** built it as one single HTML file, no server, no
install, that also talks directly to Google Calendar so check in
reminders land on my actual calendar.

**What happened:** it worked well enough that the ideas in it (tracking
progress, not fabricating numbers) carried directly into `life-planner`
above.
