# DataBuddy - Technical Implementation (Parts 5-6)

## ABOUT THIS DOCUMENT

This document is **Part 2 of 2** of the DataBuddy brief for Spring 2026 capstone projects.

**Companion Document:** `DataBuddy_base.md` contains Parts 1-4 (Creative Brief, Product Deep Dive, User Research, Strategic Guidance)

**Relationship to CogniCalibrate™ Technical Brief:**
This document is structured as a **delta** from the CogniCalibrate™ technical brief. Rather than repeating the full challenge engine architecture (which is identical), this document focuses on what changes when the platform serves a K-12 audience instead of corporate knowledge workers. Read the CogniCalibrate™ technical brief first for the complete engine specification, then read this document for the K-12 modifications.

**What's the Same (see CogniCalibrate_technical.md):**
- Adaptive difficulty engine logic
- Challenge generation algorithms for all six modes
- Core database tables: `sessions`, `responses`, `user_difficulty_tiers`, `difficulty_tier_parameters`
- WebSocket multiplayer architecture
- Achievement system mechanics
- Analytics aggregation pipeline (`daily_stats`, `weekly_stats`)

**What's Different (covered in this document):**
- Account model (class codes, no-email accounts, teacher/parent roles)
- Three-audience dashboard architecture
- COPPA compliance requirements
- Classroom management features
- Standards alignment mapping
- Kid-friendly notification language
- Modified leaderboard strategy
- Additional database tables for education-specific entities
- Modified API endpoints for teacher and parent access

---

## PART 5: TECHNICAL IMPLEMENTATION — K-12 MODIFICATIONS

### 5.1 Account Architecture

The most significant technical difference between CogniCalibrate and DataBuddy is the account model. CogniCalibrate has one user type. DataBuddy has four.

#### User Roles

```
┌──────────────────────────────────────────────────┐
│                  ACCOUNT TYPES                   │
├──────────────────────────────────────────────────┤
│                                                  │
│  STUDENT (age 7-14)                              │
│  ├── Created via class code (no email required)  │
│  ├── Username + optional password                │
│  ├── Sees: own progress, achievements, games     │
│  ├── Cannot: see other students' data            │
│  └── Data: governed by COPPA                     │
│                                                  │
│  TEACHER                                         │
│  ├── Created via email registration              │
│  ├── Creates classrooms, generates join codes    │
│  ├── Sees: all student data in their classrooms  │
│  ├── Can: assign modes, set time limits, export  │
│  └── Data: standard privacy policy               │
│                                                  │
│  PARENT                                          │
│  ├── Created via email + link from teacher/child │
│  ├── Sees: linked child's progress only          │
│  ├── Can: view progress, receive weekly emails   │
│  └── Data: standard privacy policy               │
│                                                  │
│  ADMIN (school-level, future)                    │
│  ├── Manages multiple teacher accounts           │
│  └── Sees: school-wide aggregate data            │
│                                                  │
└──────────────────────────────────────────────────┘
```

#### Student Account Creation Flow

```
Teacher creates classroom:
  POST /api/v1/classrooms
  → Returns classroom_id + join_code (e.g., "MATH-4B-STARS")

Student joins (in class, on Chromebook):
  1. Navigate to databuddy.app/join
  2. Enter class code: "MATH-4B-STARS"
  3. Enter display name: "Sofia R."
  4. Choose an avatar (pre-made options, no uploads)
  5. Optionally set a simple password (teacher can disable for K-2)
  
  → Student account created
  → Linked to classroom
  → NO email address collected
  → NO personal information beyond display name

Teacher approves (optional):
  Teacher sees: "Sofia R. joined your class"
  Teacher can: rename, remove, or approve the student
```

**Why This Matters Technically:** Traditional auth flows (email + password + email verification) don't work for 8-year-olds. Class code join eliminates the need for individual account provisioning, parental email addresses for account creation, and password recovery flows — all of which are friction points that kill classroom adoption.

---

### 5.2 Database Schema — K-12 Extensions

These tables extend the CogniCalibrate™ base schema. All core tables (`sessions`, `responses`, `user_difficulty_tiers`, `difficulty_tier_parameters`, `achievements`, `user_achievements`, `daily_stats`, `weekly_stats`, `multiplayer_sessions`, `multiplayer_participants`, `number_guesser_games`) remain identical.

#### Modified Users Table

