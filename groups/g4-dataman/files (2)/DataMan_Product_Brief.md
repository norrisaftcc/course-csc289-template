# DataMan — Product Brief

### CSC 289 · Group 4 · Spring 2026

---

## How to Use This Document

This document describes what DataMan *is* — who it's for, what problem it
solves, what each game mode feels like to play, and what success looks like.
There is no code in this document. There are no database tables. If it helps,
think of this as the document you'd hand to someone who asked "so what are
you building?" at a dinner party.

The companion document `DataMan_Technical.md` covers how DataMan works under
the hood — database schema, adaptive difficulty algorithm, Flask routes, file
structure. That's the document you keep open while coding.

The sprint schedule, user stories, and grading criteria are in
`G4_Project_Planning.md`.

**Read this one first.** It's shorter, and everything in the technical doc
will make more sense once you understand who you're building for and why.

---

## What Is DataMan?

DataMan is a web application where people practice math through short,
game-like sessions. After each session, the app saves their results and
quietly adjusts the difficulty of future problems based on how they've
been doing — harder if they're cruising, easier if they're struggling.

The original Dataman was a handheld calculator made by Texas Instruments in
1977. It was shaped like a robot, ran on a 9-volt battery, and had five math
games designed to help kids learn arithmetic. It was wildly popular. Some of
you built console versions of its games in CTS 285 last semester.

Your job is to take those core mechanics and rebuild them as a modern web
application — with user accounts, persistent data, adaptive difficulty, and
a dashboard that shows progress over time.

### Why Does This Product Exist?

Math practice has a motivation problem. It's not that people can't do math.
It's that the experience of practicing math is almost always boring, stressful,
or both.

Worksheets are boring. Timed tests are stressful. Most math apps are either
"drill disguised as a game" (boring with better graphics) or "game with math
interruptions" (fun but shallow). The result: people avoid practice, and the
skills they once had get rusty.

DataMan's bet is simple: **if the problems are at the right difficulty, the
feedback is immediate, and progress is visible, people will practice
voluntarily.** Not because they have to. Because it feels like a game they're
getting better at.

The right difficulty is the key. Too easy and it's boring. Too hard and it's
frustrating. The sweet spot — where you have to think but you *can* do it —
is different for every person and shifts as they improve. That's what the
adaptive difficulty engine handles.

---

## Who Is DataMan For?

DataMan isn't built for one specific audience. The same core loop — play
a quick session, see your score, watch yourself improve — works for very
different people in very different contexts. Here are three.

---

### Mia, Age 10 — The Reluctant Practicer

Mia is in 5th grade. She's okay at math — not terrible, not great. She'd
rather be drawing. Her mom wants her to practice math at home, but worksheets
turn into a nightly battle. Mia would do almost anything to avoid them.

**How Mia uses DataMan:**
She opens it on her Chromebook after dinner. The dashboard shows a 6-day
streak and she doesn't want to break it. She picks Speed Round because it
feels like a game, not homework. Sixty seconds of rapid-fire multiplication.
She gets 16 right — two more than yesterday. The app says "New personal
best!" She plays one more round, then goes back to drawing.

**What matters to Mia:**
- It has to feel like a *game*, not school
- Short sessions (10 minutes max, not 30)
- Visible proof she's improving (streaks, personal bests)
- Never feeling stupid — the problems should be hard enough to be interesting
  but not so hard she gets frustrated and quits

**What Mia would say about DataMan:**
*"It's better than worksheets. I like beating my score."*

---

### James, Age 34 — The Coffee Break Sharpener

James is a project manager at a mid-size company. He's not bad at math, but
he hasn't done mental arithmetic regularly since college. He plays Wordle
every morning and does the NYT crossword on his commute. He likes short
mental challenges that make him feel sharp.

**How James uses DataMan:**
He opens it during his morning coffee, before his first meeting. Picks
Answer Checker — 15 quick true/false decisions about equations. Takes about
two minutes. He's been doing it daily for three weeks, and his accuracy in
the harder tiers has gone from 65% to 82%. The trend line on the dashboard
is visible proof that his brain still works. He switches to Fill It In for
a slightly harder challenge, then closes the tab.

