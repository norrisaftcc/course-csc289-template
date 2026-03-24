# DataMan — Technical Reference

### CSC 289 · Group 4 · Spring 2026

---

## How to Use This Document

This is your technical reference for building DataMan. It covers how each
game mode works in detail (with pseudocode), what the database looks like
(with example data), how the adaptive difficulty algorithm works, what Flask
routes you need, and a suggested file structure.

**Keep this open while you're coding.** It's designed as a reference you
come back to, not a one-time read.

**Companion documents:**
- `DataMan_Product_Brief.md` — What DataMan is, who it's for, what
  each mode feels like to play. Read that first if you haven't.
- `G4_Project_Planning.md` — Sprint schedule, user stories, workflow,
  grading criteria.

---

## Part 1: The Four Game Modes — Implementation Details

Each mode is a different type of math challenge. They all save results to the
same database structure, so the dashboard and adaptive difficulty system work
across all of them.

---

### Mode 1: Answer Checker

**How a session works:**

1. The application generates 15 equations, each with a proposed answer
2. Roughly half the proposed answers are correct, half are wrong
3. For each one, the user clicks "Right" or "Wrong"
4. After all 15, the user sees their score

**Example screen:**

```
 ┌────────────────────────────────────────┐
 │                                        │
 │           24 + 17 = 41                 │
 │                                        │
 │       [ ✓ Right ]    [ ✗ Wrong ]       │
 │                                        │
 │                          Question 7/15 │
 └────────────────────────────────────────┘
```

**What gets harder at higher difficulty:**

| Tier | What changes |
|------|-------------|
| 1-2 | Single-digit addition and subtraction. Wrong answers are obviously wrong (off by 10+). |
| 3-4 | Double-digit operations. Wrong answers are closer to correct (off by 1-3). |
| 5-6 | Mixed operations (addition, subtraction, multiplication). Some multi-step. |
| 7-8 | Larger numbers, division, order of operations. Wrong answers are "near misses." |

**How to generate a question (pseudocode):**

```python
def generate_answer_checker_question(tier):
    # Pick numbers appropriate for the tier
    if tier <= 2:
        a = random.randint(1, 9)
        b = random.randint(1, 9)
        op = random.choice(['+', '-'])
    elif tier <= 4:
        a = random.randint(10, 50)
        b = random.randint(10, 50)
        op = random.choice(['+', '-', '*'])
    # ... and so on for higher tiers

    correct_answer = eval(f"{a} {op} {b}")

    # Decide if we show the right answer or a wrong one
    show_correct = random.choice([True, False])

    if show_correct:
        displayed_answer = correct_answer
    else:
        # Make a wrong answer that's close (harder to spot)
        offset = random.randint(1, max(2, tier))
        displayed_answer = correct_answer + random.choice([-1, 1]) * offset

    return {
        'equation': f"{a} {op} {b} = {displayed_answer}",
        'correct_response': 'right' if show_correct else 'wrong'
    }
```

**What you save to the database for each answer:**

- Which question it was (the equation, the displayed answer)
- What the user clicked (right or wrong)
- Whether the user was correct
- How long it took them to answer (in milliseconds)

---

### Mode 2: Number Guesser

**How a session works:**

1. The application picks a secret number within a range (e.g., 1-100)
2. The user guesses
3. The application says "higher" or "lower" (or "correct!")
4. Repeat until correct
5. One session = 5 rounds (5 different secret numbers)
6. Score is based on total guesses across all rounds

**Example screen:**

```
 ┌────────────────────────────────────────┐
 │  I'm thinking of a number from 1-100   │
 │                                        │
 │  Your guess: [  50  ] [Guess!]         │
 │                                        │
 │  📈 "Go higher!"                       │
 │                                        │
 │  Previous guesses: 50                  │
 │                            Round 3 / 5 │
 └────────────────────────────────────────┘
```

**What gets harder at higher difficulty:**

| Tier | What changes |
|------|-------------|
| 1-2 | Range is 1-20. Generous hints ("way higher" vs "a little higher"). |
| 3-4 | Range is 1-100. Standard hints. |
| 5-6 | Range is 1-500. Minimal hints. |
| 7-8 | Range is 1-1000. Just "higher" or "lower." Timed rounds. |

