# Bag — Development Playbook

**A Semester-Based Financial Pacing App for College Students**

*Mattea · Loffert · Paul · Joey*

---

## Product Vision

College students receive financial aid in lump sums but spend it week by week across a 16-week semester. Existing budgeting apps assume monthly paychecks and steady income — they don't understand semesters. **Bag** closes that gap: it divides a student's total semester funds into a weekly "safe to spend" pace, visualizes where they stand, sends supportive (never judgmental) alerts, and optionally lets a parent or guardian see high-level progress without seeing individual purchases.

---

## Epics & User Stories

Each epic below maps to a major capability. Every user story follows the format:

> **As a** [role], **I want** [capability], **so that** [benefit].

Acceptance criteria are written as testable conditions. Stories are sized **S / M / L** to guide sprint planning.

---

### Epic 1 — Onboarding & Semester Setup

| ID | Story | Size | Acceptance Criteria |
|----|-------|------|---------------------|
| **S-1** | As a student, I want to create an account with my email so that my data persists across devices. | M | Account created, email verified, user lands on empty dashboard. |
| **S-2** | As a student, I want to define my semester (start date, end date, total weeks) so the app knows my time horizon. | S | Semester saved; dashboard shows "Week 1 of N." |
| **S-3** | As a student, I want to enter my total semester funds (one or more sources) so the app can calculate my weekly pace. | M | Multiple funding sources accepted (label + amount). Total computed. Weekly "safe to spend" = total ÷ weeks remaining. |
| **S-4** | As a student, I want to label each funding source (e.g. "GI Bill," "FAFSA," "Scholarship," "MyCAA," "Savings") so I can track which money I'm drawing from. | S | Labels display in funding breakdown view; each source shows its own balance. |

---

### Epic 2 — Dashboard & Pacing Visualization

| ID | Story | Size | Acceptance Criteria |
|----|-------|------|---------------------|
| **D-1** | As a student, I want to see a dashboard showing % of semester elapsed vs. % of funds spent so I can tell at a glance if I'm on track. | L | Two-metric display updates daily. Green/yellow/red indicator based on delta. |
| **D-2** | As a student, I want to see my "safe to spend this week" amount prominently so I know my budget without doing math. | S | Amount recalculates every Monday (or configurable semester-week boundary). |
| **D-3** | As a student, I want a week-by-week pacing chart so I can see my spending trend over the semester. | M | Line or bar chart; x-axis = weeks, y-axis = cumulative spend vs. ideal pace line. |

---

### Epic 3 — Expense Tracking & Categories

| ID | Story | Size | Acceptance Criteria |
|----|-------|------|---------------------|
| **E-1** | As a student, I want to log an expense with an amount, category, and optional note so my spending is recorded. | M | Expense persists, balance updates, category totals update. |
| **E-2** | As a student, I want default spending categories (Housing, Food, Transportation, Textbooks, Personal) that I can also customize so tracking fits my life. | S | Defaults pre-populated; user can add/rename/delete categories. |
| **E-3** | As a student, I want to see a breakdown of spending by category so I know where my money goes. | M | Pie or bar chart grouped by category; tappable for detail list. |

---

### Epic 4 — Supportive Alerts

| ID | Story | Size | Acceptance Criteria |
|----|-------|------|---------------------|
| **A-1** | As a student, I want a notification when my spending pace exceeds my weekly target so I can adjust before it's too late. | M | Push or in-app alert triggers when weekly spend > safe-to-spend amount. Tone is supportive ("You've used 35% of your semester funds — we're only at week 4. Let's recalibrate."). |
| **A-2** | As a student, I want a positive reinforcement alert when I'm pacing well so I feel encouraged. | S | Alert triggers when weekly spend ≤ target ("You're pacing well this week."). |
| **A-3** | As a student, I want to control which alerts I receive and how (push, in-app, email) so I'm not overwhelmed. | S | Settings screen with toggles per alert type and channel. |

---

### Epic 5 — Parent / Guardian Viewer