**What matters to James:**
- Very short sessions (2-5 minutes)
- Feels productive, not juvenile — he doesn't want a cartoon mascot
- The difficulty should match his level (he'll get bored at Tier 1 instantly)
- Data he can see: trends, streaks, improvement over time
- Works on his phone or laptop with no install

**What James would say about DataMan:**
*"It's like Wordle but for math. Takes two minutes. I do it every day now."*

---

### Dorothy, Age 68 — The Stay-Sharp Senior

Dorothy retired from teaching three years ago. She's sharp and she intends
to stay that way. Her doctor told her that daily mental exercise helps
maintain cognitive function. She tried a few brain training apps but most
of them felt patronizing or were full of ads. She wants something
straightforward.

**How Dorothy uses DataMan:**
She plays every morning after breakfast on her iPad. She starts with Number
Guesser (she likes the logical deduction aspect) and then does a round of
Fill It In. She's methodical — she's been at it for two months and her
dashboard shows a steady, visible improvement trend in every mode. She
checks her streak every day. She's on day 43 and she's not about to break it.

**What matters to Dorothy:**
- Respectful tone — not childish, not condescending
- Clear, readable interface (good contrast, decent font size)
- Works on a tablet
- She wants to see evidence that practice is doing something
- No ads, no tricks, no subscriptions to manage
- Difficulty that genuinely challenges her, not pretend-hard

**What Dorothy would say about DataMan:**
*"It's the only app I use every day. I like seeing the numbers go up."*

---

### What the Personas Have in Common

Look at what all three want:

1. **Short sessions.** Nobody wants to practice math for 30 minutes.
2. **The right difficulty.** Too easy is boring. Too hard is frustrating. The
   sweet spot is different for each person.
3. **Visible progress.** Streaks, trends, personal bests. Proof that
   practicing is making them better.
4. **Respect for their time.** No ads. No upsells. No seven-step onboarding
   flow. Open it, play, see your score, done.

These four things should drive every design decision you make.

---

## The Four Game Modes

Each mode is a different kind of math challenge. They all feel different to
play, but they share the same underlying pattern: play a session, save the
results, adjust the difficulty, show progress.

---

### Mode 1: Answer Checker

**The pitch:** An equation appears on screen with a proposed answer. Is it
right, or is it wrong?

This is the simplest mode and the one you build first. It's quick, binary
(only two possible responses), and low-pressure. Even people who say they're
"bad at math" can look at "24 + 17 = 43" and think "that doesn't look right."

A session is 15 questions. Takes about 2 minutes.

At lower difficulty, the wrong answers are obviously wrong (off by 10 or more).
At higher difficulty, they're near-misses (off by 1), which forces real mental
arithmetic instead of gut-checking.

---

### Mode 2: Number Guesser

**The pitch:** DataMan picks a secret number. You guess. After each guess,
you're told "higher" or "lower." Find the number in as few guesses as possible.

This mode feels different from the others — it's deductive, not arithmetic.
The math is implicit. Each guess narrows the range. Smart players will
converge quickly; others will take more guesses and improve over time.

A session is 5 rounds (5 different secret numbers). The score is total
guesses across all rounds — lower is better.

At lower difficulty, the range is small (1-20). At higher difficulty, the
range is wide (1-1000) and the hints are minimal.

Fun fact: the mathematically optimal strategy is binary search — always guess
the middle of the remaining range. You don't need to teach this, but surfacing
"Optimal guesses for this range: 7" on the results screen plants a seed.

---

### Mode 3: Speed Round

**The pitch:** A 60-second countdown timer. Problems appear one at a time.
Answer as many as you can before the clock runs out.

This is the most exciting mode and the best one for demos. There's genuine
tension as the timer counts down. The score is how many you get right out of
how many you attempt, so rushing and guessing is penalized.

Speed Round is where the personal best feature really shines. "I got 18
last time, can I get 19?" is exactly the kind of motivation loop that makes
people come back.

At lower difficulty, it's basic single-digit addition and subtraction. At
higher difficulty, it's multi-digit mixed operations with less time.

---

### Mode 4: Fill It In

**The pitch:** An equation has a blank — a missing number or a missing
operator. Figure out what goes in the blank.

This is the mode that does the most cognitive heavy lifting. Answer Checker
is recognition ("does this look right?"). Fill It In is recall and reasoning
("what makes this true?"). It introduces early algebraic thinking without
using the word "algebra."

A session is 10 questions. Takes about 3-5 minutes.

At lower difficulty, it's missing addends (__ + 5 = 12). At higher difficulty,
it's missing operators (24 __ 6 = 4) and multi-step expressions with
parentheses.

---

## The Dashboard

The dashboard is the hub. It's what users see after logging in, between
sessions, and when they want to check on their progress. It answers the
question every user asks: **"How am I doing?"**

### What the Dashboard Should Communicate

**For Mia:** "You've been practicing every day and it's working. Look at
your streak! Try Speed Round — you almost beat your record yesterday."

**For James:** "Your accuracy is trending up across the board. You're at
Tier 5 in Answer Checker now. Two months ago you were at Tier 2."

**For Dorothy:** "43-day streak. Your Fill It In accuracy improved 12% this
month. Here's the chart."

### The Information, in Priority Order

1. **Streak counter.** How many days in a row they've practiced. This is the
   single most motivating number for retention. Show it prominently.

2. **Play buttons.** One for each mode. Don't make users hunt for how to
   start a session.

3. **Last session summary.** What they played most recently, and how it went.

4. **Accuracy trend.** A simple visualization showing their accuracy over
   the past week or two. Even a row of colored dots (green = good day,
   yellow = okay, red = rough day) is better than nothing. A line chart
   is better.

5. **Per-mode breakdown.** How they're doing in each mode separately. Some
   people are strong in Speed Round but struggle in Fill It In. The dashboard
   should make that visible so they know where to focus.

6. **Personal bests.** Best score in each mode. People love seeing records.

### What the Dashboard Should NOT Do

- **Don't shame.** "You missed 3 days!" is demotivating. "Welcome back!
  Ready to start a new streak?" is the same information delivered with care.

- **Don't overwhelm.** A new user's dashboard should look inviting, not
  empty. Show the play buttons and a simple welcome message. Data fills in
  as they use the app.

- **Don't lie.** Don't inflate scores or hide bad sessions. The data should
  be honest. Growth is motivating even when the starting point is low.

---

## What Makes DataMan a "Product" (Not Just a Homework Assignment)

Three features separate a product from a demo:

**Persistence.** When the user comes back tomorrow, everything is still
there — their history, their streak, their difficulty tier. The database
makes this possible. Without it, every session is isolated and there's no
reason to come back.

**Adaptation.** The application pays attention. It makes problems harder
when the user is doing well and easier when they're struggling. This happens
automatically. The user doesn't configure anything. They just notice that
the problems feel "about right" — challenging but doable. This is the feature
that makes the demo impressive and the portfolio entry credible.

**Progress visibility.** The dashboard tells a story. Not just "you scored
12/15" but "your accuracy has improved 15% over the past two weeks." That
story is why Dorothy checks her streak every morning and why Mia plays one
more round after dinner.

---

## Design Principles

When you're making a decision about how something should look, feel, or work,
run it through these filters:

**Would Mia use this voluntarily?** If it feels like homework, redesign it.

**Would James use this in two minutes?** If it takes too long to get started
or too many clicks to play a session, simplify it.

**Would Dorothy find this readable?** If the text is too small, the contrast
is too low, or the layout is confusing, fix it.

**Does the data tell a story?** If the dashboard just shows numbers without
context, add context. "78% accuracy" means little. "78% accuracy, up from
62% last week" means everything.

---

## What You're Building — The Summary

A web application with:

- **User accounts** (register, log in, log out)
- **Four game modes** (Answer Checker, Number Guesser, Speed Round, Fill It In)
- **Persistent results** (sessions saved to a database)
- **Adaptive difficulty** (problems get harder/easier based on performance)
- **A dashboard** (streaks, trends, per-mode breakdown, personal bests)

Built with Flask, SQLite (moving to PostgreSQL for deployment), and Jinja2
templates. Running in GitHub Codespaces for development, deployed to Render
in the final sprint.

That's the product. The technical doc covers how to build it.

---

*"Ship fast. Learn faster. Iterate always."*