```sql
-- Replaces CogniCalibrate's users table with role-aware version

TABLE users (
  user_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  
  -- Role
  role VARCHAR(15) NOT NULL,               -- 'student' | 'teacher' | 'parent' | 'admin'
  
  -- Authentication (varies by role)
  username VARCHAR(50),                    -- Required for students (display name)
  email VARCHAR(255),                      -- Required for teacher/parent, NULL for students
  password_hash VARCHAR(255),              -- Optional for young students (K-2)
  
  -- Profile
  display_name VARCHAR(100) NOT NULL,
  avatar_id VARCHAR(30),                   -- Pre-made avatar selection (no uploads)
  grade_level INT,                         -- Student only: 2-8
  
  -- Preferences
  preferred_theme VARCHAR(20) DEFAULT 'light',  -- Light default for kids
  sound_effects BOOLEAN DEFAULT TRUE,            -- Kid-appropriate sounds
  
  -- Streak Tracking (students only)
  current_streak INT DEFAULT 0,
  longest_streak INT DEFAULT 0,
  last_active_date DATE,
  
  -- COPPA Compliance
  is_under_13 BOOLEAN DEFAULT TRUE,        -- Students assumed under 13
  parental_consent_status VARCHAR(20),     -- 'pending' | 'granted' | 'not_required'
  parental_consent_date TIMESTAMP,
  data_retention_policy VARCHAR(20) DEFAULT 'school_year',
  
  -- Timestamps
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW(),
  last_login_at TIMESTAMP
);

-- Students don't have emails — can't use email as unique key
-- Username uniqueness is scoped to classroom (two "Sofia" in different classes OK)
CREATE UNIQUE INDEX idx_users_email ON users(email) WHERE email IS NOT NULL;
```

#### Classroom Management Tables

```sql
TABLE classrooms (
  classroom_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  teacher_user_id UUID NOT NULL REFERENCES users(user_id),
  
  -- Classroom Identity
  name VARCHAR(100) NOT NULL,              -- "Mrs. Rodriguez - 4th Grade Math"
  join_code VARCHAR(20) UNIQUE NOT NULL,   -- "MATH-4B-STARS"
  grade_level INT,                         -- 2-8
  
  -- Settings
  is_active BOOLEAN DEFAULT TRUE,
  max_students INT DEFAULT 35,
  
  -- Game Mode Controls
  allowed_modes JSONB DEFAULT '["check_it", "mystery_number", "speed_round", 
                                "fill_it_in", "hot_potato", "last_one_standing"]',
  multiplayer_enabled BOOLEAN DEFAULT TRUE,
  
  -- Practice Controls
  daily_practice_goal_minutes INT DEFAULT 10,
  max_session_minutes INT DEFAULT 20,      -- Prevent excessive play during class
  
  -- Display Controls
  show_leaderboard BOOLEAN DEFAULT TRUE,
  leaderboard_type VARCHAR(20) DEFAULT 'growth',  -- 'growth' | 'streak' | 'hidden'
  show_class_averages BOOLEAN DEFAULT FALSE,       -- Teacher can enable/disable
  
  -- Term
  school_year VARCHAR(10),                 -- "2025-2026"
  term VARCHAR(20),                        -- "Fall" | "Spring" | "Full Year"
  
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

TABLE classroom_students (
  classroom_id UUID REFERENCES classrooms(classroom_id),
  student_user_id UUID REFERENCES users(user_id),
  
  -- Enrollment
  joined_at TIMESTAMP DEFAULT NOW(),
  status VARCHAR(20) DEFAULT 'active',     -- 'active' | 'removed' | 'transferred'
  
  -- Teacher can override display name
  display_name_override VARCHAR(100),      -- NULL = use student's chosen name
  
  PRIMARY KEY (classroom_id, student_user_id)
);

-- A student can be in multiple classrooms (different teachers/subjects)
-- Example: Sofia is in Mrs. Rodriguez's Math AND Mr. Chen's Science
```

#### Parent Link Tables

```sql
TABLE parent_student_links (
  parent_user_id UUID REFERENCES users(user_id),
  student_user_id UUID REFERENCES users(user_id),
  
  -- Verification
  link_code VARCHAR(20),                   -- Teacher or student generates link code
  verified BOOLEAN DEFAULT FALSE,          -- Teacher approves parent link
  verified_by UUID REFERENCES users(user_id),  -- Which teacher approved
  
  -- Notification Preferences
  weekly_email_enabled BOOLEAN DEFAULT TRUE,
  weekly_email_day VARCHAR(10) DEFAULT 'sunday',
  alert_on_streak_break BOOLEAN DEFAULT FALSE,
  alert_on_struggle BOOLEAN DEFAULT FALSE,  -- When accuracy drops significantly
  
  linked_at TIMESTAMP DEFAULT NOW(),
  
  PRIMARY KEY (parent_user_id, student_user_id)
);
```

