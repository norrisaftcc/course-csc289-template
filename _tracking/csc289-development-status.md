# CSC 289 Programming Capstone: Development Status & Planning Document

**Document Version:** 0.1  
**Last Updated:** December 2024  
**Purpose:** Establish baseline for course manifest creation and prioritize development work

---

## 1. Course Metadata

| Field | Value |
|-------|-------|
| **Course ID** | CSC-289 |
| **Course Name** | Programming Capstone |
| **Credits** | 3 |
| **Prerequisites** | CTS-285 (or ORANGE clearance equivalent) |
| **Corequisites** | GRD Capstone (design collaboration), DBA-289 (DB specialists) |
| **Semester Length** | 16 weeks |
| **Module Structure** | 8 modules × 2 weeks each |
| **Sprint Structure** | 7 two-week sprints (Weeks 2-15) |
| **Repository Pattern** | `capstone-{platform}-{team}` |

### Catalog Description (Proposed)
> This capstone course integrates software development skills in a team-based project environment. Students work in cross-functional teams to design, build, and deploy a production-quality web application. Topics include agile methodology, team collaboration, code review, deployment, and portfolio development. Upon completion, students will have demonstrated professional-level software development competency through a completed product and technical presentation.

### Learning Outcomes
1. Apply agile/Scrum methodology in a team software development context
2. Collaborate effectively with cross-functional team members (developers, designers, DB specialists)
3. Design and implement a multi-tier web application with database backend
4. Conduct meaningful code reviews and respond to peer feedback
5. Deploy and maintain a production application
6. Present technical work to stakeholders through pitch and demo formats
7. Document architecture decisions and technical specifications
8. Demonstrate YELLOW and GREEN clearance competencies via exit tickets

---

## 2. Course Structure Overview

### Clearance Progression

```
Week:  1  2  3  4  5  6  7  8  9  10 11 12 13 14 15 16
       ├──┴──┼──┴──┼──┴──┼──┴──┼──┴──┼──┴──┼──┴──┼──┴──┤
       │ M01 │ M02 │ M03 │ M04 │ M05 │ M06 │ M07 │ M08 │
       └─────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┘

Clearance:
  ORANGE ════════════╗
                     ╠═══ YELLOW ══════════════╗
                                               ╠═══ GREEN ════════
                                               
Key Milestones:
  Week 4: PITCH ──────────────────────────────────────┐
  Week 8: Design Handoff ─────────────────────────────┤
  Week 10-12: YELLOW Exit Tickets ────────────────────┤
  Week 13-14: GREEN Exit Tickets ─────────────────────┤
  Week 15: DEMO ──────────────────────────────────────┘
```

### Module-Sprint Alignment

| Module | Weeks | Sprint | Clearance Focus | Major Events |
|--------|-------|--------|-----------------|--------------|
| M01 | 1-2 | Sprint 0 | ORANGE (entry/refresh) | Team formation, DB catch-up |
| M02 | 3-4 | Sprint 1 | ORANGE | **PITCH PRESENTATIONS (Week 4)** |
| M03 | 5-6 | Sprint 2 | ORANGE→YELLOW | Core feature development |
| M04 | 7-8 | Sprint 3 | YELLOW | **Design handoff checkpoint** |
| M05 | 9-10 | Sprint 4 | YELLOW | YELLOW exit tickets |
| M06 | 11-12 | Sprint 5 | YELLOW→GREEN | GREEN exit tickets begin |
| M07 | 13-14 | Sprint 6 | GREEN | Final integration, polish |
| M08 | 15-16 | Sprint 7 | GREEN (exit) | **DEMO (Week 15)**, wrap-up |

### Student Populations

| Role | Entry Clearance | Exit Target | Catch-up Needs |
|------|-----------------|-------------|----------------|
| CTS 285 Graduate | ORANGE | GREEN | Minimal (semester gap refresh) |
| DB Specialist | RED | YELLOW | Git workflow, collaboration patterns |

---

## 3. The Four Platforms

Each platform supports 6 white-label applications (5 genuine + 1 AlgoCratic satirical).