| ID | Story | Size | Acceptance Criteria |
|----|-------|------|---------------------|
| **P-1** | As a student, I want to invite a parent/guardian to a read-only view of my progress so they feel informed without controlling me. | M | Student generates invite link/code; viewer creates viewer account. |
| **P-2** | As a viewer, I can see overall progress (% spent, % semester elapsed) and category totals but **not** individual transactions so the student's privacy is preserved. | M | Viewer dashboard shows aggregate data only; no transaction list, no purchase details. |
| **P-3** | As a student, I want to revoke viewer access at any time so I stay in control. | S | One-tap revoke; viewer immediately loses access. |

---

### Epic 6 — Settings & Profile

| ID | Story | Size | Acceptance Criteria |
|----|-------|------|---------------------|
| **X-1** | As a student, I want to edit my semester dates and funding amounts mid-semester so I can adjust for changes (additional scholarships, reduced hours, etc.). | M | Edits recalculate weekly pace from current week forward. Historical data unchanged. |
| **X-2** | As a student, I want to reset or start a new semester so I can reuse Bag each term. | S | Archive current semester; fresh onboarding flow for new term. |

---

## Scrum Framework — Adapted for a 4-Person Capstone Team

### Roles

| Role | Who | Responsibility |
|------|-----|----------------|
| **Product Owner** | Rotate each sprint (or instructor acts as PO) | Prioritizes backlog, accepts/rejects completed stories. |
| **Scrum Master** | Rotate each sprint | Facilitates ceremonies, removes blockers, keeps the board current. |
| **Developers** | All four members | Write code, review PRs, write tests. |

Rotating PO and SM gives everyone leadership experience — a capstone learning objective.

### Sprint Cadence

**2-week sprints.** For a 16-week semester that gives you 7 full sprints plus a buffer week at each end for setup and final presentation prep.

| Week | Activity |
|------|----------|
| 1 | Sprint 0: repo setup, tooling, architecture decisions, CLAUDE.md written. |
| 2–3 | Sprint 1 |
| 4–5 | Sprint 2 |
| 6–7 | Sprint 3 |
| 8–9 | Sprint 4 |
| 10–11 | Sprint 5 |
| 12–13 | Sprint 6 |
| 14–15 | Sprint 7 |
| 16 | Final polish, presentation prep, retrospective. |

### Ceremonies

| Ceremony | When | Duration | What Happens |
|----------|------|----------|-------------|
| **Sprint Planning** | Day 1 of sprint | 1 hour | Pull stories from backlog into sprint. Assign owners. Break stories into tasks on GitHub Issues. |
| **Daily Standup** | Every class day (or async on Slack/Discord) | ≤ 15 min | Each person: what I did, what I'm doing, what's blocking me. |
| **Sprint Review** | Last day of sprint | 30 min | Demo working software to instructor/stakeholders. |
| **Sprint Retrospective** | After review | 30 min | What went well? What didn't? One concrete change for next sprint. |

### Definition of Done

A story is **Done** when:

1. Code is merged to `main` via approved PR.
2. All acceptance criteria pass (manually tested or automated).
3. No regressions in existing features.
4. Code has been reviewed by at least one teammate.
5. CLAUDE.md is updated if architecture changed.

---

## GitHub Repository Structure

```
bag/
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── user-story.md          # template for story issues
│   │   └── bug-report.md
│   └── pull_request_template.md
├── CLAUDE.md                      # project context for Claude Code
├── README.md
├── docs/
│   └── architecture.md            # living architecture doc
├── frontend/                      # React Native or web app
│   ├── src/
│   │   ├── components/
│   │   ├── screens/
│   │   ├── hooks/
│   │   ├── context/
│   │   └── utils/
│   ├── package.json
│   └── ...
├── backend/                       # API server
│   ├── src/
│   │   ├── routes/
│   │   ├── models/
│   │   ├── middleware/
│   │   ├── services/
│   │   └── utils/
│   ├── package.json
│   └── ...
└── shared/                        # types, constants shared across
    └── types/
```

### Branching Strategy

Use **GitHub Flow** (simple, fits a small team):