#### Standards Alignment Table

```sql
TABLE math_standards (
  standard_id VARCHAR(20) PRIMARY KEY,     -- "3.OA.A.1" (CCSSM format)
  
  -- Standard Details
  domain VARCHAR(50) NOT NULL,             -- "Operations & Algebraic Thinking"
  cluster VARCHAR(200),                    -- "Represent and solve problems..."
  description TEXT NOT NULL,               -- Full standard text
  grade_level INT NOT NULL,                -- 3 (for 3rd grade standard)
  
  -- Friendly Description
  student_friendly TEXT,                   -- "Understand what multiplication means"
  parent_friendly TEXT                     -- "Your child is learning multiplication concepts"
);

TABLE tier_standard_alignment (
  challenge_mode VARCHAR(30) NOT NULL,
  difficulty_tier INT NOT NULL,
  standard_id VARCHAR(20) REFERENCES math_standards(standard_id),
  
  -- How strongly this tier aligns to this standard
  alignment_strength VARCHAR(10),          -- 'primary' | 'supporting' | 'partial'
  
  PRIMARY KEY (challenge_mode, difficulty_tier, standard_id)
);

-- Example: Speed Round Tier 3 (multiplication facts) aligns to:
-- INSERT INTO tier_standard_alignment VALUES
--   ('speed_round', 3, '3.OA.C.7', 'primary'),    -- Fluently multiply within 100
--   ('speed_round', 3, '3.OA.A.1', 'supporting'),  -- Interpret products of whole numbers
--   ('speed_round', 4, '4.NBT.B.5', 'primary');    -- Multiply multi-digit by one-digit
```

#### Teacher Assignment Table

```sql
TABLE assignments (
  assignment_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  classroom_id UUID NOT NULL REFERENCES classrooms(classroom_id),
  teacher_user_id UUID NOT NULL REFERENCES users(user_id),
  
  -- What to practice
  challenge_modes JSONB,                   -- NULL = all modes, or ["speed_round", "fill_it_in"]
  minimum_minutes INT DEFAULT 10,
  minimum_sessions INT,                    -- Alternative: at least N sessions
  
  -- When
  assigned_date DATE NOT NULL,
  due_date DATE,
  
  -- Scope
  assign_to VARCHAR(20) DEFAULT 'class',   -- 'class' | 'individual'
  individual_student_ids UUID[],           -- If assign_to = 'individual'
  
  -- Status
  is_active BOOLEAN DEFAULT TRUE,
  
  created_at TIMESTAMP DEFAULT NOW()
);

TABLE assignment_completion (
  assignment_id UUID REFERENCES assignments(assignment_id),
  student_user_id UUID REFERENCES users(user_id),
  
  -- Progress
  minutes_practiced INT DEFAULT 0,
  sessions_completed INT DEFAULT 0,
  is_complete BOOLEAN DEFAULT FALSE,
  completed_at TIMESTAMP,
  
  PRIMARY KEY (assignment_id, student_user_id)
);
```

---

### 5.3 Application Configuration — K-12 Modifications