**The optimal strategy:** Binary search — always guess the middle of the
remaining range. For 1-100, the optimal player needs at most 7 guesses.
A nice results screen touch: "You found it in 5 guesses! (Optimal: 7)"

**What you save to the database for each round:**

- The secret number
- The range (min, max)
- Each guess and whether it was higher/lower
- Total number of guesses
- Time to complete the round

---

### Mode 3: Speed Round

**How a session works:**

1. Timer starts at 60 seconds
2. A problem appears (e.g., "6 × 7 = ?")
3. User types their answer and submits
4. Immediately get the next problem (with right/wrong feedback)
5. When the timer hits zero, session ends
6. Score: how many correct out of how many attempted

**Example screen:**

```
 ┌────────────────────────────────────────┐
 │  ⏱ 0:42                               │
 │                                        │
 │           6 × 7 = [____]              │
 │                                        │
 │  ✓ ✓ ✗ ✓ ✓ ✓ ✓ ✓ ✗ ✓ ✓ ✓             │
 │                                        │
 │  Correct: 10    Attempted: 12          │
 └────────────────────────────────────────┘
```

**What gets harder at higher difficulty:**

| Tier | What changes |
|------|-------------|
| 1-2 | Basic addition/subtraction facts. Single digit. |
| 3-4 | Multiplication and division facts. Single digit. |
| 5-6 | Double-digit operations. Mixed. |
| 7-8 | Multi-step problems. Shorter time window (45 seconds). |

**Important implementation detail:** The timer must run on the client side
(in JavaScript), not on the server. If the timer ran on the server, every
answer submission would require a round-trip and the delay would make it
unplayable. The server validates the final results, but the timing is
client-side.

**What you save to the database for each answer:**

- The problem and the correct answer
- What the user typed
- Whether it was correct
- Time from problem appearing to answer submitted (milliseconds)
- And for the session overall: total time, total attempted, total correct

---

### Mode 4: Fill It In

**How a session works:**

1. 10 equations, each with one blank
2. User types the missing piece
3. After all 10, see the score

**Example screens:**

```
 ┌────────────────────────────────────────┐
 │        ___ + 5 = 12                    │
 │        Answer: [____] [Submit]         │
 │                          Question 4/10 │
 └────────────────────────────────────────┘

         (answer: 7)

 A harder version:

         24 ___ 6 = 4

         (answer: ÷)
```

**What gets harder at higher difficulty:**

| Tier | What changes |
|------|-------------|
| 1-2 | Missing addend. Single digit. (__ + 3 = 8) |
| 3-4 | Missing number in any position. Double digit. |
| 5-6 | Missing operator. Mixed operations. |
| 7-8 | Multi-step with parentheses. Order of operations matters. |

**How to generate a question (pseudocode):**

```python
def generate_fill_in_question(tier):
    if tier <= 2:
        # Missing first number: ___ + b = c
        b = random.randint(1, 9)
        answer = random.randint(1, 9)
        c = answer + b
        return {
            'display': f"___ + {b} = {c}",
            'answer': str(answer),
            'blank_type': 'number'
        }
    elif tier <= 4:
        # Missing number in any position, bigger numbers
        # ... similar logic with larger ranges
    elif tier <= 6:
        # Missing operator: a ___ b = c
        a = random.randint(2, 12)
        b = random.randint(2, 12)
        op = random.choice(['+', '-', '*'])
        c = eval(f"{a} {op} {b}")
        return {
            'display': f"{a} ___ {b} = {c}",
            'answer': op,
            'blank_type': 'operator'
        }
```

**What you save:** Same pattern as Answer Checker — the question, the correct
answer, what the user entered, whether they were right, and how long it took.

---

## Part 2: The Database

This section describes every table you need, what each column is for, and what
the actual data looks like with examples. If you're new to database design,
read this section carefully.

### Why These Tables?

The application needs to answer questions like:

- "Who is this user and are they allowed to log in?" → **users** table
- "What happened during this game session?" → **sessions** table
- "What did the user answer for each question?" → **responses** table
- "How hard should the problems be for this user in this mode?" → **user_tiers** table
- "How has this user been doing recently?" → **daily_stats** table

