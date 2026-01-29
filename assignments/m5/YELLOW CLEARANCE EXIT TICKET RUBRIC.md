# YELLOW CLEARANCE EXIT TICKET RUBRIC
## CSC 289 Programming Capstone - Week 10 Individual Assessment

**AlgoCratic Futures™ Personnel Advancement Division**  
**Assessment Code:** YELLOW-CERT-26SP  
**Total Points:** 100  
**Clearance Advancement:** ORANGE → YELLOW  
**Assessment Type:** Individual (does not affect team grade)  
**Due Date:** Week 10, Friday by 11:59 PM

---

## 📋 OVERVIEW

The YELLOW Clearance Exit Ticket evaluates your individual technical competency at the professional developer level. You are transitioning from "can implement features with guidance" (ORANGE) to "can solve problems independently and communicate technical decisions" (YELLOW).

**YELLOW clearance represents:**
- **Debugging mastery** - You can diagnose and fix complex issues independently
- **Code review expertise** - You can evaluate others' code and provide substantive feedback
- **Technical communication** - You can explain architectural decisions clearly in writing

These are the skills of a senior developer (3-5 years experience equivalent) who makes technical decisions, mentors others, and solves problems without constant supervision.

### The Three Components

| Component | Points | Skill Assessed | Due Date |
|-----------|--------|----------------|----------|
| **Technical Debugging Challenge** | 35 | Problem isolation, systematic debugging, root cause analysis | Week 10, Friday |
| **Substantive Code Review** | 35 | Code comprehension, feedback quality, professional communication | Week 10, Friday |
| **Architecture Decision Record** | 30 | Technical writing, decision justification, alternatives analysis | Week 10, Friday |

**All three components must be submitted to receive YELLOW clearance.** Missing any component results in automatic ORANGE retention.

---

## 🐛 COMPONENT 1: TECHNICAL DEBUGGING CHALLENGE (35 points)

### Overview

You will receive a broken integration (API call, database query, authentication flow, or similar) with symptoms described but root cause hidden. Your task is to diagnose the issue, fix it, and document your debugging process.

**What you'll receive:**
- A repository with broken code
- Description of the symptoms (e.g., "Users can't log in after password reset")
- No indication of where the bug is

**What you must submit:**
1. **Fixed code** (GitHub PR with your solution)
2. **Debugging report** (300-500 words explaining your process)

### Grading Breakdown

#### Problem Identification (10 points)

| Criteria | Excellent (9-10) | Proficient (7-8) | Developing (4-6) | Insufficient (0-3) |
|----------|------------------|------------------|------------------|-------------------|
| **Root Cause Analysis** | Correctly identifies actual root cause. Shows systematic investigation. Clear evidence trail. | Identifies root cause but process less systematic. Some guesswork involved. | Partial identification OR incorrect but plausible diagnosis. | Completely misidentifies root cause OR no clear diagnosis. |

**What we're looking for:**
- Did you find the actual bug?
- Did you investigate systematically (logs, debugger, tests) or just guess?
- Can you explain WHERE the problem is and WHY it manifests?

**Evidence required in your report:**
- "I first checked [X] because [reasoning]"
- "The error message indicated [Y], which led me to investigate [Z]"
- "I used [debugging tool/technique] to isolate the issue"

#### Solution Quality (15 points)

| Criteria | Excellent (14-15) | Proficient (11-13) | Developing (7-10) | Insufficient (0-6) |
|----------|-------------------|-------------------|------------------|-------------------|
| **Fix Implementation** | Elegant fix that addresses root cause. No side effects. Includes test to prevent recurrence. | Working fix that solves problem. May be less elegant. Test present but basic. | Fix works but hacky/fragile. No test or inadequate test. | Fix doesn't work OR creates new problems OR missing entirely. |

**What we're looking for:**
- Does your fix actually solve the problem?
- Is it a proper solution or a band-aid?
- Did you add a test to catch this in the future?
- Is the code clean and maintainable?

**Code quality indicators:**
- Follows project style conventions
- Includes explanatory comments where needed
- Minimal changes (surgical fix, not rewriting everything)
- Edge cases considered

#### Documentation (10 points)