```yaml
# databuddy_config.yml
# DataBuddy Application Configuration
# Changes from CogniCalibrate are marked with # DELTA

app_metadata:
  name: "DataBuddy"
  tagline: "Math practice that feels like play"
  version: "1.0.0"
  platform_base: "cognitive_assessment_engine_v1"    # Same engine!
  target_audience: "k12_students"                    # DELTA
  deployment_mode: "web_application"

branding:
  colors:
    # DELTA: Light theme default (kids expect bright interfaces)
    background: "#f8f9fc"
    surface: "#ffffff"
    surface_elevated: "#f0f2f8"
    primary: "#4a6cf7"           # Friendly blue
    primary_hover: "#3b5de7"
    highlight: "#ff6b6b"         # Warm red (not aggressive)
    highlight_hover: "#ff5252"

    # DELTA: Kid-friendly functional colors
    success: "#51cf66"           # Bright green (correct!)
    warning: "#fcc419"           # Sunny yellow
    error: "#ff6b6b"             # Soft red (not scary)
    info: "#74c0fc"              # Sky blue

    text_primary: "#2b2d42"      # Dark navy (readable on white)
    text_secondary: "#6c757d"
    text_muted: "#adb5bd"

    # DELTA: Grade-level color coding (for teacher dashboard)
    grade_colors:
      grade_2: "#ff6b6b"
      grade_3: "#ffa94d"
      grade_4: "#fcc419"
      grade_5: "#51cf66"
      grade_6: "#22b8cf"
      grade_7: "#4a6cf7"
      grade_8: "#9775fa"

    # Mode colors (same modes, friendlier palette)
    mode_colors:
      check_it: "#74c0fc"        # Sky blue
      mystery_number: "#b197fc"  # Lavender
      speed_round: "#fcc419"     # Sunny gold
      fill_it_in: "#51cf66"      # Bright green
      hot_potato: "#ff6b6b"      # Warm red
      last_one_standing: "#9775fa" # Purple

  typography:
    # DELTA: Rounder, friendlier fonts
    font_primary: "Nunito"              # Rounded, friendly
    font_display: "Fredoka One"         # Playful headings
    font_monospace: "Fira Code"         # For number displays

challenge_modes:
  # DELTA: All kid-friendly names and descriptions
  check_it:
    display_name: "Check It!"
    description: "Is the answer right or wrong? You decide!"
    icon: "✓"
    cogni_equivalent: "answer_checker"    # Maps to same engine
    default_challenges_per_session: 15
    adaptive: true

  mystery_number:
    display_name: "Mystery Number"
    description: "Can you find the secret number?"
    icon: "🔍"
    cogni_equivalent: "number_guesser"
    default_rounds_per_session: 5
    adaptive: true

  speed_round:
    display_name: "Speed Round"
    description: "How many can you get in 60 seconds?"
    icon: "⚡"
    cogni_equivalent: "electro_flash"
    default_round_duration_seconds: 60
    adaptive: true

  fill_it_in:
    display_name: "Fill It In"
    description: "Find the missing piece of the equation!"
    icon: "🧩"
    cogni_equivalent: "missing_number"
    default_challenges_per_session: 10
    adaptive: true

  hot_potato:
    display_name: "Hot Potato"
    description: "Don't hold the question when time runs out!"
    icon: "🥔"
    cogni_equivalent: "wipe_out"
    min_players: 2
    max_players: 6
    # DELTA: Teacher controls multiplayer availability
    requires_teacher_enable: true

  last_one_standing:
    display_name: "Last One Standing"
    description: "Subtract smart — don't be the one who hits zero!"
    icon: "🏆"
    cogni_equivalent: "force_out"
    min_players: 2
    max_players: 2
    requires_teacher_enable: true

# DELTA: Classroom management settings
classroom:
  max_students_per_class: 35
  join_code_format: "WORD-WORD-WORD"        # e.g., "STAR-MATH-BLUE" (easy to read aloud)
  join_code_expiry_days: null                # Codes don't expire during school year
  require_teacher_approval: false            # Students auto-join (teacher can remove)
  allow_student_passwords: true              # Optional for K-2
  password_required_grade: 4                 # Grade 4+ should have passwords
  max_classrooms_per_teacher: 10

# DELTA: COPPA compliance settings
coppa:
  enabled: true
  minimum_data_collection: true
  no_email_under_13: true
  no_behavioral_advertising: true
  parental_consent_required: true
  data_deletion_on_request: true
  data_retention_max_years: 3               # After student leaves, data retained max 3 years
  privacy_policy_child_friendly: true       # Separate, simpler privacy policy for kids

notifications:
  # DELTA: Kid-friendly language throughout
  session_complete:
    templates_by_accuracy:
      high:    # >85%
        - "Amazing! {correct} out of {total}! You're on fire! 🔥"
        - "Wow, {accuracy}% accuracy! That's your best yet!"
        - "Crushed it! {correct} right! 💪"
      medium:  # 60-85%
        - "Nice work! {correct} out of {total}! Keep it up! 🎯"
        - "Good practice! You got {correct} right today."
        - "Solid session! {correct} out of {total} — you're improving!"
      low:     # <60%
        - "Good effort! You got {correct} right. Every practice makes you stronger! 💪"
        - "Tough round! {correct} out of {total} — but you showed up, and that counts!"
        - "Keep going! {correct} right today. You'll get more tomorrow!"

  # DELTA: Never guilt or shame
  streak_messages:
    active: "Day {count} of your practice streak! 🌟"
    broken: "Welcome back! Ready to start a new streak?"    # NOT "you broke your streak"
    new_record: "NEW RECORD! {count} days in a row! 🏆"

  difficulty_promoted:
    template: "Level up! 🎮 You're getting harder problems now because you're ready!"

  difficulty_demoted:
    template: "Let's practice these a bit more — you've got this!"    # NOT "demoted"

  # DELTA: No AlgoCratic flavor. Friendly loading messages instead.
  loading_messages:
    - "Getting your math problems ready..."
    - "DataBuddy is thinking of some good ones..."
    - "Almost ready! 🧮"
    - "Loading your challenge..."
    - "Let's do some math! 🚀"

# DELTA: Standards alignment
standards:
  framework: "CCSSM"                        # Common Core State Standards for Mathematics
  grade_range: [2, 8]
  alignment_visible_to: ["teacher", "parent"]  # Students see skill names, not standard codes
  
  # Student-friendly skill names (mapped from CCSSM)
  skill_labels:
    "addition_basic": "Adding Numbers"
    "subtraction_basic": "Subtracting Numbers"
    "multiplication_facts": "Times Tables"
    "division_facts": "Division Facts"
    "multi_digit_addition": "Big Number Addition"
    "multi_digit_subtraction": "Big Number Subtraction"
    "multi_digit_multiplication": "Big Number Multiplication"
    "long_division": "Long Division"
    "fractions_basic": "Fractions"
    "order_of_operations": "Order of Operations"
    "estimation": "Estimation"

limits:
  # DELTA: Tighter limits for kids + classroom context
  max_sessions_per_day: 20                  # Prevent excessive play
  max_session_duration_minutes: 20          # Teacher-configurable
  max_multiplayer_concurrent: 1             # One game at a time for focus
  api_requests_per_minute: 60               # Lower than corporate
```