Each table answers a different question. If you find yourself needing to answer
a question that none of these tables can handle, you may need to add a column
or a table — and that's fine.

---

### Table: users

Stores one row per registered user.

| Column | Type | What It's For |
|--------|------|--------------|
| id | Integer, primary key, auto-increment | Unique ID for each user |
| username | String(50), unique, not null | Display name |
| email | String(255), unique, not null | Login identifier |
| password_hash | String(255), not null | Hashed password (never store plain text!) |
| created_at | DateTime, default now | When they registered |
| current_streak | Integer, default 0 | Consecutive days they've played |
| longest_streak | Integer, default 0 | Their all-time streak record |
| last_active_date | Date, nullable | The last day they completed a session |

**Example row:**

```
id: 1
username: "sofia_r"
email: "sofia@example.com"
password_hash: "pbkdf2:sha256:260000$abc123..."  (this is what hashed passwords look like)
created_at: 2026-03-25 14:30:00
current_streak: 4
longest_streak: 4
last_active_date: 2026-03-28
```

**Important:** Use Flask-Login or a similar library for authentication. Don't
build your own session management from scratch. Hash passwords with `werkzeug`
(which Flask includes): `generate_password_hash()` and `check_password_hash()`.

---

### Table: sessions

Stores one row per game session (one time a user sits down and plays a mode).

| Column | Type | What It's For |
|--------|------|--------------|
| id | Integer, primary key | Unique session ID |
| user_id | Integer, foreign key → users.id | Who played |
| mode | String(20), not null | Which game mode: 'answer_checker', 'number_guesser', 'speed_round', 'fill_it_in' |
| started_at | DateTime, not null | When the session began |
| ended_at | DateTime, nullable | When it ended (null if abandoned) |
| total_questions | Integer | How many questions were presented |
| correct_answers | Integer | How many the user got right |
| accuracy | Float | correct_answers / total_questions (stored for convenience) |
| avg_response_ms | Integer, nullable | Average time per answer in milliseconds |
| difficulty_tier | Integer | What tier they were playing at (1-8) |

**Example row:**

```
id: 42
user_id: 1
mode: "speed_round"
started_at: 2026-03-28 15:00:00
ended_at: 2026-03-28 15:01:03
total_questions: 18
correct_answers: 14
accuracy: 0.778
avg_response_ms: 3200
difficulty_tier: 3
```

This tells us: Sofia played Speed Round at tier 3, attempted 18 problems in
about a minute, and got 14 right (77.8% accuracy).

---

### Table: responses

Stores one row per individual answer within a session. This is your biggest
table — if a user plays 5 sessions of 15 questions each, that's 75 rows.

| Column | Type | What It's For |
|--------|------|--------------|
| id | Integer, primary key | Unique response ID |
| session_id | Integer, foreign key → sessions.id | Which session this belongs to |
| question_number | Integer | Position in the session (1, 2, 3...) |
| question_data | String/JSON | The question that was shown (enough to reconstruct it) |
| correct_answer | String | What the right answer was |
| user_answer | String | What the user actually entered |
| is_correct | Boolean | Did they get it right? |
| response_time_ms | Integer | How long it took them (milliseconds) |

**Example row:**

```
id: 501
session_id: 42
question_number: 7
question_data: "6 × 7 = ?"
correct_answer: "42"
user_answer: "42"
is_correct: true
response_time_ms: 2100
```

**Do you need this table?** For your MVP, you could skip individual response
tracking and only store session-level totals (in the sessions table). That's
a valid Sprint 1 simplification. Add the responses table in Sprint 2 when you
need per-question data for the adaptive difficulty algorithm and the dashboard.

---

### Table: user_tiers

Tracks the current difficulty tier for each user in each mode. One row per
user-mode combination.

| Column | Type | What It's For |
|--------|------|--------------|
| id | Integer, primary key | Unique row ID |
| user_id | Integer, foreign key → users.id | Which user |
| mode | String(20), not null | Which game mode |
| current_tier | Integer, default 1 | Their current difficulty tier (1-8) |
| updated_at | DateTime | When the tier last changed |

