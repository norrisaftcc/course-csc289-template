# DataMan
### G4 Project Planning
### March 2026

## What this Document is About
As part of your programming capstone, you will go through the process of building
a full software product -- that is, a piece of software that *meets customer needs*.

These needs are generally called *user stories*. 

There are three major steps in this process:
- Determining what your product is, what it does, and who it's for
- Describing the product in two ways:
  - User Stories: What it's going to do (when it's finished)
  - Design Document: What tools and tech should be used to build it
- Building the project, through "development sprints"

The face-to-face groups have been working through these steps since late January.
Your group is starting in late March. That's not a problem — it just means we're
working efficiently and skipping things you don't need.

Here's what's different about your situation:

1. **Your project is already defined.** Other groups spent weeks figuring out what
   to build. Your product — DataMan — is a math challenge platform with known
   game modes and a known user base. That's a head start, not a disadvantage.

2. **You have three sprints instead of seven.** This means you're building an MVP
   (Minimum Viable Product), not the full dream version. An MVP is the smallest
   version of the product that actually works and actually demonstrates your skills.

3. **You're online.** Everything happens through GitHub, video calls, and async
   communication. This makes your team contract and communication norms even more
   important than they are for face-to-face groups.

---

## The Product: DataMan

DataMan is a web-based math challenge platform. Users log in, choose a game mode,
solve problems, and see their performance tracked over time. Problems get harder or
easier based on how the user is doing.

That's the whole pitch. Everything else is details.

### Where DataMan Comes From

The original Dataman was a handheld calculator made by Texas Instruments in 1977.
It was shaped like a robot and had math games designed to teach kids arithmetic.
It was popular. Several of you built console versions of some of these games
in CTS 285 last semester.

Your capstone job is to take those same core mechanics and build them as a
**full-stack web application** — with user accounts, a database, and a dashboard
that shows progress over time.

### Detailed Product Briefs

Two sets of detailed briefs have been prepared for you. They describe the **same
underlying engine** aimed at two different markets:

| Document | Target Market | Voice |
|----------|--------------|-------|
| `CogniCalibrate_base.md` + `CogniCalibrate_technical.md` | Corporate / workplace wellness | Professional, a bit satirical |
| `DataBuddy_base.md` + `DataBuddy_technical.md` | K-12 students and teachers | Friendly, encouraging, kid-safe |

**Read both sets.** These briefs contain your personas, your user stories, your
database schema, and your API design. They are the most important reference
documents you have.

For your MVP, the core loop is the same regardless of market direction:

```
User logs in → Picks a game mode → Solves problems → Sees their score → Comes back tomorrow
```