```
main ← always deployable
 └── feature/S-3-funding-sources
 └── feature/D-1-dashboard-pacing
 └── fix/A-1-alert-timing
```

**Rules:**

- `main` is protected. No direct pushes.
- Every story gets a branch named `feature/{story-id}-short-description`.
- Bugs get `fix/{story-id}-short-description`.
- PRs require 1 approval before merge.
- Squash-merge to keep history clean.

### Issue Tracking

Use **GitHub Projects** (board view) with columns:

| Column | Meaning |
|--------|---------|
| **Backlog** | All stories not yet in a sprint. |
| **Sprint Backlog** | Stories committed to the current sprint. |
| **In Progress** | Actively being worked. Limit: 1 per person. |
| **In Review** | PR open, awaiting review. |
| **Done** | Merged to main, acceptance criteria met. |

Each issue uses the **user-story template**:

```markdown
## User Story
As a [role], I want [capability], so that [benefit].

## Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2

## Technical Notes
<!-- Implementation hints, API endpoints, data model changes -->

## Story Points
<!-- S = 1, M = 3, L = 5 -->
```

---

## Claude Code Workflow — Feature by Feature, Story by Story

This is the core workflow loop. Each team member works through their assigned story using Claude Code as a pair programmer.

### Step 0: CLAUDE.md — The Project Brain

Before any coding begins, the team writes a `CLAUDE.md` file at the repo root. This is the single most important file for Claude Code — it reads this first every session. Keep it updated.

```markdown
# CLAUDE.md

## Project
Bag — a semester-based financial pacing app for college students.

## Tech Stack
- Frontend: [React Native / Next.js / your choice]
- Backend: [Node + Express / your choice]
- Database: [PostgreSQL / SQLite / your choice]
- Auth: [your choice]

## Architecture
- RESTful API. Frontend calls backend over HTTP.
- See docs/architecture.md for data model and API contract.

## Conventions
- All components use functional React with hooks.
- API routes follow /api/v1/{resource} pattern.
- Error responses use { error: string, code: number } shape.
- Tests live next to source files: Component.test.tsx.

## Current Sprint
Sprint [N]: [list of story IDs in progress]

## How to Run
- `cd frontend && npm install && npm run dev`
- `cd backend && npm install && npm run dev`

## How to Test
- `cd frontend && npm test`
- `cd backend && npm test`
```

### Step 1: Claim a Story

1. Assign yourself a story on the GitHub Project board.
2. Move it to **In Progress**.
3. Create a feature branch:

```bash
git checkout -b feature/E-1-log-expense
```

### Step 2: Kick Off with Claude Code

Open Claude Code in the repo and give it context:

```
I'm working on story E-1: "As a student, I want to log an expense with an
amount, category, and optional note so my spending is recorded."

Acceptance criteria:
- Expense persists to database
- Balance updates after entry
- Category totals update

Please read CLAUDE.md for project context, then let's plan the
implementation together before writing code.
```

Claude Code will:
- Read `CLAUDE.md` and understand your stack.
- Propose an implementation plan (API route, data model, frontend form).
- Wait for your approval before writing code.

### Step 3: Build Iteratively

Work through the story in small increments:

1. **Data model** — "Create the Expense model/migration."
2. **API route** — "Create POST /api/v1/expenses and GET /api/v1/expenses."
3. **Frontend form** — "Create the AddExpense screen that calls the API."
4. **Integration** — "Wire the form to the API, update the balance display."
5. **Tests** — "Write tests for the expense API route and the form component."

Each increment is a conversation turn. Review what Claude Code produces, ask for adjustments, then move on.

### Step 4: PR and Review

```bash
git add -A
git commit -m "feat(E-1): add expense logging with categories"
git push -u origin feature/E-1-log-expense
gh pr create --title "E-1: Log expense with amount, category, note" \
  --body "Closes #[issue-number]. Adds POST/GET expense endpoints and AddExpense screen."
```

A teammate reviews the PR. They can use Claude Code to help review:

```
Please review this PR diff for story E-1. Check for:
- Missing error handling
- Security issues (input validation, SQL injection)
- Consistency with our conventions in CLAUDE.md
```

### Step 5: Merge and Move On

After approval, squash-merge to `main`. Move the story to **Done** on the board. Pick up the next story.

---

## Recommended Sprint Plan

This plan sequences stories so each sprint builds on the last, producing a working increment.

### Sprint 0 (Week 1) — Foundation

**Goal:** Running app skeleton, repo ready, team aligned.

- Set up GitHub repo with branch protection and PR template.
- Choose tech stack (hold a 30-minute architecture decision meeting).
- Write `CLAUDE.md` and `docs/architecture.md`.
- Scaffold frontend and backend projects.
- Set up CI (GitHub Actions: lint + test on every PR).
- Each team member makes one trivial PR to practice the workflow.
- TY4YC.

### Sprint 1 (Weeks 2–3) — Onboarding

**Stories:** S-1, S-2, S-3, S-4

**Goal:** A student can sign up, define a semester, and enter funding sources. The app calculates weekly "safe to spend."

### Sprint 2 (Weeks 4–5) — Core Dashboard

**Stories:** D-1, D-2, E-1, E-2

**Goal:** Student sees the pacing dashboard and can log expenses with categories. The "safe to spend" amount updates in real time.

### Sprint 3 (Weeks 6–7) — Visualization & Categories

**Stories:** D-3, E-3

**Goal:** Pacing chart and category breakdown are visible. The app now tells a visual story of the student's semester.

### Sprint 4 (Weeks 8–9) — Alerts

**Stories:** A-1, A-2, A-3

**Goal:** The app sends supportive notifications based on spending pace. Students can configure alert preferences.

### Sprint 5 (Weeks 10–11) — Parent/Guardian Viewer

**Stories:** P-1, P-2, P-3

**Goal:** A parent can see aggregate progress. Student controls access. "Transparency without control" is realized.

### Sprint 6 (Weeks 12–13) — Settings & Polish

**Stories:** X-1, X-2

**Goal:** Mid-semester adjustments work. Semester reset/archive works. Edge cases handled.

### Sprint 7 (Weeks 14–15) — Hardening

**Goal:** Bug fixes, performance, accessibility audit, final UI polish. No new features — only quality.

### Week 16 — Presentation & Retro

**Goal:** Final demo. Team retrospective. Celebrate.

---

## Team Assignments — Suggested Ownership Areas

Play to each member's strengths while ensuring everyone touches full-stack code:

| Member | Primary Area | Why |
|--------|-------------|-----|
| **Mattea** | Alerts system & notification preferences | Presented the supportive-not-judgmental philosophy. Owns the tone. |
| **Loffert** | Dashboard & pacing visualization | Presented the core value prop ("safe to spend per week"). |
| **Paul** | Expense tracking & category management | Presented the "current apps fail" analysis — knows the gap to fill. |
| **Joey** | Onboarding, semester setup & parent viewer | Presented the "why" — owns the first and last impressions. |

Everyone reviews everyone's PRs. No silos.

---

## Claude Code Tips for the Team

**Start every session** by telling Claude Code which story you're on and asking it to read `CLAUDE.md`. This grounds it in your project.

**Keep conversations focused.** One story per conversation. If you switch stories, start a new session.

**Use Claude Code for code review.** Paste a diff or point it at a PR. It catches things tired eyes miss.

**Use Claude Code for tests.** After building a feature, ask: "Write unit tests for what we just built." It's good at this.

**Update CLAUDE.md as you go.** If you add a new convention, a new dependency, or change the data model — update the file. Future-you and your teammates will thank present-you.

**Don't copy-paste blindly.** Read what Claude Code produces. Understand it. Ask "why did you do it this way?" if something's unclear. This is a learning experience.

---

## Token Faucet Note

The instructor will provide Claude Code API access (token faucet) to each team member. Logistics TBD — focus on the plan first, the tokens will follow.

---

*Secure your bag. Ship your code. Own your independence.*
