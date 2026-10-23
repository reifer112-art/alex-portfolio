# BranchdIn

**A networking site that connects college students with alumni who share their school, major, or student organization.**

Live at [branchdin.com](https://www.branchdin.com). The source code is private.

## The problem
Cold messages on big networks often go unanswered. Students want warm introductions to alumni who share real history with them, like the same school, major, or student organization.

## What I built
Next.js, TypeScript, and Supabase (database, sign in, and row level security), deployed on Vercel. The app has sign up flows for students and alumni, a directory and profiles, coffee chat booking, messaging, notifications, invite links, and AI drafted introduction messages.

## What I have verified is working (checked October 2026)
- **Student sign up with a school email**, using an emailed code. A friend at another school completed it.
- **Alumni sign up with LinkedIn**, with alumni details labeled as self reported.
- **Server side rules for verified students.** The database only completes a student profile when the confirmed email belongs to the chosen school.
- **Alumni landing page and invite links** for bringing in people I know.
- **Warm introduction route**, checked after its release.
- **Fixes confirmed on the live site.** I keep a bug list. Fourteen logged bugs were fixed and checked, including a search privacy leak that I found, fixed, and re-checked live.

## Built, but not yet proven live
Coffee chat booking is deployed and its page loads, with a database lock designed to stop double booking. I tested the logic on a restored copy of the data with fictional fixtures, but I have not yet completed a real booking on the live site, so I'm not claiming it works end to end.

## Decisions and tradeoffs
- LinkedIn does not share education history, so alumni details are self reported until someone vouches.
- I chose not to build a paid referral feature because it could raise employment agency and money transmission rules.
- A cross school search I built made members of other schools visible. I undid it the same day and chose not to rebuild it.
- Database changes happen only after a backup and a dry run on a restored copy.

## At a glance
- **My contribution:** product idea, trust rules, review of every change, database safety process, and every deploy decision. The code was written with AI coding tools.
- **Key decision:** enforce who counts as a verified student on the database side, not only in the sign up page.
- **Verified example:** a search privacy leak I found was fixed and re-checked on the live site.
- **What I learned:** reviews catch problems that building does not, and a feature is not done until it is proven live.

## What I'm improving
I am not publishing user numbers. I'm still tightening sign up and account checks and finishing the live coffee chat test.

## Process
Built with AI coding tools (Claude Code and Codex). I direct the work, review changes, and approve every deploy. Its private dashboard and agent team are described in [Growth HQ](branchdin-growth-hq.md).