The differences between CogniCalibrate and DataBuddy are *product* differences
(who it's for, what it looks like, what language it uses), not *engine* differences.
For three sprints, you're building the engine. The product flavor is your choice —
pick whichever direction your team finds more interesting, or invent a third option.
The grading criteria are about the code, the process, and the demo — not which
market you chose.

### The Four Game Modes

DataMan has four solo game modes. These are the ones you're building.

| Original (1977) | CogniCalibrate Name | DataBuddy Name | What It Does |
|-----------------|---------------------|----------------|-------------|
| Answer Checker | Quick Calibration | Check It! | Show an equation with an answer. Is it right or wrong? |
| Number Guesser | Neural Narrowing | Mystery Number | Guess the secret number. Get higher/lower hints. |
| Electro Flash | Reflex Calibration | Speed Round | Rapid-fire problems against a countdown timer. |
| Missing Number | Pattern Extraction | Fill It In | An equation has a blank. Fill in the missing piece. |

The original Dataman also had multiplayer-style games (Wipe Out and Force Out).
These are not part of your MVP. Multiplayer requires real-time communication
between browsers (WebSocket infrastructure), which is a significant engineering
effort on top of everything else — honestly, it's a project in itself. Your MVP
focuses on the four solo modes done well, with adaptive difficulty and a
dashboard that tells a meaningful story about the user's progress.

---

## User Stories and Technical Requirements

Before we get into the sprint backlogs, a quick but important distinction:

**A user story** describes something a *user* can do with your product:

> **As a** [type of user], **I can** [do something], **so that** [I get some benefit].

User stories are written from the user's perspective. The user doesn't care about
your repository. The user doesn't care about your database. The user cares about
logging in, playing a game, and seeing their score.

**A technical requirement** describes something the *team* needs to do to make
the product work. Setting up the repo, configuring the database, deploying — these
are essential work, but they aren't user stories because no user is asking for them.

Both types of work go on your sprint board as GitHub Issues. Both get branches
and pull requests. The difference matters because it trains you to think about
*who benefits* from each piece of work — and in a professional setting, that
distinction is how you communicate with non-technical stakeholders.

---

### Technical Requirements: Project Setup

These need to happen before or during Sprint 1. They are not user stories —
they're the plumbing that makes user stories possible.

| ID | Requirement | Notes |
|----|------------|-------|
| T-1 | Repository created with branch protection on `main` and all team members added. | Require at least 1 PR approval before merge. |
| T-2 | README exists with: project name, how to run locally, team members. | Will grow over time. Start simple. |
| T-3 | GitHub Project Board created with columns: Backlog, Sprint Backlog, In Progress, In Review, Done. | |
| T-4 | Team contract completed and committed to the repo. | Roles, standup schedule, response time expectations. |
| T-5 | Application runs locally from a GitHub Codespace. | Flask + SQLite. Everyone can open it and run it. |
| T-6 | CLAUDE.md file exists at repo root with project name, tech stack, and how to run. | This is context for your AI coding assistant. |
| T-7 | SQLite database file created and connected to the application. | Start simple. We may move to PostgreSQL later. |

---

### Sprint 1: "Make it work once"
**Duration:** 2 weeks
**Goal:** A user can create an account, log in, play one game mode, and see their score saved.

This sprint is about building the pipeline from front to back. When it's done,
one complete path through your application works end-to-end.

**User Stories:**

| ID | User Story | Must-Have? |
|----|-----------|:----------:|
| 1.1 | **As a new user**, I can create an account with a username, email, and password. | ★ |
| 1.2 | **As a returning user**, I can log in with my credentials. | ★ |
| 1.3 | **As a logged-in user**, I can log out. | ★ |
| 1.4 | **As a user**, I see a dashboard after logging in that shows at least one game mode I can play. | ★ |
| 1.5 | **As a user**, I can play a full session of **Answer Checker** (15 questions: see an equation + answer, decide right or wrong). | ★ |
| 1.6 | **As a user**, I see a score summary after finishing a session (correct/total, accuracy %). | ★ |
| 1.7 | **As a user**, my session results are saved and persist across logins. | ★ |
| 1.8 | As a user, I can see a list of my past sessions on the dashboard. | |

**What "done" looks like for Sprint 1:**

You can open your application in a browser (running from a Codespace is fine for
now — you don't need a public URL yet). You can register. You can log in.
You can play Answer Checker. You can see your score. You can log out and
log back in and your score is still there. That's it. Nothing else matters yet.

---

### Sprint 2: "Make it smart"
**Duration:** 2 weeks
**Goal:** Three game modes, difficulty adapts to the player, dashboard shows progress over time.

This sprint turns your application from a toy into a product. The adaptive difficulty
engine is the core technical achievement of this project — it's what makes your
portfolio demo impressive.

**User Stories:**

| ID | User Story | Must-Have? |
|----|-----------|:----------:|
| 2.1 | **As a user**, I can play **Number Guesser** (guess a secret number, get higher/lower hints, see how many guesses it took). | ★ |
| 2.2 | **As a user**, I can play **Speed Round** (rapid-fire problems against a 60-second countdown timer, see how many I got right). | ★ |
| 2.3 | **As a user**, I can choose which game mode to play from the dashboard. | ★ |
| 2.4 | **As a user**, my difficulty adjusts between sessions based on my recent performance (problems get harder when I'm doing well, easier when I'm struggling). | ★ |
| 2.5 | **As a user**, I can see my current streak (consecutive days I've practiced) on the dashboard. | ★ |
| 2.6 | **As a user**, I can see my accuracy trend over the past 7 days. | ★ |
| 2.7 | As a user, I see my performance broken down by game mode on the dashboard. | |
| 2.8 | As a user, I see a "personal best" indicator when I beat my previous record in Speed Round. | |

**What "done" looks like for Sprint 2:**

Three game modes work. The application notices how you're doing and makes problems
harder or easier accordingly. The dashboard shows you're improving (or not) over time.

**How adaptive difficulty works (the short version):**

The system tracks your accuracy over your last 5 sessions per game mode.
If your rolling accuracy is above 85%, your difficulty tier goes up — meaning
harder problems next time. If it's below 55%, it goes down. Everything in
between stays the same.

"Harder problems" means different things per mode: bigger numbers, mixed
operations, tighter time pressure, closer decoy answers.

This is not complicated to implement. It's a rolling average and some if-statements.
But it's the feature that makes the demo sing.

---

### Sprint 3: "Make it real"
**Duration:** 2 weeks
**Goal:** Fourth solo mode, polished dashboard, deployed to a public URL, demo-ready.

This sprint is about completeness and polish. Everything you built in Sprints 1
and 2 should get better. The fourth game mode fills out the solo suite. And this
is when we deploy to the internet.

**User Stories:**

| ID | User Story | Must-Have? |
|----|-----------|:----------:|
| 3.1 | **As a user**, I can play **Fill It In** (equations with a missing number or operator — find the blank). | ★ |
| 3.2 | **As a user**, the dashboard shows a visual summary of my performance across all four modes. | ★ |
| 3.3 | As a user, the dashboard suggests which mode I should practice next (based on weakest performance). | |
| 3.4 | As a user, I earn achievements for milestones (first session, 5-day streak, 100 problems solved, etc.). | |
| 3.5 | **As a user**, the application works on a phone or tablet screen. | ★ |
| 3.6 | **As a user**, the application handles errors gracefully (loading states, error messages, empty states for new accounts). | ★ |

**Technical Requirements for Sprint 3:**

| ID | Requirement | Notes |
|----|------------|-------|
| T-8 | Application deployed to Render with a public URL. | This is when we go live on the internet. |
| T-9 | Database migrated from SQLite to PostgreSQL (if deploying to Render). | Render doesn't support SQLite in production. We'll help with this. |
| T-10 | README fully documents setup, architecture, and how to run the project. | Another developer could set it up from the README alone. |
| T-11 | Demo presentation prepared (10 minutes + Q&A). | |
| T-12 | Portfolio documentation prepared: description, screenshots, architecture diagram, live link. | |

**What "done" looks like for Sprint 3:**

Four game modes. Adaptive difficulty. A dashboard that tells a story about the
user's improvement. Deployed, stable, looks professional. You can demo it for
10 minutes without anything breaking, and a viewer understands what it does and
why it matters.

---

## Tech Stack

Your tech stack is decided. This keeps things simple and lets the team focus on
building the product rather than debating tools.

**Backend:** Flask (Python)
You know Python from CTS 285. Flask is lightweight, well-documented, and
appropriate for this project.

**Database:** SQLite (Sprints 1–2), PostgreSQL (Sprint 3)
SQLite requires zero setup — it's a file on disk. Perfect for local development
and for running in Codespaces. When you deploy to Render in Sprint 3, we'll
migrate to PostgreSQL (Render doesn't support SQLite for production). This
migration is smaller than it sounds — SQLAlchemy abstracts most of it.

**ORM:** SQLAlchemy
This lets you interact with the database using Python objects instead of writing
raw SQL. It also makes the SQLite-to-PostgreSQL migration straightforward.

**Frontend:** Jinja2 templates (server-rendered)
Flask serves HTML pages. No separate frontend framework. This is the simplest
path and gives you more time to focus on the game logic and database work.

**Development Environment:** GitHub Codespaces
Everyone gets the same environment. No "it works on my machine" problems. No
local installation headaches. Open the repo in a Codespace and you're ready to
develop.

**Deployment:** Codespaces (Sprints 1–2), Render (Sprint 3)
For the first two sprints, you demo from your Codespace. In Sprint 3, you deploy
to Render for a permanent public URL.

**AI Tooling:** Claude (via Claude Code or Claude Projects)
Use AI to write code faster. Understand what it writes. Iterate on its output.
Document your AI usage in PR descriptions.

---

## The Workflow

Every piece of work follows the same loop. Every time. No exceptions.

```
1. Create a GitHub Issue (describes what you're building)
        |
        v
2. Create a Branch (named after the issue)
        |
        v
3. Open a Draft Pull Request (before you write code)
        |
        v
4. Write and Test Your Code (this is the actual work)
        |
        v
5. Finish the Pull Request (mark ready for review)
        |
        v
6. Teammate Reviews the PR (at least one approval)
        |
        v
7. Merge to Main (closes the issue automatically)
```

This is the **Sacred Workflow**. Follow it every time, even when it feels like
overkill for small changes. The discipline is the point.

**Why this matters for an online team:** You don't have the luxury of tapping
someone on the shoulder and saying "hey, look at this." The pull request *is*
your conversation. Good PR descriptions and thorough code reviews are how
online teams avoid chaos.

---

## Sprint Ceremonies

Each sprint (2 weeks) has four ceremonies. Keep them short.

**Sprint Planning** (start of sprint) — Meet as a team (video call, 60 minutes).
Review the user stories for this sprint. Move stories from Backlog to Sprint
Backlog. Each person claims their first issue. Leave knowing what everyone is
working on.

**Daily Standup** (async, every day) — Post in your team channel: what I did,
what I'm working on next, what's blocking me. This takes 2 minutes. Do it anyway.
Silence is the enemy of online teams.

**Sprint Review** (end of sprint) — Meet as a team (video call, 30 minutes).
Demo what you built (running application, not slides). Be honest about what
didn't get done. Record the demo or take screenshots for your portfolio.

**Sprint Retrospective** (after review) — What went well? What didn't?
What will we change? Write it down. Commit it to the repo. It counts as a
deliverable.

---

## Timeline

| Week | Dates | Sprint | What Happens |
|------|-------|--------|-------------|
| 1 | Mar 24 – Mar 28 | Sprint 1 | Setup + Auth + Answer Checker |
| 2 | Mar 31 – Apr 4 | Sprint 1 | Complete first mode, running in Codespace |
| 3 | Apr 7 – Apr 11 | Sprint 2 | Number Guesser + Speed Round |
| 4 | Apr 14 – Apr 18 | Sprint 2 | Adaptive difficulty + Dashboard |
| 5 | Apr 21 – Apr 25 | Sprint 3 | Fill It In + Deploy to Render |
| 6 | Apr 28 – May 2 | Sprint 3 | Polish + Demo Prep + Portfolio |
| — | Early May (TBD) | — | **Final Presentation** |

---

## What You're Being Graded On

**Process (how you worked):**
- GitHub history: issues, branches, PRs, reviews, merges
- Sprint artifacts: planning docs, retro notes, board screenshots
- Team contribution: everyone ships code, everyone reviews code
- Communication: standups happened, decisions are documented

**Product (what you built):**
- Does it work? Can a stranger use it without instructions?
- Does it persist data correctly?
- Does adaptive difficulty function?
- Does the dashboard tell a meaningful story?

**Presentation (how you showed it):**
- Can you demo it live without it breaking?
- Can you explain the architecture?
- Can you answer questions about your code?
- Can you articulate what you learned?

**Portfolio (what you take away):**
- README that another developer could follow
- Screenshots or screen recording
- Architecture diagram
- Deployed live link
- Honest reflection on what went well and what didn't

---

## Getting Started: This Week

Here is your checklist for the first few days. Do these in order.

```
[ ] Read CogniCalibrate_base.md (the creative brief)
[ ] Read CogniCalibrate_technical.md (the technical brief)
[ ] Skim DataBuddy_base.md + DataBuddy_technical.md (compare)
[ ] As a team: decide your market direction (corporate, kids, or your own spin)
[ ] Create your GitHub Organization and invite all team members + instructor
[ ] Create the repository with a README and branch protection on main
[ ] Create your GitHub Project Board with the five columns
[ ] Write your team contract
[ ] Each person: open the repo in a Codespace, run it, confirm it works
[ ] Create your CLAUDE.md file
[ ] Load Sprint 1 user stories onto your project board as GitHub Issues
[ ] Begin Sprint 1
```

You're behind the other groups on calendar time. You're ahead of them on project
clarity. Use that.

---

*"Ship fast. Learn faster. Iterate always."*