| Platform | Technical Challenge | DB Focus | AlgoCratic Variant |
|----------|--------------------|-----------|--------------------|
| **Task Management** | Multi-tenant workspaces, RBAC | Hierarchies, access control | TeamFlow™ |
| **E-Commerce** | Transaction integrity, state machines | Order processing, inventory | ComplianceCart™ |
| **Recommendation Engine** | Similarity algorithms, preferences | Graph data, ratings | PreferenceOptimizer™ |
| **Health Tracker** | Time-series data, privacy | Temporal queries, retention | BiometricCompliance™ |

**Source:** `26SP_Platform_Project_Matrix.md` (complete)

---

## 4. Content Inventory: What Exists

### 4.1 Planning Documents (Strong)

| Document | File | Status | Notes |
|----------|------|--------|-------|
| Platform Project Matrix | `26SP_Platform_Project_Matrix.md` | ✅ Complete | All 4 platforms + 24 white-labels defined |
| Cross-Discipline Protocol | `26SP_Cross_Discipline_Protocol.md` | ✅ Complete | Dev × Design integration cadence |
| Database Integration Guide | `26SP_Database_Capstone_Integration.md` | ✅ Complete | DB student role definition |
| Uncertainty Matrix | `26SP_Uncertainty_Matrix.md` | ✅ Complete | Open questions tracked |
| Visual Roadmap | `VISUAL_ROADMAP.md` | ✅ Complete | CTS 285 → CSC 289 pipeline |

### 4.2 Student Support (Strong)

| Document | File | Status | Notes |
|----------|------|--------|-------|
| Capstone Survival Guide | `UNDERGROUND_CAPSTONE_SURVIVAL_GUIDE.md` | ✅ Complete | 43KB GameFAQs-style guide |
| Day 1 Essentials | `DAY_1_ESSENTIALS.md` | ✅ Complete | 5-minute survival brief |
| Team Contract Template | (in Day 1 Essentials) | ✅ Draft | Needs extraction to standalone |

### 4.3 Technical References (Strong)

| Document | File | Status | Notes |
|----------|------|--------|-------|
| Node.js Instructor Bootcamp | `Node_js_Infrastructure_Instructor_Bootcamp.md` | ✅ Complete | If using Node stack |
| Node.js Pedagogical Framework | `Advanced_Node_js_Pedagogical_Framework.md` | ✅ Complete | Teaching approach |
| GitHub Surveillance Framework | `The_GitHub_Surveillance_State...` | ✅ Complete | Activity tracking approach |

### 4.4 Shared Framework (Strong)

All AlgoCratic framework documents apply (Style Guide, FOBSS protocols, etc.)

### 4.5 Student-Facing Course Content (Weak)

| Content Type | Exists | Notes |
|--------------|--------|-------|
| Platform briefs (individual) | 0 | Need extraction from Matrix |
| Pitch presentation rubric | 0 | Need creation |
| Demo rubric | 0 | Need creation |
| Sprint ceremony guide | 0 | Need standalone doc |
| YELLOW exit ticket spec | 0 | Need creation |
| GREEN exit ticket spec | 0 | Need creation |
| Peer evaluation rubric | 0 | Need creation |
| Architecture doc template | 0 | Living document template |
| Design brief template | 0 | Living document template |

---

## 5. Gap Analysis by Module

### M01: Level Setting & Team Formation (Weeks 1-2)

**Existing content:**
- ✅ Day 1 Essentials
- ✅ Capstone Survival Guide
- ✅ Platform Project Matrix (source for briefs)
- ✅ Cross-Discipline Protocol

**Content needed:**
- [ ] Platform Brief: Task Management (extract from Matrix)
- [ ] Platform Brief: E-Commerce (extract from Matrix)
- [ ] Platform Brief: Recommendation Engine (extract from Matrix)
- [ ] Platform Brief: Health Tracker (extract from Matrix)
- [ ] Team Contract Template (extract and formalize)
- [ ] DB Specialist Catch-up Guide (Git workflow refresher)
- [ ] Sprint 0 Ceremony Guide (adapted for formation sprint)
- [ ] Communication Channel Setup Guide

### M02: Sprint 1 & Pitch (Weeks 3-4)

**Existing content:**
- ✅ Survival Guide covers sprint basics

