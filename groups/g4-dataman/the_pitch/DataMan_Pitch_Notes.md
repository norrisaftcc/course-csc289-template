# DataMan Pitch — Speaker's Notes

### Video Script for Group 4 Project Introduction
### CSC 289 · Spring 2026

---

## How to Use This Document

These are the speaker's notes for the DataMan pitch presentation
(`DataMan_Pitch.html`). They're written as a video script — what the
instructor says while advancing through the slides. Each section
corresponds to one slide.

**Target length:** 8-10 minutes total (matching the pitch rubric).

**Tone:** Direct, warm, enthusiastic about the product without overselling.
The audience is students who are about to build this — they need to
understand the "why" before they open a code editor.

---

## Slide 0: Title
**[~15 seconds]**

"This is the pitch presentation for DataMan — the project that Group 4
will be building this semester. The other groups already gave their
pitches in person a few weeks ago. This video is your equivalent.

By the end of this, you'll understand what you're building, who you're
building it for, and why it matters."

---

## Slide 1: The Problem
**[~60 seconds]**

"Let's start with the problem — before we talk about the product, before
we talk about the team, before anything else. Because if you don't
understand the problem, the solution won't make sense.

Math practice has a motivation problem. Most people aren't bad at math.
They're bad at *wanting to practice math*. And honestly, can you blame
them?

Worksheets are boring. Timed tests create anxiety. Most apps are either
drill-and-kill with better graphics, or they're genuinely fun games where
the math is an afterthought — you're really just playing a video game
with occasional multiplication interruptions.

The result is predictable: people avoid practice. Kids fight with their
parents about it. Adults lose the mental math skills they had in school.
Seniors worry about cognitive decline but the tools available to them
feel patronizing.

The practice itself isn't the problem. The *experience* of practicing
is the problem."

---

## Slide 2: The Difficulty Problem
**[~45 seconds]**

"There's a deeper issue, though. Even when someone sits down and tries
to practice, most tools get the difficulty wrong.

If the problems are too easy, you're bored in thirty seconds. You're not
learning anything. You close the app.

If the problems are too hard, you're frustrated immediately. You think
'I'm bad at this' and you close the app.

The sweet spot — where you actually have to think, but you *can* do
it — is different for every single person. A 10-year-old and a
34-year-old and a 68-year-old all need different levels of challenge.
And that sweet spot shifts as they improve.

Most tools don't adjust. They give everyone the same problems, or they
offer three static levels — Easy, Medium, Hard — and hope for the best.
DataMan adjusts automatically. That's the core innovation."

---

## Slide 3: The Team
**[~20 seconds]**

"Group 4 is the online cohort. In the filmed version, I'm introducing
the project on your behalf. When you give your final presentation in
May, each of you will introduce yourselves — your name and your primary
role on the team. Keep it to thirty seconds total. The audience wants to
know who you are, not your life story."

*[Note: If filming, the instructor can mention team members by name
here if the roster is finalized, or leave it as a placeholder.]*

---

## Slide 4: The Personas
**[~90 seconds]**

"So who are we building for? Let me introduce you to two people.

First: Mia. She's ten. She's in fifth grade. She's okay at math — not
terrible, not great. She'd rather be drawing. Her mom wants her to
practice at home, but worksheets turn into a nightly battle. Mia has
tried three different math apps. She used each one for about a week
before getting bored, because the problems were either too easy — she
felt like a baby — or too hard, and she felt stupid.

Mia doesn't hate math. She hates the *experience* of practicing math.

Second: James. He's thirty-four. He's a project manager. He hasn't done
mental math regularly since college, and honestly, he uses a calculator
for everything now. But he plays Wordle every morning. He does the
crossword on his commute. He *likes* short mental challenges that make
him feel sharp. He'd do math puzzles too — if they took two minutes
and didn't feel like they were designed for children.

Notice what Mia and James have in common: they'd both practice
willingly if the difficulty was right and the session was short. That's
the insight DataMan is built on."

---

## Slide 5: The Solution
**[~60 seconds]**

