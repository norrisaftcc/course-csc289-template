# Assignment - Make One Feature 
## AKA "Solo Burger"

Phase 1: The Solo Burger.md

Phase 1: The Solo Burger
Your First Story Through the Sacred Workflow

Walkthrough video (relevant part starts 8 minutes in): https://us-lti.bbcollab.com/recording/82b06f1a4cad492eb0c8e9121812b16d


Assignment Code: PHASE-1-SOLO
Prerequisite: Phase 0 (README dry run) complete
Time Estimate: 2–3 class sessions (~4–5 hours total)
Type: Individual
Tools Required: Python 3, Flask, Git, GitHub, Claude Code (API key from instructor)

What This Is

You just proved (in Phase 0) that you can move a card across the board, create a branch, open a PR, get a review, and merge. You know the mechanics of the Sacred Workflow.

Now you’re going to do it for real — with actual code, an actual user story, and an AI pair programmer.

Everyone does the same story. This is deliberate. You can compare approaches, help each other debug, and review each other’s code without needing to understand five different projects. The process is the lesson. The login page is just the vehicle.

The Setup

You’ll receive a starter repo (via GitHub Classroom or a template fork — your instructor will provide the link). It contains a working Flask skeleton and everything you need to get started.

That repo is at : https://github.com/norrisaftcc/sample-solo-burger


What’s in the starter repo
phase1-flask-login/
├── app.py                    # Working Flask app (returns "Hello World")
├── templates/
│   └── base.html             # Minimal HTML boilerplate
├── static/
│   └── style.css             # Empty — yours to use if you want
├── requirements.txt          # Flask pinned
├── CLAUDE.md                 # Pre-written project context for Claude Code
├── README.md                 # How to install and run
├── .gitignore                # Python defaults
└── .github/
    └── pull_request_template.md   # PR template

Getting it running
# Clone your copy of the repo
git clone https://github.com/norrisaftcc/sample-solo-burger
cd phase1-flask-login

# Create a virtual environment (recommended)
python -m venv venv
source venv/bin/activate        # macOS/Linux
# venv\Scripts\activate         # Windows

# Install dependencies
pip install -r requirements.txt

# Run the app
python app.py


Visit http://localhost:5000 in your browser. You should see “Hello World.” If you do, you’re ready.

If you don’t, stop here and troubleshoot before moving on. See the Troubleshooting section at the bottom of this document.

The User Story

This is the work you’re delivering. Read it carefully — every line matters.

┌──────────────────────────────────────────────────────────────────┐
│                                                                  │
│  USER STORY #1: Basic Login Page                                 │
│                                                                  │
│  As a user, I want to log in with a username and password        │
│  so that I can access my account dashboard.                      │
│                                                                  │
│  ACCEPTANCE CRITERIA:                                            │
│                                                                  │
│  1. A /login route exists and renders a form with fields         │
│     for username and password.                                   │
│                                                                  │
│  2. Submitting valid credentials redirects to /dashboard.        │
│                                                                  │
│  3. Submitting invalid credentials re-renders the login          │
│     page with a visible error message.                           │
│                                                                  │
│  4. The /dashboard page displays "Welcome, [username]".          │
│                                                                  │
│  5. The /dashboard page redirects to /login if the user          │
│     is not authenticated (no session).                           │
│                                                                  │
│  6. A logout link or button on /dashboard clears the             │
│     session and returns to /login.                               │
│                                                                  │
│  TECHNICAL CONSTRAINTS:                                          │
│                                                                  │
│  - Use Flask and Jinja2 templates.                               │
│  - Use Flask's built-in session for authentication state.        │
│  - Hardcoded credentials are fine (no database required).        │
│  - Minimal styling is acceptable. It doesn't need to be pretty.  │
│  - It needs to WORK.                                             │
│                                                                  │
└──────────────────────────────────────────────────────────────────┘


These six acceptance criteria are your definition of done. When all six are true, the story is complete.

The Process: Step by Step

You already know the Sacred Workflow from Phase 0. Now you’re running it with real code. Each step below maps to a workflow step and tells you exactly what to do.

Step 1: Claim the Issue

Your starter repo comes with Issue #1 pre-created, containing the user story above.

