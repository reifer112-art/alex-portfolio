# Alex Reifer — Projects

Six things I've built. For each one: why I started it, what I actually did,
and what happened. Private for now.

---

## second brain — a personal knowledge system (Claude Code + Obsidian)

**Why I built it:** I read everything from lecture notes to research papers
to random articles, and none of it stayed connected — a month later I'd
half-remember a good idea and have no way to find where it came from. I
wanted one system that actually keeps growing smarter instead of just
piling up notes.

**What I did:** paired an AI agent (Claude Code) with Obsidian, a
markdown-based note-taking app, and gave the agent a strict schema instead
of letting it write freeform notes:
1. Anything I drop in — an article, a class reading, a syllabus, a source —
   goes into an untouched "raw" folder, never edited after the fact.
2. The agent reads it, tells me the key takeaways *before* writing
   anything, then builds out linked wiki pages: one page per source, one
   per person/tool/company it mentions, one per real idea or method, and
   deeper synthesis pages when multiple sources connect.
3. Every claim in the wiki has to trace back to an actual source — if the
   agent doesn't know something, the page says so outright instead of
   guessing.
4. When a new source disagrees with something already written down, both
   sides get kept, flagged as a contradiction, not silently overwritten.
5. On top of the wiki, it connects to my email and class schedule to put
   together a daily brief of what's actually due or coming up — so it's
   not just a note archive, it's something that proactively tells me what
   I need to know each morning.

**What happened:** it's been running and growing for a few weeks now — 70+
linked pages and counting, self-organizing as more goes in. I'm not
publishing the actual vault here since it has personal and academic
information in it, but the system itself — the schema, the ingest/query/
lint workflow — is the interesting part, and that's what's described above.

---

## omniroute-test — automated futures trading system

**Why I built it:** I wanted to see if I could build a real automated
trading system that could pass a funded futures evaluation (Topstep,
$50,000 account) — not just pick a strategy that looks good, but actually
solve the harder problem underneath it: making sure the *system* can't hurt
itself, even if a signal is wrong or the price data lies to it.

**What I did, step by step:**
1. Picked a strategy (a liquidity-sweep-and-reversal setup on S&P 500 /
   Nasdaq futures) and tested it against 6 months of real, minute-by-minute
   price history — about 190,000 individual price bars.
2. Split that history in two: tuned the rules on the first 70%, then
   checked them against the *other* 30%, which the tuning never saw. That
   second check is the whole point — it's the difference between "looks
   good because I shaped it to look good" and "actually holds up."
3. Built a risk engine that sits *between* the strategy and the broker: a
   hard dollar cap per trade, a daily loss limit, a "stop after 3 losses in
   a row" rule, and a kill switch — all independent of what the strategy
   itself says, so a bad signal physically can't turn into a bad trade.
4. Hit a real problem while getting ready to go live: the account's price
   feed lagged real time by minutes, which meant my own safety check (is
   this price still accurate?) kept correctly rejecting things. Instead of
   loosening that check to make it "work," I found a different, faster data
   connection the broker offered (a live streaming feed instead of the slow
   one) and rebuilt the price check around that.
5. Deliberately made the last step — actually connecting a real trading
   signal to the live broker — something that can't happen by accident. It
   requires a specific, manual decision, every time.

**What happened:** tested against real data, the best version of the
strategy made about $4,946 on the tuning half and $2,106 on the untouched
half of the 6 months — roughly 14% over that period, and it worked on data
it hadn't seen, not just the data it was shaped around. No real order has
been placed yet — that's intentional, not a limitation I ran out of time
to fix.

---

## overnight-pressure — systematic stock strategy research

**Why I built it:** I wanted to find out if there's a real, provable reason
to hold certain stocks *overnight* specifically — not day-trade them, just
buy near the close and sell near the next open — with a hard constraint of
$0 to spend on data.

**What I did, step by step:**
1. Started from a real, measurable fact: almost all of the stock market's
   long-run gain happens overnight, not during the actual trading day.
2. Built a way to rank which of the 500 biggest US stocks are worth holding
   overnight, based on a statistical model of price movement, and tested it
   against 10 years of real daily data (2016–2026).
3. Found a second, mostly unrelated idea — buying stocks right after a
   strong earnings report — and tested whether combining the two made
   things better or just added noise.
4. For every idea that didn't hold up (and there were several), kept it
   written down in the project instead of deleting it, specifically so the
   good numbers below aren't quietly cherry-picked from a pile of hidden
   failures.

**What happened:** the overnight-holding strategy alone beat just holding
the market by about 8.9% a year, with real statistical confidence behind
it (not a lucky-looking chart). Adding the earnings-report idea pushed the
tested number to about 15% a year — but about half of that second piece's
extra return turned out to just be "the market was going up" rather than
real skill, so I say 14–15%/year is the honest number, not the bigger one.
It's running now with real (small) money in the market.

---

## daytrader-bot — early trading bot (Alpaca paper account)

**Why I built it:** before building anything with real evaluation money on
the line, I wanted to prove the basic mechanics of an automated trading bot
actually work — placing orders, sizing a position correctly, stopping out
on a loss — with zero real risk while I got it right.

**What I did:** built a simple moving-average crossover strategy, wired it
to Alpaca's paper (fake-money) trading account, and added the same kind of
risk rules — position size caps, a stop-loss, a daily-loss circuit breaker
— that later became the design basis for the real risk engine in
`omniroute-test`.

**What happened:** it worked — proved the mechanics were solid — and became
the direct predecessor to the funded-account system above.

---

## life-planner — meal planning and macro tracking app

**Why I built it:** I wanted a meal-planning app that wouldn't quietly lie
to me. Every app I tried would guess a cooking time it didn't actually
know, or show a default value when it didn't have real data — and present
it exactly like a real number.

**What I did:** built a full meal-planning app — plan a week, see the
macros/points update live, get a grocery list generated straight from the
plan, cook with a step-by-step mode — with one rule enforced everywhere in
the code: if the app doesn't actually know something, it says so on
screen, instead of showing a confident-looking guess.

**What happened:** it's live and I use it myself
(life-planner-liart.vercel.app) — multi-user, with your data synced to a
real hosted database in the background so a bad connection never loses
your plan.

---

## recomp-planner-legacy — body recomposition tracker

**Why I built it:** wanted something dead simple to track body-recomp
progress and schedule check-ins, without standing up a whole app for it.

**What I did:** built it as one single HTML file — no server, no install —
that also talks directly to Google Calendar so check-in reminders land on
my actual calendar.

**What happened:** it worked well enough that the ideas in it (tracking
progress, not fabricating numbers) carried directly into `life-planner`
above.