"DataMan is a web-based math challenge platform with four game modes,
adaptive difficulty, and a progress dashboard.

Four game modes — each one exercises a different kind of thinking.
Answer Checker is fast binary decisions: is this equation right or
wrong? Number Guesser is deductive reasoning: find the secret number
with higher/lower hints. Speed Round is fluency under time pressure:
how many can you solve in sixty seconds? Fill It In is algebraic
thinking: find the missing piece of the equation.

Adaptive difficulty — the app tracks your recent accuracy and adjusts
automatically. If you're getting 90% right, problems get harder. If
you're struggling at 50%, they get easier. No configuration required.
The user doesn't even know it's happening — they just notice that the
problems feel 'about right.'

Progress dashboard — streaks, accuracy trends, personal bests. The
dashboard tells a story: 'you're getting better.' That story is what
brings people back.

And persistent accounts — register, log in, and everything is saved.
Your streak, your history, your difficulty tier. Come back tomorrow
and pick up where you left off."

---

## Slide 6: Connecting to Personas
**[~45 seconds]**

"Let me show you how this plays out for our two users.

Mia opens DataMan after dinner. Her dashboard shows a six-day streak,
and she doesn't want to break it. She picks Speed Round because it
feels like a game. Sixty seconds of multiplication. She gets sixteen
right — two more than yesterday. The app says 'New personal best!'
She plays one more round, then goes back to drawing. Total time: four
minutes. She practiced math voluntarily.

James opens DataMan with his morning coffee. His dashboard shows his
accuracy is up twelve percent this month. He picks Answer Checker —
fifteen quick right-or-wrong decisions. The problems are at Tier 5,
which means they're genuinely challenging for him. Done in two minutes.
Closes the tab. He'll do it again tomorrow.

Both people solved the same problem: math practice that doesn't feel
like punishment. The adaptive difficulty made it personal. The dashboard
made the progress visible."

---

## Slide 7: Demo Walkthrough
**[~60 seconds]**

"Since nothing is built yet — that's your job — let me walk you
through what the finished product looks like.

You register with a username, email, and password. Standard stuff.

After logging in, you see the dashboard. Your streak is displayed
prominently — that's the most motivating number. Below it are play
buttons for each of the four modes. Below that, your accuracy trend
for the past week, shown as a simple visual. The dashboard also tells
you which mode you're strongest in and which one needs work.

You click Play on Speed Round. A sixty-second timer starts. Math
problems appear one at a time. You type your answer and hit enter.
Immediately you get feedback — right or wrong — and the next problem
appears. When the timer hits zero, you see your results.

The results screen shows your score, whether you beat your personal
best, and whether your difficulty tier went up. Then you can play
again or go back to the dashboard.

That's the whole loop. Registration, dashboard, play, results,
dashboard. Simple."

---

## Slide 8: Technical Architecture
**[~45 seconds]**

"Under the hood, this is a standard three-tier web application.

The browser handles HTML, CSS, and JavaScript. The JavaScript is mostly
for the Speed Round timer — that has to run client-side so there's no
network delay between problems.

Flask handles the backend — routing, session management, game logic,
the adaptive difficulty algorithm. Jinja2 templates render the HTML.
SQLAlchemy talks to the database.

The database starts as SQLite — it's a file on disk, zero configuration,
perfect for development in Codespaces. In Sprint 3, when you deploy to
Render, you'll migrate to PostgreSQL. SQLAlchemy abstracts the
difference, so it's a config change, not a rewrite.

Why Flask? Because you already know Python from CTS 285. Why SQLite
first? Because it eliminates setup friction — you can focus on building
features instead of configuring a database server."

---

## Slide 9: Database Schema
**[~45 seconds]**

"Five tables. Each one answers a different question.

The users table handles authentication — who is this person, are they
allowed to log in, what's their current streak.

The sessions table records every game session — who played, which mode,
what tier, how many they got right.

The responses table stores individual answers within a session. This is
optional in Sprint 1 — you can start with just session-level totals and
add per-question tracking in Sprint 2.