Go to the Issues tab in your GitHub repo.
Find Issue #1 (“Basic Login Page”).
Assign it to yourself.
On your project board, move the card from Backlog to In Progress.

Why this matters: Before you write code, you’re telling your team (and your future self) what you’re working on. This is a habit. Build it now.

Step 2: Create Your Branch
# Make sure you're on main and it's up to date
git checkout main
git pull origin main

# Create your feature branch
git checkout -b feature/1-login-page


The branch name includes the issue number (1) and a short description (login-page). This is the naming convention. Use it every time.

Step 3: Open a Draft Pull Request

Push your branch immediately — even though you haven’t written any code yet.

git push -u origin feature/1-login-page


Now go to GitHub and open a Draft Pull Request:

Title: WIP: Basic Login Page
Body: Write Closes #1 on its own line. (This is magic syntax — when the PR merges, it automatically closes the issue.)

You now have a place to take notes, a visible signal that you’re working, and a link to share if you need help.

Why a draft first? Because the PR is your workspace, not just your submission. You’ll update it as you go. Starting it now means you never forget to create it later.

Instructor notes: Is this the correct order to do this? Can you make a draft before adding code?

Step 4: Plan With Claude Code

NOTES: Install the "Claude Code for VS Code" plugin -- it will set Claude Code up for you. 

But, before you do, run this command (use the full API key from Module Five):

export ANTHROPIC_API_KEY=sk-ant-...

curl -fsSL https://claude.ai/install.sh | bash
claude

(Press enter after each line. Replace the API Key with the full key!)

Open Claude Code in your repo directory. Start the session like this:

I'm working on Issue #1: "As a user, I want to log in with a username
and password so that I can access my account dashboard."

Here are the acceptance criteria:
1. /login route renders a form with username and password fields
2. Valid credentials redirect to /dashboard
3. Invalid credentials show an error message on the login page
4. /dashboard displays "Welcome, [username]"
5. /dashboard redirects to /login if not authenticated
6. Logout clears the session and returns to /login

Please read CLAUDE.md for project context, then help me plan the
implementation before we write any code.


Claude Code will read your CLAUDE.md, understand the Flask setup, and propose a plan. Read the plan. Does it make sense? Does it cover all six acceptance criteria? Ask questions if anything is unclear.

Do not start coding until you understand the plan.

This is the most important habit in AI-assisted development. The students who struggle are the ones who skip planning and let the AI generate code they don’t understand. Slow down here. It saves time later.

Step 5: Build Iteratively

Work through the implementation in small pieces. Each piece should be one conversation turn with Claude Code, followed by you reviewing and testing what it produced.

Here is a recommended sequence. You don’t have to follow it exactly, but each step should be small enough that you can understand and test it before moving on.

Piece 1 — The login template and route (GET)

Ask Claude Code to create the /login route and a login.html template with a form. Test it:

Visit http://localhost:5000/login
→ Do you see a form with username and password fields?
→ Does the form have a submit button?


If yes, commit:

git add -A
git commit -m "feat(auth): add login route and template

Renders login form with username and password fields.

Refs #1"
git push


Piece 2 — Form handling and credential check (POST)

Ask Claude Code to handle the form submission. For now, hardcode a username and password (e.g., admin / password123). On valid credentials, set a session variable and redirect to /dashboard. On invalid credentials, re-render the login page with an error.

Test it:

Submit the form with wrong credentials.
→ Do you see an error message?

Submit the form with correct credentials.
→ Are you redirected to /dashboard?
  (It's OK if /dashboard doesn't exist yet — a 404 is expected.)


Commit.

Piece 3 — The dashboard page

Ask Claude Code to create the /dashboard route and template. It should display “Welcome, [username]” using the session data. If there’s no session (user isn’t logged in), redirect to /login.

Test it:

Log in with valid credentials.
→ Do you see "Welcome, admin" (or whatever username)?

Open a new browser tab and go directly to /dashboard without logging in.
→ Are you redirected to /login?


Commit.

Piece 4 — Logout

Ask Claude Code to add a logout mechanism — a link or button on the dashboard that clears the session and redirects to /login.

Test it:

Log in. See the dashboard. Click logout.
→ Are you back at /login?