**Content needed:**
- [ ] Sprint Ceremony Guide (Planning, Standup, Review, Retro)
- [ ] **Pitch Presentation Rubric** ⭐ HIGH PRIORITY
- [ ] Pitch Presentation Template/Guidelines
- [ ] Sprint 1 Deliverables Checklist
- [ ] First PR Expectations Guide

### M03: Sprint 2 (Weeks 5-6)

**Existing content:**
- ✅ General sprint guidance in Survival Guide

**Content needed:**
- [ ] Sprint 2 Expectations (core features focus)
- [ ] Code Review Rubric (if not using CTS 285 version)
- [ ] Mid-project Architecture Review Prompt

### M04: Sprint 3 & Design Handoff (Weeks 7-8)

**Existing content:**
- ✅ Cross-Discipline Protocol covers handoff

**Content needed:**
- [ ] Design Handoff Checklist
- [ ] Design Brief Template (living document)
- [ ] Sprint 3 Integration Expectations
- [ ] Wireframe-to-Implementation Guide

### M05: Sprint 4 & YELLOW Exit Tickets (Weeks 9-10)

**Existing content:**
- ✅ Training Module Manifest defines YELLOW competencies

**Content needed:**
- [ ] **YELLOW Exit Ticket Specification** ⭐ HIGH PRIORITY
- [ ] YELLOW Competency Checklist
- [ ] Sprint 4 Testing Expectations
- [ ] Performance Baseline Requirements

### M06: Sprint 5 & GREEN Exit Tickets (Weeks 11-12)

**Existing content:**
- ✅ Training Module Manifest defines GREEN competencies

**Content needed:**
- [ ] **GREEN Exit Ticket Specification** ⭐ HIGH PRIORITY
- [ ] GREEN Competency Checklist
- [ ] Documentation Standards Guide
- [ ] Sprint 5 Polish Expectations

### M07: Sprint 6 & Final Integration (Weeks 13-14)

**Existing content:**
- ✅ Survival Guide covers end-game

**Content needed:**
- [ ] Final Integration Checklist
- [ ] Deployment Guide (platform-agnostic)
- [ ] Sprint 6 "Feature Freeze" Protocol
- [ ] Bug Triage Process

### M08: Demo & Wrap-up (Weeks 15-16)

**Existing content:**
- ✅ Survival Guide covers presentations

**Content needed:**
- [ ] **Demo Rubric** ⭐ HIGH PRIORITY
- [ ] Demo Presentation Guidelines
- [ ] Portfolio Documentation Template
- [ ] Peer Evaluation Rubric
- [ ] Course Retrospective Guide

---

## 6. Deliverable Summary

| Type | Total Needed | Currently Exist | Gap |
|------|--------------|-----------------|-----|
| Platform Briefs | 4 | 0 (source exists) | 4 |
| Rubrics | 5 | 0 | 5 |
| Ceremony/Process Guides | 4 | 0 | 4 |
| Exit Ticket Specs | 2 | 0 | 2 |
| Living Doc Templates | 3 | 0 | 3 |
| Checklists | 5 | 0 | 5 |
| Student Guides | 3 | 2 (Day 1, Survival) | 1 |
| **TOTAL** | **26** | **2** | **24** |

**Estimated Completion: ~8%** (student-facing deliverables only)  
**With planning docs factored: ~45%** (strong foundation, weak templates)

---

## 7. Priority Matrix

### Tier 1: Blocks Course Launch
1. **Platform Briefs (4)** - Teams need these Week 1
2. **Team Contract Template** - Due Week 1
3. **Pitch Presentation Rubric** - Needed before Week 4
4. **Sprint Ceremony Guide** - Needed Week 2

### Tier 2: Needed Before Midpoint
5. YELLOW Exit Ticket Specification
6. Design Handoff Checklist
7. Code Review Standards (or inherit from CTS 285)

### Tier 3: Needed Before Finals
8. GREEN Exit Ticket Specification
9. Demo Rubric
10. Peer Evaluation Rubric
11. Portfolio Documentation Template

### Tier 4: Nice to Have
12. Platform-specific technical guides
13. Advanced deployment guides
14. Cross-functional retrospective template