The user_tiers table tracks where each user is in each mode — their
current difficulty level. One row per user per mode.

And daily_stats rolls everything up into one row per user per day, which
is what powers the dashboard trend chart.

The relationships are straightforward: users have many sessions, sessions
have many responses. The schema is documented in detail in the technical
brief."

---

## Slide 10: Next Steps / Roadmap
**[~45 seconds]**

"Three sprints, six weeks.

Sprint 1 is 'make it work once.' User registration, login, Answer
Checker, score saved to the database, running in a Codespace. At the
end of Sprint 1, one complete path through the application works end
to end.

Sprint 2 is 'make it smart.' Two more game modes — Number Guesser and
Speed Round — plus the adaptive difficulty engine, streaks, and the
accuracy trend chart. This is where the product comes alive. The known
technical challenge here is the client-side timer for Speed Round.

Sprint 3 is 'make it real.' The fourth mode — Fill It In — plus a
polished dashboard, deployment to Render, responsive design, and demo
prep. The known challenge here is the SQLite-to-PostgreSQL migration,
but SQLAlchemy handles most of it.

At the end of Sprint 3, you have a deployed, working product that you
can demo live and put on your portfolio."

---

## Slide 11: Close
**[~20 seconds]**

"DataMan. Math practice that meets you where you are.

Mia practices because it feels like a game. James practices because
it takes two minutes. The app remembers their progress, adapts to their
level, and shows them they're improving.

Your job is to build it. The product brief, the technical brief, and
the project planning doc are your roadmap. Read them, set up your repo,
and start Sprint 1.

Questions? Ask in our next team meeting, or post them in the channel."

---

## Timing Summary

| Slide | Section | Target Time |
|-------|---------|-------------|
| 0 | Title | 15 sec |
| 1 | Problem Statement | 60 sec |
| 2 | Difficulty Problem | 45 sec |
| 3 | Team Introduction | 20 sec |
| 4 | Personas | 90 sec |
| 5 | Solution Overview | 60 sec |
| 6 | Persona Connection | 45 sec |
| 7 | Demo Walkthrough | 60 sec |
| 8 | Architecture | 45 sec |
| 9 | Database Schema | 45 sec |
| 10 | Roadmap | 45 sec |
| 11 | Close | 20 sec |
| **Total** | | **~8.5 min** |

This leaves room for natural pauses and ~1.5 minutes of buffer before
hitting the 10-minute limit.

---

## Rubric Alignment Checklist

| Rubric Requirement | Slide(s) | Status |
|-------------------|----------|--------|
| Problem stated BEFORE solution | 1-2 | ✓ Problem is slides 1-2, solution is slide 5 |
| Problem explains who experiences it | 1, 4 | ✓ "Kids, adults, seniors" → two specific personas |
| Why current solutions are inadequate | 1, 2 | ✓ Worksheets, drill apps, gamified apps, difficulty mismatch |
| Team introduction with roles | 3 | ✓ Template for team to fill in |
| TWO distinct personas with names | 4 | ✓ Mia (10) and James (34) |
| Personas have context and pain points | 4 | ✓ Age, situation, specific frustration, quote |
| Personas presented BEFORE solution | 4 vs 5 | ✓ Personas slide 4, solution slide 5 |
| Solution explains what platform IS | 5 | ✓ "Web-based math challenge platform" |
| 3-5 key features identified | 5 | ✓ Four features: modes, adaptive, dashboard, accounts |
| Features connected to persona pain points | 6 | ✓ Mia's experience, James's experience |
| Demo shows working functionality | 7 | ✓ Walkthrough mockup (nothing built yet) |
| Architecture diagram with data flow | 8 | ✓ Browser ↔ Flask ↔ Database |
| Database schema with relationships | 9 | ✓ Five tables, relationships shown |
| Technology justification | 8 | ✓ "Why Flask? Why SQLite?" |
| Sprint 2 roadmap with specifics | 10 | ✓ Three sprints detailed with known challenges |

---

*"Ship fast. Learn faster. Iterate always."*