Try to visit /dashboard again.
→ Are you redirected to /login? (Session should be cleared.)


Commit.

Piece 5 — Review all acceptance criteria

Go through the six criteria one by one. Check each off:

[ ] /login renders a form with username and password fields
[ ] Valid credentials redirect to /dashboard
[ ] Invalid credentials show an error message
[ ] /dashboard displays "Welcome, [username]"
[ ] /dashboard redirects to /login if not authenticated
[ ] Logout clears session and returns to /login


If any fail, fix them. Then commit.

Step 6: Finish the Pull Request

Go to GitHub and convert your Draft PR into a real PR.

Update the title: “Add basic login page with session auth”

Fill out the PR template:

## Summary
Implements Issue #1: basic login page with session-based authentication.
Users can log in with hardcoded credentials, see a personalized dashboard,
and log out.

## Related Issue
Closes #1

## Testing Done
- [x] /login renders form with username and password fields
- [x] Valid credentials redirect to /dashboard
- [x] Invalid credentials show error message on login page
- [x] /dashboard shows "Welcome, [username]"
- [x] /dashboard redirects to /login when not authenticated
- [x] Logout clears session and redirects to /login

## Screenshots
[Include a screenshot of the login page and the dashboard, if possible]

Step 7: Get a Review and Merge

Request a review from a classmate. While you wait, go review someone else’s PR — this counts toward the collaboration minimum and it’s good practice.

When reviewing someone else’s PR:

Pull their branch and run the app locally.
Test all six acceptance criteria.
Look at the code. Does it make sense? Are there obvious issues?
Leave at least one substantive comment. “LGTM” alone doesn’t count.
You can use Claude Code to help: “Please review this code for security issues and consistency.”

Once your PR is approved:

Squash and merge to main.
Delete the branch (GitHub offers this button after merge).
Move the card on your project board to Done.
The Rubric

Your Phase 1 submission is graded on three dimensions: workflow, functionality, and comprehension. All three matter.

Workflow (40 points)
Criterion	Full Credit	Partial Credit	No Credit
Issue claimed and tracked (5 pts)	Issue assigned to you, card moved through all board columns	Issue exists but card didn’t move through all columns	No issue or no board movement
Branch naming (5 pts)	feature/1-login-page or equivalent following convention	Branch exists but doesn’t follow naming convention	No feature branch (worked on main)
Draft PR opened early (5 pts)	Draft PR created before or alongside first code commit	PR created but not as a draft / created after most code was written	No PR until the end
Commit history (10 pts)	3+ meaningful commits showing iterative progress, good messages using type(scope): subject format	2 commits or messages that are vague but present	1 giant commit or no meaningful messages
PR description (5 pts)	Summary, “Closes #1”, testing checklist filled out	PR exists with some description	Empty or minimal PR description
Code review given (5 pts)	Reviewed a classmate’s PR with at least one substantive comment	Review exists but is only “LGTM” or equivalent	No review given
Code review received (5 pts)	PR was reviewed by a classmate before merging	PR was merged without review	N/A
Functionality (35 points)

Instructor or peer verifies by cloning the repo, running the app, and checking each criterion.

Criterion	Points	Pass	Fail
/login renders a form with username and password fields	5	Form displays correctly	Route missing or form broken
Valid credentials redirect to /dashboard	7	Redirect works with correct credentials	Doesn’t redirect or crashes
Invalid credentials show an error message	5	Error message visible on the login page	No feedback on bad credentials
/dashboard displays “Welcome, [username]”	6	Personalized greeting using session data	Generic or missing greeting
/dashboard redirects to /login when not authenticated	7	Direct navigation to /dashboard without login redirects properly	Dashboard accessible without login
Logout clears session and returns to /login	5	Session cleared, redirect works, can’t re-access dashboard	Logout missing or session persists
Comprehension (25 points)

You will answer two of the following questions, either verbally (with instructor during class) or in a short written reflection submitted with your PR. Your instructor will specify which format.

Each answer should demonstrate that you understand your own code — not that you can recite a definition.