**Example rows for one user:**

```
id: 1,  user_id: 1,  mode: "answer_checker",  current_tier: 4,  updated_at: 2026-03-28
id: 2,  user_id: 1,  mode: "speed_round",     current_tier: 3,  updated_at: 2026-03-27
id: 3,  user_id: 1,  mode: "number_guesser",  current_tier: 2,  updated_at: 2026-03-26
```

This tells us: Sofia is at different tiers in different modes. She's strongest
in Answer Checker (tier 4) and newest at Number Guesser (tier 2). Each mode
adapts independently.

**Unique constraint:** Each user should have at most one row per mode. Use a
unique constraint on (user_id, mode) so you don't accidentally create duplicates.
When a tier changes, you UPDATE the existing row, not INSERT a new one.

---

### Table: daily_stats

Stores one row per user per day, summarizing that day's activity. This powers
the dashboard trend chart and streak tracking.

| Column | Type | What It's For |
|--------|------|--------------|
| id | Integer, primary key | Unique row ID |
| user_id | Integer, foreign key → users.id | Which user |
| date | Date, not null | Which day |
| sessions_played | Integer | How many sessions they completed that day |
| total_questions | Integer | Total questions across all sessions |
| total_correct | Integer | Total correct answers across all sessions |
| accuracy | Float | total_correct / total_questions |
| time_spent_seconds | Integer | Total active time |

**Example rows:**

```
user_id: 1, date: 2026-03-25, sessions: 2, questions: 30, correct: 23, accuracy: 0.767
user_id: 1, date: 2026-03-26, sessions: 1, questions: 15, correct: 13, accuracy: 0.867
user_id: 1, date: 2026-03-27, sessions: 3, questions: 43, correct: 38, accuracy: 0.884
user_id: 1, date: 2026-03-28, sessions: 2, questions: 33, correct: 28, accuracy: 0.848
```

This tells us: Sofia has been playing every day, and her accuracy is trending
upward. That's exactly the kind of story the dashboard should tell.

**When to compute this:** After each session ends, update (or create) the
daily_stats row for that user and date. This is simpler than computing it
on the fly every time the dashboard loads.

**Unique constraint:** One row per (user_id, date). If the user plays three
sessions on the same day, you UPDATE the totals, not INSERT three rows.

---

### How the Tables Connect

```
  users
    │
    ├── sessions (one user has many sessions)
    │       │
    │       └── responses (one session has many responses)
    │
    ├── user_tiers (one user has one tier per mode)
    │
    └── daily_stats (one user has one summary per day)
```

**In SQLAlchemy terms:**

```python
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    # ... other columns ...
    sessions = db.relationship('Session', backref='user', lazy=True)

class Session(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    user_id = db.Column(db.Integer, db.ForeignKey('user.id'), nullable=False)
    # ... other columns ...
    responses = db.relationship('Response', backref='session', lazy=True)
```