---

### 5.4 API Endpoints — K-12 Modifications

#### Classroom Management (NEW — not in CogniCalibrate)

```
# Teacher endpoints
POST   /api/v1/classrooms                  # Create classroom, get join code
GET    /api/v1/classrooms                  # List teacher's classrooms
GET    /api/v1/classrooms/{id}             # Classroom details + roster
PUT    /api/v1/classrooms/{id}             # Update settings (modes, limits)
DELETE /api/v1/classrooms/{id}             # Archive classroom

GET    /api/v1/classrooms/{id}/students    # Student roster with status
DELETE /api/v1/classrooms/{id}/students/{sid}  # Remove student

# Student endpoints
POST   /api/v1/classrooms/join             # Join via code + display name
GET    /api/v1/classrooms/mine             # Student's current classrooms
```

**Example: Create Classroom**
```json
POST /api/v1/classrooms
{
  "name": "Mrs. Rodriguez - 4th Grade Math",
  "grade_level": 4,
  "school_year": "2025-2026",
  "daily_practice_goal_minutes": 10,
  "allowed_modes": ["check_it", "mystery_number", "speed_round", "fill_it_in"],
  "multiplayer_enabled": true,
  "leaderboard_type": "growth"
}

Response:
{
  "classroom_id": "uuid-classroom-1",
  "join_code": "STAR-MATH-BLUE",
  "name": "Mrs. Rodriguez - 4th Grade Math",
  "student_count": 0,
  "created_at": "2026-02-15T10:00:00Z"
}
```

**Example: Student Join**
```json
POST /api/v1/classrooms/join
{
  "join_code": "STAR-MATH-BLUE",
  "display_name": "Sofia R.",
  "avatar_id": "robot-purple"
}

Response:
{
  "user_id": "uuid-sofia",
  "display_name": "Sofia R.",
  "classroom_name": "Mrs. Rodriguez - 4th Grade Math",
  "grade_level": 4,
  "message": "Welcome to Mrs. Rodriguez's class! Ready to practice? 🎮"
}
```

#### Teacher Dashboard (NEW)

```
GET    /api/v1/teacher/dashboard                    # Overview: all classrooms
GET    /api/v1/teacher/classrooms/{id}/dashboard    # Single classroom overview
GET    /api/v1/teacher/classrooms/{id}/students/{sid}/detail  # Individual student deep dive
GET    /api/v1/teacher/classrooms/{id}/standards    # Standards alignment report
GET    /api/v1/teacher/classrooms/{id}/export       # Export progress report (CSV/PDF)

POST   /api/v1/teacher/assignments                  # Create practice assignment
GET    /api/v1/teacher/assignments                  # List assignments
GET    /api/v1/teacher/assignments/{id}/progress    # Assignment completion status
```