---

## 8. Living Documents Concept

CSC 289 differs from earlier courses in having documents that evolve throughout the semester. These need templates, not finished content.

| Living Document | Created | Updated | Owner |
|-----------------|---------|---------|-------|
| Team Contract | M01 | As needed | Team |
| Platform Brief | M01 (provided) | Never | Instructor |
| Architecture Decision Record | M02 | Each sprint | Team |
| Design Brief | M04 | M04-M06 | Team + Design |
| Deployment Runbook | M06 | M06-M08 | Team |
| Portfolio Page | M07 | M07-M08 | Individual |

**Manifest tracks:** Template existence and status  
**GitHub tracks:** Per-team document evolution

---

## 9. Exit Ticket Framework

### YELLOW Exit Tickets (M05, Weeks 9-10)

From `TRAINING_MODULE_MANIFEST.md`, YELLOW requires:
- Demonstrate debugging proficiency
- Complete integration projects successfully
- Maintain productivity above threshold

**Proposed YELLOW Exit Ticket Components:**
1. **Technical Challenge** - Debug a provided broken integration
2. **Code Review** - Review a PR with substantive feedback
3. **Documentation** - Explain an architecture decision in writing

### GREEN Exit Tickets (M06, Weeks 11-12)

From `TRAINING_MODULE_MANIFEST.md`, GREEN requires:
- Optimize system performance metrics
- Lead small team initiatives
- Show strategic thinking capabilities

**Proposed GREEN Exit Ticket Components:**
1. **Leadership Evidence** - Document a time you helped a teammate succeed
2. **Technical Depth** - Explain a complex system component to a "new hire"
3. **Portfolio Artifact** - Polished documentation of your contribution

---

## 10. Open Questions

### Resolved by This Document
- ✅ Module structure (8 modules, sprint-aligned)
- ✅ Clearance progression (ORANGE→YELLOW→GREEN)
- ✅ Major milestones (Pitch Week 4, Demo Week 15)

### Still Open
1. **Exit ticket timing:** Are they during class? Take-home? Scheduled appointments?
2. **Platform assignment:** Student preference, random, or instructor assigned?
3. **Team size:** 3-5 confirmed, but what's the target?
4. **DB specialist pairing:** One per team, but what if enrollment doesn't match?
5. **Design collaboration:** Required or optional integration with GRD capstone?
6. **Deployment target:** Heroku? Railway? Self-hosted? Student choice?

---

## 11. Recommended Development Sequence

### Sprint 1: Launch Essentials (Before Week 1)
- [ ] Extract 4 Platform Briefs from Matrix
- [ ] Formalize Team Contract Template
- [ ] Create Sprint Ceremony Guide
- [ ] Create DB Catch-up Guide

### Sprint 2: Pitch Support (Before Week 4)
- [ ] Create Pitch Presentation Rubric
- [ ] Create Pitch Guidelines Document
- [ ] Review Sprint Ceremony Guide with first cohort

### Sprint 3: Midpoint Materials (Before Week 8)
- [ ] Create Design Handoff Checklist
- [ ] Create YELLOW Exit Ticket Spec
- [ ] Create Architecture Decision Template

### Sprint 4: Exit & Demo (Before Week 13)
- [ ] Create GREEN Exit Ticket Spec
- [ ] Create Demo Rubric
- [ ] Create Peer Evaluation Rubric
- [ ] Create Portfolio Template

---

## 12. Notes for Manifest Creation

The CSC 289 manifest should:

1. **Track teaching materials, not student work** - GitHub handles team progress
2. **Include platform briefs as deliverables** - One per platform, instructor-provided
3. **Mark living document templates** - Distinguish from one-time deliverables
4. **Note ceremony vs. content deliverables** - Some "deliverables" are process guides
5. **Align modules to sprints** - But not 1:1 (Sprint 0 is half a sprint, Sprint 7 is demo prep)

---

*"The Algorithm has optimized your capstone. Your compliance will be measured in commits, your loyalty in code reviews, your growth in GREEN clearance."*

**Document Status:** Ready for manifest conversion  
**Next Action:** Create `course-manifest-csc289.yaml`