If you haven't used SQLAlchemy before, the
[Flask-SQLAlchemy quickstart](https://flask-sqlalchemy.palletsprojects.com/)
is the place to start. It takes about 30 minutes to read and will save you hours.

---

## Part 3: Adaptive Difficulty

This is the feature that makes DataMan a *product* and not just a quiz. It's
also simpler than it sounds.

### The Algorithm

After each session, the system looks at how the user has been doing in that
mode recently. If they're crushing it, problems get harder. If they're
struggling, problems get easier.

```
After a session ends in mode X:

1. Look at the user's last 5 sessions in mode X
2. Calculate their average accuracy across those sessions
3. Compare to thresholds:
   - If average accuracy > 85%  → tier goes UP by 1 (max 8)
   - If average accuracy < 55%  → tier goes DOWN by 1 (min 1)
   - Otherwise                  → tier stays the same
4. Update the user_tiers table
```

That's it. It's a rolling average and two if-statements.

### In Python

```python
def update_difficulty(user_id, mode):
    """Call this after every session ends."""

    # Get the last 5 sessions for this user in this mode
    recent = Session.query.filter_by(
        user_id=user_id,
        mode=mode
    ).order_by(Session.ended_at.desc()).limit(5).all()

    if len(recent) < 2:
        return  # Not enough data yet, keep current tier

    # Calculate average accuracy
    avg_accuracy = sum(s.accuracy for s in recent) / len(recent)

    # Get current tier
    tier_record = UserTier.query.filter_by(
        user_id=user_id,
        mode=mode
    ).first()

    if tier_record is None:
        tier_record = UserTier(user_id=user_id, mode=mode, current_tier=1)
        db.session.add(tier_record)

    # Adjust
    if avg_accuracy > 0.85 and tier_record.current_tier < 8:
        tier_record.current_tier += 1
    elif avg_accuracy < 0.55 and tier_record.current_tier > 1:
        tier_record.current_tier -= 1

    tier_record.updated_at = datetime.utcnow()
    db.session.commit()
```

### What Each Tier Means

The tier number (1-8) controls the parameters of the question generator.
Here's what those parameters look like for Answer Checker as an example:

| Tier | Number Range | Operations | Wrong Answer Offset |
|------|-------------|-----------|-------------------|
| 1 | 1-9 | + only | ±5 or more (obvious) |
| 2 | 1-9 | +, - | ±3 to ±5 |
| 3 | 10-50 | +, - | ±2 to ±4 |
| 4 | 10-50 | +, -, × | ±1 to ±3 |
| 5 | 10-99 | +, -, × | ±1 to ±2 |
| 6 | 10-99 | +, -, ×, ÷ | ±1 (near misses) |
| 7 | 10-200 | All, multi-step | ±1 |
| 8 | 10-500 | All, multi-step | ±1 |

Each mode has its own tier parameter table (see the tier tables in Part 1).
The tier number is a simple integer, and your question generator uses it to
look up how hard to make things.

**You don't need all 8 tiers for your MVP.** Tiers 1-4 with clear differences
between them is plenty for a demo. You can add tiers 5-8 as refinement.

### What the User Sees

The user does **not** see their tier number. They just notice that problems
are getting harder or easier. The only hint should be subtle:

- After a tier-up: "Nice work! Here come some tougher ones."
- After a tier-down: "Let's practice these a bit more."
- Otherwise: nothing. The difficulty just shifts quietly.

---

## Part 4: Dashboard Implementation

The product brief describes what the dashboard should communicate. This
section covers where that data comes from technically.

### Data Sources

Every piece of dashboard data comes from a database query:

| Dashboard element | Query |
|-------------------|-------|
| Streak | `users.current_streak` (updated when sessions are logged) |
| Last session | Most recent row from `sessions` for this user |
| 7-day accuracy | Last 7 rows from `daily_stats` for this user |
| Per-mode accuracy | `sessions` grouped by mode, averaged |
| Personal bests | `sessions` filtered by mode, ordered by correct_answers desc, limit 1 |

### Updating the Streak

The streak logic runs every time a session ends:

```python
def update_streak(user):
    today = date.today()

    if user.last_active_date == today:
        return  # Already played today, streak unchanged

    if user.last_active_date == today - timedelta(days=1):
        # Played yesterday — streak continues
        user.current_streak += 1
    else:
        # Missed a day (or first session ever) — streak resets
        user.current_streak = 1

    if user.current_streak > user.longest_streak:
        user.longest_streak = user.current_streak

    user.last_active_date = today
    db.session.commit()
```

### Updating Daily Stats

Call this after every session ends:

```python
def update_daily_stats(user_id, session):
    today = date.today()
    stats = DailyStats.query.filter_by(user_id=user_id, date=today).first()

    if stats is None:
        stats = DailyStats(user_id=user_id, date=today,
                           sessions_played=0, total_questions=0,
                           total_correct=0, accuracy=0)
        db.session.add(stats)

    stats.sessions_played += 1
    stats.total_questions += session.total_questions
    stats.total_correct += session.correct_answers
    stats.accuracy = stats.total_correct / stats.total_questions
    db.session.commit()
```

---

## Part 5: Flask Routes

Here's a map of the routes your Flask application will need, organized by
which sprint they belong to.

### Sprint 1 Routes

```
GET  /                     → Landing page (redirect to /dashboard if logged in)
GET  /register             → Show registration form
POST /register             → Create account, redirect to /dashboard
GET  /login                → Show login form
POST /login                → Authenticate, redirect to /dashboard
GET  /logout               → Log out, redirect to /login
GET  /dashboard            → Main dashboard (requires login)
GET  /play/answer-checker  → Start an Answer Checker session
POST /play/answer-checker  → Submit an answer (or submit entire session)
GET  /session/<id>/results → Show results for a completed session
```

### Sprint 2 Routes (add these)

```
GET  /play/number-guesser  → Start a Number Guesser session
POST /play/number-guesser  → Submit a guess
GET  /play/speed-round     → Start a Speed Round session
POST /play/speed-round     → Submit results (client-side timer, server validates)
```

### Sprint 3 Routes (add these)

```
GET  /play/fill-it-in      → Start a Fill It In session
POST /play/fill-it-in      → Submit an answer
```

**A note about game state in Flask:** Each game session needs to track
state (which question you're on, what the answers have been so far). You have
two options:

1. **Flask session (cookie-based):** Store game state in `session['game_state']`.
   Simple, but limited in size and lost if the user clears cookies.

2. **Database-backed:** Create the `sessions` row when the game starts, and
   update it as the user answers. More work, but the data is already where
   it needs to be for the dashboard.

Option 2 is recommended because you need the data in the database anyway.

---

## Part 6: Quick Reference

### The Numbers That Matter

| What | Value | Where It's Used |
|------|-------|----------------|
| Answer Checker questions per session | 15 | Session length |
| Number Guesser rounds per session | 5 | Session length |
| Speed Round duration | 60 seconds | Timer |
| Fill It In questions per session | 10 | Session length |
| Difficulty tiers | 1 through 8 | Question generation |
| Rolling window for difficulty | Last 5 sessions | Adaptive algorithm |
| Promote threshold | > 85% accuracy | Tier goes up |
| Demote threshold | < 55% accuracy | Tier goes down |
| Starting tier for new users | 1 | Default |

### File Structure (Suggested)

```
dataman/
├── app.py                  # Flask app setup, config
├── models.py               # SQLAlchemy models (User, Session, Response, etc.)
├── routes/
│   ├── auth.py             # Register, login, logout
│   ├── dashboard.py        # Dashboard view
│   └── play.py             # Game mode routes
├── game/
│   ├── answer_checker.py   # Question generation for mode 1
│   ├── number_guesser.py   # Question generation for mode 2
│   ├── speed_round.py      # Question generation for mode 3
│   ├── fill_it_in.py       # Question generation for mode 4
│   └── difficulty.py       # Adaptive difficulty algorithm
├── templates/
│   ├── base.html           # Base template (nav, layout)
│   ├── dashboard.html      # Dashboard page
│   ├── login.html          # Login form
│   ├── register.html       # Registration form
│   ├── play/
│   │   ├── answer_checker.html
│   │   ├── number_guesser.html
│   │   ├── speed_round.html
│   │   └── fill_it_in.html
│   └── results.html        # Session results page
├── static/
│   ├── style.css
│   └── game.js             # Client-side timer for Speed Round
├── dataman.db              # SQLite database file (not committed to git!)
├── requirements.txt        # Python dependencies
├── CLAUDE.md               # AI assistant context
└── README.md               # How to run, architecture, team info
```

**Important:** Add `dataman.db` to your `.gitignore` file. The database should
not be committed to version control — each developer generates their own local
copy, and production will use PostgreSQL anyway.

### Python Dependencies (requirements.txt)

```
Flask==3.0.*
Flask-SQLAlchemy==3.1.*
Flask-Login==0.6.*
Werkzeug==3.0.*
```

That's it for Sprint 1. Add more as needed.

---

## You're Ready

Between this document, the product brief, and the planning doc, you have
everything you need to start Sprint 1 today.

When you get stuck — and you will, everyone does — re-read the relevant
section here, talk to your team, ask your AI assistant, and ask your
instructor. In that order.

Good luck. Build something you're proud of.

---

*"Ship fast. Learn faster. Iterate always."*