| Criteria | Excellent (9-10) | Proficient (7-8) | Developing (4-6) | Insufficient (0-3) |
|----------|------------------|------------------|------------------|-------------------|
| **Debugging Report** | Clear narrative of debugging process. Explains dead ends and breakthroughs. Teaches someone else how to debug similar issues. | Describes process adequately. Some detail on investigation steps. | Vague description. Lists steps but doesn't explain reasoning. | Minimal documentation OR just describes the fix without process. |

**Required elements in your report:**
1. **Initial hypothesis** - What did you think was wrong first?
2. **Investigation steps** - What did you check and in what order?
3. **Dead ends** - What didn't work? (This is important!)
4. **Breakthrough** - What discovery led you to the solution?
5. **Root cause** - Technical explanation of why the bug existed
6. **Prevention** - How could this have been caught earlier?

**Length:** 300-500 words (enforced - too short lacks detail, too long means you're padding)

---

## 📝 COMPONENT 2: SUBSTANTIVE CODE REVIEW (35 points)

### Overview

You will review a pull request from another team's project. This simulates a common professional practice: reviewing code you didn't write, in a codebase you're not deeply familiar with.

**What you'll receive:**
- A GitHub PR link from another team's project
- Context about what the PR is supposed to do
- Access to the repository

**What you must submit:**
- Minimum **8 substantive review comments** directly on the GitHub PR
- Comments must be constructive, specific, and demonstrate code comprehension

### Grading Breakdown

#### Code Comprehension (10 points)

| Criteria | Excellent (9-10) | Proficient (7-8) | Developing (4-6) | Insufficient (0-3) |
|----------|------------------|------------------|------------------|-------------------|
| **Understanding Demonstrated** | Comments show deep understanding of what code does and why. Identifies subtle issues. Connects changes to broader architecture. | Comments show good understanding. Identifies main logic. Some architectural awareness. | Surface-level understanding. May misinterpret code purpose. Limited context. | Clearly doesn't understand the code. Comments are wrong or irrelevant. |

**What we're looking for:**
- Do your comments show you read the code carefully?
- Do you understand the control flow and data transformations?
- Can you trace the logic from inputs to outputs?
- Do you grasp why this code exists in the first place?

**Examples:**

**Good (shows comprehension):**
> "In line 47, you're filtering users by `status == 'active'` but then in line 52 you're accessing `user.last_login` without checking if it exists. If a user is active but has never logged in (e.g., just created account), this will throw an AttributeError."

**Bad (no comprehension):**
> "You should add more comments here."

#### Feedback Quality (15 points)

| Criteria | Excellent (14-15) | Proficient (11-13) | Developing (7-10) | Insufficient (0-6) |
|----------|-------------------|-------------------|------------------|-------------------|
| **Review Substance** | At least 8 substantive comments. Identifies bugs, security issues, performance concerns, and/or design problems. Provides specific suggestions for improvement. | 6-7 substantive comments OR 8+ comments with some being superficial. Identifies some issues with solutions. | 4-5 substantive comments OR many comments but mostly trivial (style, formatting). | Fewer than 4 comments OR all comments are trivial/unhelpful. |

**Substantive comments include:**
- **Bugs:** "This will crash if X happens"
- **Security issues:** "SQL injection possible here"
- **Performance concerns:** "This N+1 query will be slow with many users"
- **Logic errors:** "This doesn't handle the case where..."
- **Design improvements:** "Consider extracting this into a helper function"
- **Edge cases:** "What if the input is empty/null/negative?"

**Non-substantive comments (don't count):**
- "Good job!"
- "Add a space here"
- "Rename this variable to be more clear" (unless explaining why current name is misleading)
- "LGTM" (Looks Good To Me) without explanation

**Required:** At least **ONE** comment must identify a security or performance concern

#### Professional Communication (10 points)

| Criteria | Excellent (9-10) | Proficient (7-8) | Developing (4-6) | Insufficient (0-3) |
|----------|------------------|------------------|------------------|-------------------|
| **Tone & Clarity** | Constructive, respectful tone. Clear explanations. Balances criticism with acknowledgment of what works. Suggests solutions, not just problems. | Mostly constructive. Clear enough. May be blunt but not rude. | Some comments are harsh or unclear. May be demotivating. | Rude, dismissive, or condescending tone OR completely unclear feedback. |

**What we're looking for:**
- Would you want to receive these comments on your code?
- Do you explain WHY something is a problem, not just THAT it's a problem?
- Do you suggest solutions or alternatives?
- Do you acknowledge good decisions when you see them?

**Examples:**

**Good tone:**
> "Nice use of list comprehension in line 23! One suggestion: on line 28, consider using `get()` instead of direct dictionary access to avoid KeyError if the API response changes structure."

**Bad tone:**
> "This is terrible. Why would you do it this way?"

**Required:** Comments must be written as if this code will be read by the PR author. Assume they're a colleague you want to maintain a good relationship with.

---

## 🏗️ COMPONENT 3: ARCHITECTURE DECISION RECORD (30 points)

### Overview

You will write an Architecture Decision Record (ADR) documenting a significant technical decision made in your capstone project. This demonstrates your ability to justify technical choices and think strategically about system design.

**What is an ADR?**
An ADR is a document that captures an important architectural decision along with its context and consequences. It's a standard professional practice for documenting "why we built it this way."

**What you must submit:**
A markdown document (500-800 words) following the ADR format, submitted to your team's repository.

### ADR Required Sections

Your ADR must include all five sections:

1. **Context** - What problem or situation required a decision?
2. **Decision** - What did you decide to do?
3. **Rationale** - Why did you make this decision?
4. **Consequences** - What are the positive and negative outcomes?
5. **Alternatives Considered** - What other options did you evaluate and reject?

### Grading Breakdown

#### Technical Accuracy (10 points)

| Criteria | Excellent (9-10) | Proficient (7-8) | Developing (4-6) | Insufficient (0-3) |
|----------|------------------|------------------|------------------|-------------------|
| **Technical Correctness** | Decision is technically sound. Rationale demonstrates deep understanding. No significant technical errors. | Decision is reasonable. Minor technical inaccuracies. Generally sound reasoning. | Decision is questionable but not catastrophic. Some technical misunderstandings. | Decision is technically wrong OR serious misunderstandings evident. |

**What we're looking for:**
- Is this decision actually a good technical choice?
- Do you understand the technical trade-offs?
- Are your technical explanations accurate?

**Examples of good decisions to document:**
- Why you chose Flask over Node.js
- Why you used PostgreSQL's JSONB type for certain data
- Why you implemented caching at the application layer vs. database
- Why you chose a certain authentication strategy
- Why you structured your database schema a particular way

#### Clarity & Structure (8 points)

| Criteria | Excellent (8) | Proficient (6-7) | Developing (4-5) | Insufficient (0-3) |
|----------|--------------|------------------|------------------|-------------------|
| **Writing Quality** | Clear, well-organized prose. All five sections present and complete. Technical concepts explained for a knowledgeable audience. | Adequately clear. All sections present but some may be thin. | Somewhat disorganized. May be missing a section or have very weak sections. | Poorly written OR missing multiple sections OR incomprehensible. |

**Required section lengths (approximate):**
- Context: 100-150 words
- Decision: 50-100 words
- Rationale: 150-250 words (this is the heart of the ADR)
- Consequences: 100-150 words
- Alternatives: 100-150 words

**Writing quality indicators:**
- Uses technical terminology correctly
- Explains acronyms on first use
- Organized logically
- No major grammar/spelling errors
- Written for a technical audience (not dumbed down, not needlessly complex)

#### Alternatives Analysis (7 points)

| Criteria | Excellent (7) | Proficient (5-6) | Developing (3-4) | Insufficient (0-2) |
|----------|--------------|------------------|------------------|-------------------|
| **Alternative Evaluation** | Describes 2-3 genuine alternatives. Explains pros/cons of each. Shows you seriously considered other options. | Mentions 1-2 alternatives with some analysis. May be superficial. | Mentions alternatives but no real analysis OR only strawman alternatives. | No alternatives discussed OR clearly made up after the fact. |

**What we're looking for:**
- Did you actually consider other options?
- Can you articulate why you rejected them?
- Are the alternatives realistic (not just "do nothing" and "build a supercomputer")?

**Example:**

**Good alternatives section:**
> "We considered three approaches:
> 1. **Microservices architecture** - Would provide better scalability but adds complexity we don't need for our user base (< 1000 users). Deployment would be more complex than our team can manage in the time available.
> 2. **Monolithic architecture with modular design** - (Our choice) Simpler deployment, faster development, still maintainable. Trade-off: harder to scale horizontally if we exceed 10,000 concurrent users (unlikely in our context).
> 3. **Serverless functions** - Would be very scalable but introduces cold start latency (300-500ms) which violates our < 200ms response time requirement."

**Bad alternatives section:**
> "We thought about using a different database but PostgreSQL seemed good."

#### Consequences Discussion (5 points)

| Criteria | Excellent (5) | Proficient (3-4) | Developing (1-2) | Insufficient (0) |
|----------|--------------|------------------|------------------|-------------------|
| **Impact Analysis** | Discusses both positive and negative consequences. Considers performance, maintainability, scalability, cost, team expertise. | Mentions consequences but may be one-sided or incomplete. | Very brief or only positive consequences mentioned. | No consequences discussed OR clearly fictional. |

**What we're looking for:**
- Do you understand the implications of this decision?
- Are you honest about the downsides?
- Do you consider multiple dimensions (not just performance)?

**Example:**

**Good consequences section:**
> "**Positive consequences:**
> - Development velocity increased (team already knows Flask)
> - Debugging is simpler with synchronous request handling
> - Deployment is straightforward (single process)
>
> **Negative consequences:**
> - May struggle with concurrent users above 1000
> - CPU-intensive tasks will block other requests
> - Migration to async framework later would require significant refactoring"

**Bad consequences section:**
> "This was a good decision and will make everything better."

---

## 📊 EVALUATION SUMMARY

| Component | Points | Your Score |
|-----------|--------|------------|
| **COMPONENT 1: Technical Debugging Challenge** | 35 | |
| - Problem Identification | 10 | |
| - Solution Quality | 15 | |
| - Documentation | 10 | |
| **COMPONENT 2: Substantive Code Review** | 35 | |
| - Code Comprehension | 10 | |
| - Feedback Quality | 15 | |
| - Professional Communication | 10 | |
| **COMPONENT 3: Architecture Decision Record** | 30 | |
| - Technical Accuracy | 10 | |
| - Clarity & Structure | 8 | |
| - Alternatives Analysis | 7 | |
| - Consequences Discussion | 5 | |
| **TOTAL** | **100** | |

---

## 🎯 CLEARANCE ADVANCEMENT CRITERIA

| Points | Clearance Granted | Interpretation |
|--------|-------------------|----------------|
| 93-100 | YELLOW | Exemplary technical competency. Strong senior developer level. |
| 85-92 | YELLOW | Solid technical competency. Professional developer level. |
| 77-84 | PROVISIONAL YELLOW | Adequate competency. Must demonstrate continued growth. |
| 70-76 | ORANGE (retained) | Not yet ready for YELLOW. Requires remediation. |
| 0-69 | ORANGE (retained) | Significant gaps in technical competency. |

**CRITICAL:** Achieving YELLOW clearance requires:
- Minimum 70 points overall
- No component scored below 20/35 or 17/30
- All three components submitted

**Provisional YELLOW:** Students in the 77-84 range receive YELLOW clearance but are flagged for additional support. They can still achieve GREEN clearance in Week 14 if they demonstrate growth.

---

## ⚠️ SUBMISSION REQUIREMENTS

### Component 1: Technical Debugging Challenge

**Deliverables:**
1. GitHub PR with your fix (link submitted to Canvas)
2. Debugging report (300-500 words, submitted as markdown or PDF)

**File naming:** `yellow-debug-[YourLastName].md` or `.pdf`

### Component 2: Substantive Code Review

**Deliverables:**
1. Comments posted directly on the assigned GitHub PR
2. Screenshot compilation (PDF) showing all your comments (backup in case GitHub is unavailable)

**File naming:** `yellow-review-[YourLastName].pdf`

### Component 3: Architecture Decision Record

**Deliverables:**
1. ADR document added to your team's repository at `/docs/architecture/adr-[number]-[title].md`
2. Link to the committed ADR submitted to Canvas

**File naming:** `adr-001-database-choice.md` (or similar, numbered sequentially if you have multiple ADRs)

**All submissions due:** Week 10, Friday by 11:59 PM

---

## 💡 TIPS FOR SUCCESS

### Technical Debugging Challenge

✅ **Do this:**
- Work systematically (don't just try random fixes)
- Use debugging tools (print statements, debuggers, logs)
- Document your process AS YOU GO (don't reconstruct from memory)
- Test your fix thoroughly
- Add a test to prevent regression

❌ **Don't do this:**
- Guess wildly without investigation
- Submit a fix without understanding why it works
- Copy solutions from Stack Overflow without understanding
- Skip the documentation ("the fix speaks for itself")

### Substantive Code Review

✅ **Do this:**
- Read the code carefully before commenting
- Run the code if possible
- Think about edge cases and error handling
- Provide specific, actionable feedback
- Balance criticism with acknowledgment
- Suggest alternatives, not just problems

❌ **Don't do this:**
- Skim the code and leave superficial comments
- Be harsh or condescending
- Focus only on style/formatting
- Leave 8 comments on one function (spread them out)
- Say "this is wrong" without explaining why

### Architecture Decision Record

✅ **Do this:**
- Choose a significant decision (database choice, architecture style, auth strategy)
- Be honest about trade-offs
- Show you considered alternatives seriously
- Write for a technical audience
- Use the standard five-section format

❌ **Don't do this:**
- Document a trivial decision ("we chose to use Git")
- Pretend there are no downsides to your choice
- Make up alternatives you never considered
- Write vaguely ("we chose the best option")
- Skip sections or combine them incorrectly

---

## 🤔 FREQUENTLY ASKED QUESTIONS

**Q: What if I can't fix the debugging challenge?**  
A: Document what you tried and why it didn't work. Partial credit is possible for systematic investigation even if you don't find the solution.

**Q: Can I ask teammates for help on debugging?**  
A: You can discuss approaches, but the fix and report must be your own work. Don't copy code.

**Q: What if the PR I'm reviewing has no problems?**  
A: Unlikely, but if code is truly excellent, you can suggest enhancements, discuss design alternatives, or explain why certain choices are good. Find something substantive to say.

**Q: Can I write an ADR about a decision I didn't personally make?**  
A: Yes, but you must understand it deeply enough to document it. This is a common professional task (documenting existing architecture).

**Q: What if we haven't made any architectural decisions yet?**  
A: You have. You chose a tech stack, database, authentication approach, file structure, etc. Document one of those.

**Q: How long should each component take?**  
A: Budget 4-6 hours total (1.5-2 hours per component). This is individual work.

**Q: Is this graded harshly?**  
A: It's graded fairly but rigorously. YELLOW clearance means "senior developer level." We're assessing if you're ready for that.

**Q: Can I revise and resubmit?**  
A: No. This is an exit ticket, not an iterative assignment. Make it good the first time.

---

## 🚀 THE MEANING OF YELLOW CLEARANCE

YELLOW clearance is not just a grade. It's a professional milestone that represents:

**Technical Competency:**
- You can debug complex issues independently
- You can evaluate code quality and provide meaningful feedback
- You can articulate technical decisions clearly

**Professional Readiness:**
- You think systematically about problems
- You communicate technical concepts effectively
- You consider trade-offs and alternatives
- You document your decisions for others

**Career Translation:**
YELLOW clearance maps to 3-5 years professional experience. Employers hiring at this level expect you to:
- Solve problems without constant supervision
- Review junior developers' code
- Make architecture decisions
- Explain technical choices to non-technical stakeholders

**This exit ticket proves you can do all of that.**

---

**The Algorithm recognizes your growing competence. Demonstrate it systematically. Your YELLOW clearance awaits.**

---

**Document Version:** 1.0  
**Last Updated:** January 2026  
**Reviewed By:** AlgoCratic Futures™ Technical Competency Division

*"Your debugging will be evaluated. Your reviews will be analyzed. Your decisions will be documented. Your advancement will be optimized."*