Question	What a good answer shows
“What does @app.route('/login', methods=['GET', 'POST']) do? Why both methods?”	Understands that GET renders the form, POST processes the submission, and the decorator maps the URL to the function.
“How does Flask’s session work in your implementation? Where is the data stored?”	Can explain that session is a signed cookie (by default), that secret_key enables it, and can point to where they set and read session values in their code.
“What would you change to support real user accounts instead of hardcoded credentials?”	Can describe adding a database, storing hashed passwords, and querying instead of comparing strings. Doesn’t need to implement it — just reason about it.
“Show me one thing Claude Code suggested that you changed or questioned. Why?”	Can identify a specific suggestion, explain what was wrong or suboptimal about it, and describe what they did instead. Demonstrates critical engagement with AI output.
“Walk me through what happens from the moment a user clicks ‘Login’ to when they see the dashboard.”	Can trace the HTTP request → route handler → credential check → session write → redirect → dashboard route → session read → template render. Doesn’t need to be this precise, but should show understanding of the flow.

Scoring:

Level	Points (per question)	Description
Strong	11–12.5	Clear, specific, references their actual code. Could explain it to a teammate.
Adequate	7–10	Generally correct but vague. Understands the concept but can’t point to specifics.
Weak	3–6	Partially correct. Significant gaps or confusion.
Insufficient	0–2	Cannot explain their own code. Suggests copy-paste without engagement.
Grade Summary
Component	Points
Workflow	40
Functionality	35
Comprehension	25
Total	100
What the scores mean
Score	Interpretation	What happens next
85–100	You’ve got it. The workflow is solid, the code works, you understand it.	Proceed to Phase 2 with your team.
70–84	Almost there. Minor gaps in workflow or understanding.	Brief check-in with instructor, then proceed to Phase 2.
55–69	Workflow or comprehension needs work.	Targeted redo: fix the specific gap, resubmit the affected component.
Below 55	Significant gaps.	Meet with instructor. We’ll figure out what’s blocking you and get you through it.

This is not a gotcha. The point is to make sure you can do the thing before we add team coordination on top of it. If you’re struggling, that’s information — not a punishment.

Troubleshooting
=== BEGIN TRANSMISSION ===

So something's broken. Don't panic. Here's the field guide.

=== SYMPTOM ===  "ModuleNotFoundError: No module named 'flask'"
FIX: You're not in your virtual environment.
     Run: source venv/bin/activate   (macOS/Linux)
     Run: venv\Scripts\activate      (Windows)
     Then: pip install -r requirements.txt

=== SYMPTOM ===  "Address already in use" when running app.py
FIX: Another process is using port 5000.
     Kill it:  lsof -i :5000  (find the PID, then kill it)
     Or run on a different port:
     flask run --port 5001

=== SYMPTOM ===  Login form submits but nothing happens / page just refreshes
FIX: Check your form's action and method attributes:
     <form method="POST" action="/login">
     Missing method="POST" means it sends a GET request, which just
     re-renders the page.

=== SYMPTOM ===  "RuntimeError: The session is unavailable because no
                  secret key was set."
FIX: Your app needs a secret key for sessions.
     In app.py:  app.secret_key = 'change-this-in-production'
     The starter code has this already. If you deleted it, put it back.

=== SYMPTOM ===  Dashboard accessible even after logout
FIX: Your logout route isn't clearing the session properly.
     Make sure you're calling session.clear() or session.pop('username')
     AND redirecting to /login afterward.

=== SYMPTOM ===  "I broke everything and I don't know what I changed"
FIX: Git has your back.
     git diff              ← see what changed
     git stash             ← stash changes, go back to last commit
     git stash pop         ← bring changes back when you're ready

     If it's truly hosed:
     git checkout -- .     ← NUCLEAR OPTION: reverts all changes to
                             last commit. You will lose uncommitted work.
     This is why we commit early and commit often.

=== SYMPTOM ===  "My classmate's app looks completely different from mine"
DIAGNOSIS: This is fine. Same story, different implementations. There are
           many correct ways to build a Flask login page. Compare
           approaches — you'll both learn something.

=== SYMPTOM ===  "Claude Code generated something I don't understand"
FIX: Ask it.
     "Explain what this code does line by line."
     "Why did you use session.get('username') instead of
      session['username']?"
     If the explanation doesn't make sense, ask it to simplify.
     If it STILL doesn't make sense, ask a classmate or the
     instructor. There's no shame in that.