**Example: Teacher Dashboard Response**
```json
GET /api/v1/teacher/classrooms/uuid-classroom-1/dashboard

{
  "classroom": "Mrs. Rodriguez - 4th Grade Math",
  "student_count": 27,
  "today": {
    "students_practiced": 22,
    "students_not_practiced": 5,
    "average_accuracy": 74.3,
    "average_minutes": 8.2
  },
  "this_week": {
    "average_days_active": 3.8,
    "most_improved_student": { "name": "Sofia R.", "accuracy_change": +18.5 },
    "needs_attention": [
      { "name": "Jayden M.", "reason": "Division accuracy dropped 22%", "student_id": "uuid-jayden" },
      { "name": "Emma L.", "reason": "No activity in 5 days", "student_id": "uuid-emma" }
    ]
  },
  "difficulty_distribution": {
    "tier_1_2": 3,
    "tier_3_4": 14,
    "tier_5_6": 8,
    "tier_7_8": 2
  },
  "skill_heatmap": {
    "addition": { "class_avg_accuracy": 89, "status": "strong" },
    "subtraction": { "class_avg_accuracy": 81, "status": "strong" },
    "multiplication": { "class_avg_accuracy": 68, "status": "developing" },
    "division": { "class_avg_accuracy": 52, "status": "needs_focus" }
  }
}
```

#### Parent Dashboard (NEW)

```
GET    /api/v1/parent/children                     # List linked children
GET    /api/v1/parent/children/{id}/progress       # Child's progress summary
GET    /api/v1/parent/children/{id}/weekly-report  # Weekly email content

POST   /api/v1/parent/link                         # Link to child via code
PUT    /api/v1/parent/preferences                  # Email notification settings
```

**Example: Parent Progress Response**
```json
GET /api/v1/parent/children/uuid-sofia/progress

{
  "child_name": "Sofia R.",
  "grade_level": 4,
  "this_week": {
    "days_practiced": 4,
    "total_minutes": 38,
    "problems_solved": 127,
    "accuracy": 76
  },
  "streak": {
    "current": 8,
    "longest": 12
  },
  "skills": {
    "strong": ["Addition", "Subtraction"],
    "developing": ["Multiplication Facts"],
    "needs_practice": ["Division Facts"]
  },
  "encouragement": "Sofia practiced 4 out of 5 school days this week! Her multiplication is getting stronger — she improved from 62% to 71% accuracy. Encourage her to try Speed Round at home!",
  "comparison_to_grade_level": "Sofia is on track for 4th grade math expectations."
}
```

#### Modified Student Endpoints

The student-facing endpoints are similar to CogniCalibrate's but with restrictions:

```
# Students CANNOT:
# - See other students' individual data
# - Access raw analytics (only student-friendly summary)
# - Modify classroom settings
# - Delete their account (teacher/parent action)

# Students CAN:
GET    /api/v1/me/progress                 # Simplified progress (kid-friendly)
GET    /api/v1/me/achievements             # Badges earned
GET    /api/v1/me/streak                   # Current streak info
GET    /api/v1/me/personal-bests           # Per-mode records
GET    /api/v1/leaderboard/classroom       # Classroom leaderboard (growth-based)

# Game session endpoints are identical to CogniCalibrate
POST   /api/v1/sessions                    # Start session
POST   /api/v1/sessions/{id}/respond       # Submit answer
PUT    /api/v1/sessions/{id}/end           # End session
```

---

## PART 6: SPRINT-READY BACKLOG — K-12 MODIFICATIONS

This backlog is structured as **modifications to the CogniCalibrate backlog**. Stories that are identical to CogniCalibrate are marked "SAME." New stories specific to the K-12 context are marked "NEW." Modified stories are marked "MODIFIED."

### Sprint 0: Foundation (Weeks 1-2)

| ID | User Story | Same/New | Notes |
|----|-----------|----------|-------|
| S0-1 | Team sets up project repository | SAME | |
| S0-2 | Team completes team contract | SAME | |
| S0-3 | Developer can run application locally | SAME | |
| S0-4 | Team has deployed "hello world" | SAME | |
| S0-5 | Database provisioned and connected | SAME | |
| S0-6 | Sprint 1 planning artifacts created | SAME | |

---

### Sprint 1: Core Pipeline (Weeks 3-4)

**Sprint Goal:** A teacher can create a classroom. A student can join via code, play Check It!, and see their score saved.