=== GENERAL ADVICE ===

The 30-minute rule: if you've been stuck for 30 minutes without
making progress, reach out. Claude Code first, then a classmate,
then the instructor. Being stuck in silence is the only real mistake.

=== END TRANSMISSION ===

Starter File Reference

For convenience, here’s what the key starter files contain. Your actual repo will have these pre-populated.

app.py

from flask import Flask

app = Flask(__name__)
app.secret_key = 'change-this-in-production'  # Required for session support

@app.route('/')
def index():
    return "Hello, World! Your Flask app is running."

if __name__ == '__main__':
    app.run(debug=True)

templates/base.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}Flask Login Exercise{% endblock %}</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
    {% block content %}{% endblock %}
</body>
</html>

CLAUDE.md

# CLAUDE.md

## Project
Flask Login Exercise — A simple login page for learning the Sacred Workflow.

## Tech Stack
- Python 3.x with Flask
- Jinja2 templates (in templates/ directory)
- Flask built-in sessions (cookie-based, signed with secret_key)
- No database — hardcoded credentials for this exercise
- Static files in static/ directory

## Architecture
Single-file Flask app (app.py) with Jinja2 templates extending base.html.
Authentication via Flask session. No ORM, no database, no external auth.

## Conventions
- Routes defined in app.py
- Templates in templates/, extending base.html
- Static assets in static/
- Follow PEP 8 style
- Commit messages: type(scope): subject — e.g., feat(auth): add login route

## Current Task
Issue #1: Implement login page with session-based authentication.

Acceptance criteria:
1. /login route renders form with username and password fields
2. Valid credentials redirect to /dashboard
3. Invalid credentials show error message on login page
4. /dashboard displays "Welcome, [username]"
5. /dashboard redirects to /login if not authenticated
6. Logout clears session and returns to /login

Hardcoded credentials: username = "admin", password = "password123"

## How to Run
pip install -r requirements.txt
python app.py
Visit http://localhost:5000

## How to Test
Manual testing against the six acceptance criteria listed above.

.github/pull_request_template.md
## Summary
[What this PR does in 1-2 sentences]

## Related Issue
Closes #[issue number]

## Testing Done
- [ ] /login renders form with username and password fields
- [ ] Valid credentials redirect to /dashboard
- [ ] Invalid credentials show error message
- [ ] /dashboard displays "Welcome, [username]"
- [ ] /dashboard redirects to /login if not authenticated
- [ ] Logout clears session and returns to /login

## Screenshots (if applicable)
[Login page and dashboard screenshots]

## Notes
[Anything the reviewer should know]

Checklist Before Submitting

Use this as your final check. Every box should be checked.

WORKFLOW
[ ] Issue #1 is assigned to me
[ ] Board card moved through all columns (Backlog → Sprint → In Progress → Review → Done)
[ ] Feature branch named feature/1-login-page (or similar with issue #)
[ ] Draft PR was opened before or alongside first code commit
[ ] At least 3 commits with meaningful messages
[ ] PR description filled out with summary, "Closes #1", and testing checklist
[ ] I reviewed a classmate's PR with at least one substantive comment
[ ] My PR was reviewed by a classmate
[ ] Squash-merged to main
[ ] Branch deleted after merge

FUNCTIONALITY
[ ] /login renders a form
[ ] Valid login redirects to /dashboard
[ ] Invalid login shows an error
[ ] Dashboard says "Welcome, [username]"
[ ] Dashboard redirects if not logged in
[ ] Logout works and clears session

COMPREHENSION
[ ] I can answer 2 of the 5 comprehension questions about my own code
[ ] I did not copy-paste code I cannot explain

A Final Note

This exercise is not about building a beautiful login page. Nobody’s hiring you based on this login page.

This exercise is about proving to yourself that you can take a user story, plan it, build it with an AI pair programmer, test it against acceptance criteria, and ship it through the Sacred Workflow — all while understanding what you built.

Once you can do this with a login page, you can do it with anything. That’s the point.

Ship the burger. Move on to the lunch rush.