| ID | User Story | Same/New | Acceptance Criteria |
|----|-----------|----------|---------------------|
| S1-1 | **As a teacher**, I can create an account with email and password | MODIFIED | Teacher registration flow (replaces generic user registration) |
| S1-2 | **As a teacher**, I can create a classroom and get a join code | NEW | Classroom created, word-based join code generated (e.g., "STAR-MATH-BLUE") |
| S1-3 | **As a student**, I can join a classroom using a code and display name (no email required) | NEW | Class code + name → account created, linked to classroom |
| S1-4 | As a student, I see a dashboard with game modes available to me | MODIFIED | Teacher-configured modes shown; kid-friendly layout |
| S1-5 | As a student, I can play Check It! (Answer Checker) session of 15 questions | SAME | Same engine, kid-friendly feedback messages |
| S1-6 | As a student, I see a **kid-friendly** score summary after completing a session | MODIFIED | "You got 12 right! Nice work! 🎯" — not raw statistics |
| S1-7 | As a student, my session results are saved to the database | SAME | |
| S1-8 | **As a teacher**, I can see a list of students in my classroom and whether they've practiced today | NEW | Basic roster view with activity indicators |

**Sprint 1 DoD:** Teacher can create class → Students join via code → One game mode works → Teacher sees basic roster.

---

### Sprint 2: Second Mode + Student Progress (Weeks 5-6)

**Sprint Goal:** Two modes playable, students see streaks and progress, teacher dashboard shows basic data.

| ID | User Story | Same/New | Acceptance Criteria |
|----|-----------|----------|---------------------|
| S2-1 | As a student, I can play Mystery Number (Number Guesser) | SAME | Kid-friendly hints ("Go higher! 📈"), efficiency shown as stars not percentages |
| S2-2 | As a student, I can choose which game mode to play | SAME | |
| S2-3 | As a student, I see my practice streak on the dashboard | MODIFIED | Visual streak (calendar with stars), never "you broke your streak" messaging |
| S2-4 | As a student, I see my progress with **kid-friendly skill labels** | NEW | "Times Tables: Getting Stronger! ⭐⭐⭐" not "Tier 3 accuracy 68%" |
| S2-5 | **As a teacher**, I can see a class dashboard with per-student overview | NEW | Who's practicing, who's not, basic accuracy by student |
| S2-6 | **As a teacher**, I can click a student to see their skill breakdown | NEW | Per-mode, per-skill detail view |
| S2-7 | As a developer, daily_stats are computed after each session | SAME | |

---

### Sprint 3: Speed Mode + Adaptive Difficulty (Weeks 7-8)

**Sprint Goal:** Speed Round playable with timer, difficulty adapts, personal bests tracked.

| ID | User Story | Same/New | Acceptance Criteria |
|----|-----------|----------|---------------------|
| S3-1 | As a student, I can play Speed Round (Electro Flash) with countdown timer | SAME | Same engine, kid-friendly celebration on personal bests |
| S3-2 | As a student, my difficulty adjusts between sessions based on performance | SAME | Same algorithm; tier labels hidden from students |
| S3-3 | **As a teacher**, I can see difficulty tier distribution for my class | NEW | "12 students at Tier 3, 8 at Tier 4, 5 at Tier 5" |
| S3-4 | As a student, I see a celebration when I beat my personal record | MODIFIED | Animated confetti/star burst, age-appropriate |
| S3-5 | As a developer, challenge generator uses tier parameters | SAME | |
| S3-6 | **As a teacher**, I can set which game modes are available in my classroom | NEW | Toggle modes on/off per classroom |

---

### Sprint 4: Solo Suite + Teacher Dashboard v1 (Weeks 9-10)

**Sprint Goal:** All four solo modes. Teacher dashboard has actionable data.

| ID | User Story | Same/New | Acceptance Criteria |
|----|-----------|----------|---------------------|
| S4-1 | As a student, I can play Fill It In (Missing Number) | SAME | |
| S4-2 | As a student, I see a visual progress chart for my skills | MODIFIED | Bright, simple bar chart or star rating — not radar charts |
| S4-3 | **As a teacher**, I can see a "needs attention" alert list | NEW | Students whose accuracy dropped significantly or who haven't practiced |
| S4-4 | **As a teacher**, I can assign practice (minimum minutes/sessions) | NEW | Assignment created, completion tracked per student |
| S4-5 | **As a teacher**, I can export a class progress report | NEW | CSV or printable view for parent conferences |
| S4-6 | As a student, I see a **growth-focused** leaderboard (improvement, not raw score) | MODIFIED | Shows "most improved" and "longest streak" — never raw rank by accuracy |
| S4-7 | **As a teacher**, I can see standards alignment mapping | NEW | Which CCSSM standards each tier addresses |

---

### Sprint 5: Multiplayer — Hot Potato (Weeks 11-12)

**Sprint Goal:** Students can play Hot Potato together in real-time. Teacher controls access.

| ID | User Story | Same/New | Acceptance Criteria |
|----|-----------|----------|---------------------|
| S5-1 | **As a teacher**, I can enable/disable multiplayer for my classroom | NEW | Toggle in classroom settings |
| S5-2 | As a student, I can create a Hot Potato game within my classroom | MODIFIED | Games scoped to classroom — can't play with strangers |
| S5-3 | As a student, I can join a Hot Potato game via class join | MODIFIED | See available games within my classroom |
| S5-4 | Multiplayer gameplay works (turns, timer, elimination) | SAME | Same WebSocket architecture |
| S5-5 | As a student, I see results with **encouraging** messaging | MODIFIED | "Great game! You answered 8 correctly!" not "You were eliminated 3rd" |
| S5-6 | Practice mode against AI timer available | SAME | |
| S5-7 | **As a teacher**, multiplayer sessions log to teacher dashboard | NEW | Teacher can see who played, when, how they did |

---

### Sprint 6: Last One Standing + Parent Dashboard (Weeks 13-14)

**Sprint Goal:** Full feature set including parent access and polish.

| ID | User Story | Same/New | Acceptance Criteria |
|----|-----------|----------|---------------------|
| S6-1 | As a student, I can play Last One Standing against classmate or AI | SAME | |
| S6-2 | As a student, I earn achievements (badges) for milestones | MODIFIED | Kid-friendly badge names: "Math Explorer," "Streak Star," "Speed Champion" |
| S6-3 | **As a parent**, I can link to my child's account and see their progress | NEW | Link code from teacher, parent-friendly dashboard |
| S6-4 | **As a parent**, I can opt into weekly progress emails | NEW | Automated email with plain-English summary |
| S6-5 | UI is responsive and works on **school Chromebooks** | MODIFIED | Priority: Chromebook, then tablet, then phone |
| S6-6 | **COPPA compliance** verified: no email for under-13, minimal data collection, no ads | NEW | Audit checklist completed |

---

### Sprint 7: Demo Prep + Deployment (Weeks 15-16)

| ID | User Story | Same/New | Notes |
|----|-----------|----------|-------|
| S7-1 | Application deployed at production URL | SAME | |
| S7-2 | README documents setup and architecture | SAME | |
| S7-3 | Demo presentation prepared | SAME | |
| S7-4 | DataBuddy mascot/character elements polished | NEW | Friendly robot character in loading states, achievements, empty states |
| S7-5 | Course retrospective completed | SAME | |
| S7-6 | Portfolio documents prepared | SAME | |

---

### 6.2 Database Challenge Profile — K-12 Differences

| Challenge | CogniCalibrate Version | DataBuddy Version | What Changes |
|-----------|----------------------|-------------------|--------------|
| **Rolling Window** | Same computation | Same computation | Nothing — same algorithm |
| **Leaderboard** | Rank by improvement score | Rank by improvement, **never by raw score** | Query adds WHERE clause, different sort |
| **Multi-Role Auth** | One user type | Four user types with different permissions | Role-based access control, scoped queries |
| **Classroom Scoping** | N/A | All student data scoped to classroom | Every query adds classroom_id filter |
| **Standards Mapping** | N/A | Tier-to-standard alignment JOIN | New tables, new queries |
| **COPPA Compliance** | N/A | Data minimization, deletion endpoints | Schema constraints, deletion cascades |
| **Three Dashboards** | One view | Three role-specific aggregation views | Three different query patterns for same data |

**Signature Query:**
```sql
-- "Show me all students in Mrs. Rodriguez's class who are below
--  grade-level expectations in multiplication, ranked by how much
--  they've improved this week"
-- Requires: classroom JOIN, difficulty tier → standard mapping,
--  weekly improvement calculation, grade-level threshold comparison
```

**Portfolio Highlight:** "Built a multi-role adaptive learning platform with COPPA-compliant student data handling, classroom management, teacher and parent dashboards, and standards-aligned progress tracking"

---

## END OF DATABUDDY TECHNICAL BRIEF — PARTS 5-6

**Companion Document:** `DataBuddy_base.md` contains Parts 1-4 (Creative Brief, Product Deep Dive, User Research, Strategic Guidance)

**Key Takeaway for Students:**
Compare this document to `CogniCalibrate_technical.md`. The challenge engine — generation, difficulty, scoring, multiplayer — is identical. The *product* layer — accounts, permissions, dashboards, compliance, messaging — is entirely different. That's the lesson: **platforms are technical; products are human.**

---

*"Every kid deserves math practice that meets them where they are."*

*"Every developer deserves to understand the difference between a platform and a product."*
