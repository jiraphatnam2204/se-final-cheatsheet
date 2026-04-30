---
title: "ISO/IEC 29110 — Exam Study Guide"
tags: [software-engineering, iso-29110, certification, vse, exam-prep]
created: 2026-04-30
exam-in: 3 days
---

# ISO/IEC 29110 — Exam Study Guide

> [!info] Source
> Lecture slides by Dr. Nuengwong Tuaycharoen (ref: ISEM.co.th), Week 15.
> Standard scope: **Software Engineering — Lifecycle Profiles for Very Small Entities (VSEs)**.

---

## 1. Core Concepts

> [!abstract] How to use this section
> Each term below is written as it would appear on an exam. Memorize the **acronym → full name → one-line purpose** triple for each.

### 1.1 The Standard Itself

- **ISO/IEC 29110** — A family of international standards titled *Software Engineering — Lifecycle Profiles for Very Small Entities (VSEs)*; it defines lightweight processes for software organizations of **≤ 25 employees** (or small software units inside larger organizations), modeled on **ISO/IEC 12207** (the slide says "ISO 15540," but the actual parent standard is **ISO/IEC 12207**; the slide's "ISO 15504" reference is to *Process Assessment*, not the lifecycle parent).
- **VSE (Very Small Entity)** — An enterprise, organization, department, or project of **up to 25 people** engaged in software development; the target audience of ISO/IEC 29110.
- **ISO/IEC 29110-4-1** — The **Profile Specification** part: defines the *what* — the model, goals, and requirements that must be satisfied for the **Basic Profile**.
- **ISO/IEC 29110-5-1** — The **Management and Engineering Guide** part: defines the *how* — the required **processes**, **activities**, **tasks**, and **work products (artifacts)** that implement Part 4-1.

### 1.2 The Two Required Processes (Part 5-1, Basic Profile)

- **PM — Project Management Process** — Establishes and manages the project tasks, enabling fulfillment of objectives in terms of quality, time, and cost.
- **SI — Software Implementation Process** — The systematic engineering activities for analyzing, designing, constructing, integrating, and delivering software per requirements.

### 1.3 Certification Roles

- **CB — Certification Body** — The accredited authority that issues the certificate (e.g., in Thailand: **depa**-recognized bodies).
- **CLA — Certified Lead Auditor** — Heads the audit; owns planning, execution, risk, and the customer contract.
- **ATM — Assessment Team Member** — Performs document checks, interviews, and rates artifacts under the CLA.
- **COB — Certified Observer** — Mentors and evaluates **candidate** Lead Auditors; also participates in the assessment.
- **CU — Certified Unit** — The customer organization being audited (its sub-roles are listed in §1.4).

### 1.4 Customer-Side (CU) Roles

- **Project Sponsor** — Funds and authorizes the assessment; must attend Kickoff, Opening Brief, and Final Findings.
- **First-Business Point of Contact (1BPOC)** — Logistics liaison (rooms, parking, food).
- **Certification Site Coordinator / Technical Supporter** — Provides technical infrastructure (projector, Wi-Fi, etc.).
- **Project Owner** — Prepares evidence/documents and assembles interviewees.
- **Interviewees** — PM, Project Coordinator, Engineers (Analyst/Developer/Tester), Configuration Management.

### 1.5 Rating Vocabulary

- **Goal** — A required outcome of a Process Area; the unit of pass/fail in 29110-4-1.
- **Practice** — A specific activity that contributes to achieving a goal.
- **Artifact / Work Product** — A tangible output (document, code, record) used as objective evidence.
- **Finding** — A documented weakness identified during assessment.
- **Rating Scale (NPLC + NA)** — `NI` (Not), `PI` (Partially), `LI` (Largely), `CI` (Completely), `NA` (Not Applicable).
- **Goal Outcome** — `Satisfied (S)` = PASS, `Unsatisfied (U)` = FAIL.

### 1.6 The 22 Required Work Products (Basic Profile)

> [!tip] Memorize as 4 buckets
> Easier to recall than a flat list of 22.

**Project Management evidence (PM):**
1. Statement of Work
2. Project Plan
3. Project Repository
4. Project Repository Backup
5. Meeting Record
6. Progress Status Record
7. Correction Register
8. Change Request
9. Verification Results
10. Validation Results
11. Acceptance Record

**Requirements & Design (SI early):**
12. Requirements Specification
13. Software Design
14. Traceability Record
15. Test Cases and Test Procedures

**Construction & Integration (SI mid):**
16. Software Component
17. Software (the product)
18. Software Configuration
19. Test Report

**Delivery (SI end):**
20. Product Operation Guide
21. Software User Documentation
22. Maintenance Documentation

---

## 2. Mechanisms & How Things Work

### 2.1 The Two-Process Architecture (Part 5-1)

The Basic Profile organizes work as a **closed loop** between two processes that exchange artifacts:

```
        ┌─────────────────────┐         ┌─────────────────────┐
Customer├──Statement of Work──►│   PM    │──Project Plan──────►│   SI    │
        ◄──Software Config.───┤ Process │◄──Progress / Change─┤ Process │
        └─────────────────────┘         └─────────────────────┘
                                              ▲       │
                                              │       ▼
                                          VSE Management
```

- **PM produces** the Project Plan; **SI consumes** it and reports back via Progress Status Records and Change Requests.
- **SI produces** the Software (+ docs + config); **PM hands it off** to the customer through the Acceptance Record at Project Closure.

### 2.2 PM Process — 4 Activities (Logical Flow)

1. **Project Planning** — Convert *Statement of Work* into a *Project Plan*: scope, schedule, resources, risks, verification/validation strategy. Output goes into the **Project Repository** (with backup).
2. **Project Plan Execution** — Run the plan: hold meetings (→ Meeting Record), record progress (→ Progress Status Record), handle changes (→ Change Request).
3. **Project Assessment & Control** — Compare actuals vs. plan; when deviation occurs, log it in the **Correction Register** and trigger replanning.
4. **Project Closure** — Deliver final product, obtain **Acceptance Record** from customer, archive the **Software Configuration**.

> [!important] Causality
> Assessment & Control is the **feedback loop** that makes the process self-correcting. Without it, deviations from the plan would not propagate back into replanning — this is why it is its own activity, not a sub-task of Execution.

### 2.3 SI Process — 6 Activities (Logical Flow)

1. **Software Implementation Initiation** — Review the Project Plan; assign engineering team; set up environment.
2. **Software Requirements Analysis** — Elicit, document, and verify requirements → **Requirements Specification** + **Validation Results**.
3. **Software Architectural & Detailed Design** — Decompose requirements into components → **Software Design**, **Traceability Record**, and the **Test Cases / Test Procedures** (note: tests are designed *here*, not in the test phase — this is a common exam trap).
4. **Software Construction** — Implement components per the design → **Software Components** + Verification Results.
5. **Software Integration & Tests** — Integrate components, execute tests → **Test Report**, **Software**, **Software User Documentation**.
6. **Product Delivery** — Package and hand over → **Maintenance Documentation**, **Product Operation Guide**, updated **Software Configuration**.

### 2.4 Goal Rating Mechanism (Part 4-1)

A **Goal is rated `Satisfied`** if and only if either of the following holds:

- **Path A:** No findings document weaknesses associated with the goal, **OR**
- **Path B (compound, both must hold):**
  - **(a)** All associated practices are characterized at the organizational-unit level as **LI** (Largely Implemented) **or** **CI** (Completely Implemented), **AND**
  - **(b)** The aggregate of weaknesses does not have a **significant negative impact** on goal achievement.

Otherwise → **Unsatisfied**.

> [!warning] Logical structure trap
> The standard uses **OR** between A and B, but **AND** *inside* B. A goal with one minor weakness can still be Satisfied (Path B), but only if practices are LI/CI **and** the weakness is not significant. Missing either sub-condition flips it to Unsatisfied.

### 2.5 Artifact Scoring Mechanism (Part 5-1)

| % Achievement | Assigned Score | Degree | Code | Color  |
| ------------- | -------------- | ------ | ---- | ------ |
| 0 % – ≤15 %   | 0.0            | Not    | NI   | Red    |
| >15 % – 50 %  | 0.3            | Partial| PI   | Orange |
| >50 % – 90 %  | 0.7            | Largely| LI   | Yellow |
| >90 % – 100 % | 1.0            | Complete| CI  | Green  |
| —             | N/A            | N/A    | NA   | Gray   |

**Rating Criteria (rolling up to PASS/FAIL):**

| Rating Criteria              | Result      | PASS/FAIL |
| ---------------------------- | ----------- | --------- |
| All `LI` or `CI` and **no Red** | Satisfied   | **PASS**  |
| Otherwise                    | Unsatisfied | **FAIL**  |

> [!danger] The "no red" rule
> Even if the *average* score is high, a **single Red (NI) artifact** can fail the entire profile. This is the most common conceptual trap on this material.

### 2.6 Certification Process — End-to-End Pipeline

```
[Gap Analysis] → [Standard Explanation] → [Implementation Help] →
[Organization Ready] → [CB Assessment] → [Certificate Issued] →
[Surveillance #1 @ 365 days] → [Surveillance #2 @ 730 days] → [Re-cert]
```

1. **Gap Analysis** — Compare current practice to the standard; identify deltas.
2. **Standard Explanation** — Train the VSE on what 29110 requires.
3. **Implementation Support** — Adjust workflows and produce the 22 work products.
4. **Assessment** — CB conducts the audit (see §2.7).
5. **CB Decision** — CB issues the certificate based on the assessment report.
6. **Surveillance** — Mandatory check-ins at **365 days** and **730 days** from registration.
7. **Expiration** — At **730 days**, certificate expires; re-audit required.

### 2.7 Assessment Schedule (6 days total)

| # | Phase                     | Duration |
| - | ------------------------- | -------- |
| 1 | Kick Off                  | 1 day    |
| 2 | Confirm Project           | 1 day    |
| 3 | Pre-Onsite (Doc Review)   | 2 days   |
| 4 | Onsite (Interviews)       | 2 days   |

> [!note] Document review *precedes* interviews
> Pre-onsite document review (step 3) feeds the questions used in onsite interviews (step 4). The order is causal, not arbitrary.

### 2.8 Project Selection Criteria (the VSE picks **2 projects**)

1. Project status: **Go-live, Maintenance, or Closed by Condition** within the last **3–6 months**.
2. Project members still employed at the company.
3. Main participants available: **PM, SA/BA, Developer, Tester**.
4. Project covers **both PM and SI** processes.
5. Documentation maps to all **22 work products**.
6. Project represents **core products/services** of the VSE.

### 2.9 Post-Assessment & Surveillance Timeline

| Task                                        | Owner          | Deadline                      |
| ------------------------------------------- | -------------- | ----------------------------- |
| Complete Final Report                       | CLA + ATMs     | Within **60 days**            |
| Provide certification feedback to CB        | CLA, ATMs, Sponsor | Within **30 days**        |
| Surveillance #1 (90-day reminder, 60-day confirm) | Sponsor / CB | Day **365** from registration |
| Surveillance #2 (90-day reminder, 60-day confirm) | Sponsor / CB | Day **730** from registration |
| Certificate expiration                      | —              | Day **730**                   |

---

## 3. Comparisons & Trade-offs

### 3.1 Part 4-1 vs. Part 5-1

| Dimension      | ISO/IEC 29110-4-1                     | ISO/IEC 29110-5-1                              |
| -------------- | ------------------------------------- | ---------------------------------------------- |
| Type           | **Profile Specification**             | **Management & Engineering Guide**             |
| Answers        | *What* must be achieved (goals)       | *How* to achieve it (processes, tasks, artifacts) |
| Used for       | Defining pass/fail criteria           | Implementing the daily workflow                |
| Audience       | Auditors, certification bodies        | VSE practitioners (PM, engineers)              |
| Output of use  | Goal ratings (S / U)                  | 22 work products                               |

### 3.2 Certification Roles — Authority & Accountability

| Role | Side       | Leads? | Mentors? | Rates artifacts? | Owns customer contract? |
| ---- | ---------- | ------ | -------- | ---------------- | ----------------------- |
| CLA  | Auditor    | ✅      | ❌        | ✅                | ✅                       |
| COB  | Auditor    | ❌      | ✅ (mentors candidate CLA) | ❌ | ✅                  |
| ATM  | Auditor    | ❌      | ❌        | ✅                | ✅                       |
| CB   | Authority  | ❌ (governs) | ❌    | ❌ (decides)      | N/A                     |

### 3.3 Rating Levels — Score Bands

| Code | Meaning           | Band         | Counts as PASS contribution? |
| ---- | ----------------- | ------------ | ---------------------------- |
| NI   | Not Implemented   | 0–15 %       | ❌ (Red — auto-fail)          |
| PI   | Partially         | >15–50 %     | ❌                            |
| LI   | Largely           | >50–90 %     | ✅                            |
| CI   | Completely        | >90–100 %    | ✅                            |
| NA   | Not Applicable    | —            | Excluded                     |

### 3.4 PM vs. SI Process

| Dimension         | PM Process                  | SI Process                       |
| ----------------- | --------------------------- | -------------------------------- |
| Activities        | 4                           | 6                                |
| Concern           | Cost, schedule, scope, risk | Requirements → working software  |
| Primary inputs    | Statement of Work           | Project Plan, Project Repository |
| Primary outputs   | Project Plan, Acceptance Record | Software, User Docs, Maintenance Docs |
| Rated under       | PM.1–PM.4                   | SI.1–SI.6                        |

### 3.5 ISO/IEC 29110 vs. CMMI (industry context)

| Dimension       | ISO/IEC 29110           | CMMI                          |
| --------------- | ----------------------- | ----------------------------- |
| Target          | VSEs (≤ 25 people)      | Any size, often large orgs    |
| Cost / overhead | **Low** — designed for small teams | High — heavy documentation |
| Levels          | Profiles (Entry, Basic, Intermediate, Advanced) | Maturity levels 1–5 |
| Assessment time | ~6 days                 | Weeks to months               |
| Best for        | Startups, small studios | Large defense/enterprise IT   |

---

## 4. Common Misconceptions & Traps

> [!danger] Trap 1 — "All goals satisfied = pass"
> A goal is `Satisfied` if there are **no findings**, **OR** if practices are LI/CI **AND** the aggregate weakness is not significant. Findings *can* exist and the goal can still be Satisfied — but **NI (red) artifacts auto-fail** the profile regardless.

> [!danger] Trap 2 — VSE size limit
> The threshold is **25 employees**, not 50, not 100. It applies to the *enterprise OR the software unit within a larger company*.

> [!danger] Trap 3 — Roles confusion
> - **CLA** *leads* the audit.
> - **COB** *observes and mentors* candidate CLAs — **does not lead**.
> - **ATM** *executes* document checks and ratings — **does not lead**.
> - The CB *certifies* but does not perform the day-to-day audit.

> [!danger] Trap 4 — Test design is in *Design*, not in *Testing*
> **Test Cases & Test Procedures** are produced during **Software Architectural & Detailed Design (SI.3)**, not during Integration & Tests (SI.5). Examiners love this.

> [!danger] Trap 5 — Process count
> PM has **4** activities; SI has **6**. Total: **10**. Some students confuse "10 activities" with "10 processes" — there are still only **2 processes**.

> [!danger] Trap 6 — Surveillance timing
> Surveillance happens at **365** and **730** days **from the registration date**, not from the assessment date. Reminder is sent at **90 days** before; confirmation required at **60 days** before.

> [!danger] Trap 7 — Number of projects assessed
> The standard's selection criteria require **2 projects**, not 1, not "all". Both must cover PM + SI.

> [!danger] Trap 8 — "LI is enough"
> Practices must be **LI or CI** (Largely OR Completely). A `PI` rating fails the practice. But *individual artifacts* can be LI without failing — what matters is **no Red anywhere**.

> [!danger] Trap 9 — Acceptance Record vs. Software Configuration
> - **Acceptance Record** = customer's signoff (the *transaction*).
> - **Software Configuration** = the controlled set of build/source artifacts (the *thing*).

> [!danger] Trap 10 — "Statement of Work" originates with whom?
> The **customer** issues the Statement of Work. The **VSE** produces the Project Plan in response.

---

## 5. High-Probability Exam Questions

> [!question] Q1. Maximum employee count for a VSE under ISO/IEC 29110
> **A.** 10  **B.** 25  **C.** 50  **D.** 100
>
> ✅ **Answer: B (25)**
> *Why others are wrong:* 10 is too restrictive (no source); 50 is the EU SME mid-tier threshold (different standard); 100 is the EU SME upper threshold. ISO/IEC 29110 explicitly targets ≤25.

> [!question] Q2. Which part of the standard defines the **goals/requirements** that determine pass/fail?
> **A.** ISO/IEC 29110-1 (Overview)  **B.** ISO/IEC 29110-4-1 (Profile Specification)  **C.** ISO/IEC 29110-5-1 (Engineering Guide)  **D.** ISO/IEC 29110-6 (Case Studies)
>
> ✅ **Answer: B**
> *Why others are wrong:* Part 1 is informative-only (vocabulary/overview). Part 5-1 tells you *how* (processes, artifacts) — not the requirements. Part 6 is illustrative case studies.

> [!question] Q3. A Goal in 29110-4-1 is rated **Satisfied** when…
> **A.** All practices are CI and there are no findings of any kind.
> **B.** No findings document weaknesses, OR (all practices are LI/CI AND weaknesses are not significant).
> **C.** The auditor's overall impression is favorable.
> **D.** ≥ 90 % of artifacts are scored CI.
>
> ✅ **Answer: B**
> *Why others are wrong:* A is too strict (overstates the bar). C is subjective and not standard-compliant. D conflates artifact-level and goal-level rating.

> [!question] Q4. How many activities does the **PM Process** contain?
> **A.** 2  **B.** 4  **C.** 6  **D.** 10
>
> ✅ **Answer: B (4)**
> *Why others are wrong:* 2 is the number of *processes* (PM + SI). 6 is the number of SI activities. 10 is PM + SI activities combined.

> [!question] Q5. **Test Cases and Test Procedures** are produced during which SI activity?
> **A.** Software Requirements Analysis
> **B.** Software Architectural and Detailed Design
> **C.** Software Construction
> **D.** Software Integration and Tests
>
> ✅ **Answer: B**
> *Why others are wrong:* Requirements Analysis produces the *Requirements Specification*, not test artifacts. Construction produces *Components*. Integration & Tests *executes* the tests and produces the *Test Report* — but the cases/procedures are designed earlier, in B, to match the architecture being tested.

> [!question] Q6. Which role **mentors candidate Lead Auditors**?
> **A.** CLA  **B.** ATM  **C.** COB  **D.** CB
>
> ✅ **Answer: C (COB — Certified Observer)**
> *Why others are wrong:* CLA leads but is the one being mentored *toward*, not the mentor of new candidates. ATM is a peer auditor. CB is the certifying authority, not a mentor.

> [!question] Q7. An assessment shows: 21 artifacts at CI, 1 artifact at NI (Red). What is the result?
> **A.** PASS — average score is well above 90 %.
> **B.** PASS — only 1 of 22 is non-conformant (4.5 %).
> **C.** FAIL — any single Red triggers Unsatisfied.
> **D.** Conditional PASS pending re-audit of the failing artifact.
>
> ✅ **Answer: C**
> *Why others are wrong:* A and B both fall for the "average" trap — the rating criterion is *categorical* ("LI/CI **and no red**"), not statistical. D invents a category that isn't in the rating table — the binary outcome is Satisfied / Unsatisfied.

> [!question] Q8. Surveillance #1 is conducted at what point relative to certification?
> **A.** 90 days  **B.** 180 days  **C.** 365 days  **D.** 730 days
>
> ✅ **Answer: C (365 days from registration date)**
> *Why others are wrong:* 90 days is the *reminder* lead time, not the surveillance itself. 180 days is not used. 730 days is Surveillance #2 / expiration.

> [!question] Q9. Which document is produced by the **customer** (not the VSE)?
> **A.** Project Plan  **B.** Statement of Work  **C.** Software Configuration  **D.** Test Report
>
> ✅ **Answer: B**
> *Why others are wrong:* A, C, D are all VSE outputs. The Statement of Work is the *input* from the customer that triggers PM Project Planning.

> [!question] Q10. A practice is rated PI (Partially Implemented, score 0.3). Can the goal it belongs to still be `Satisfied`?
> **A.** Yes — as long as no findings exist.
> **B.** Yes, automatically — PI counts as implemented.
> **C.** No — Path B requires all associated practices to be LI or CI.
> **D.** Only if the auditor waives the practice.
>
> ✅ **Answer: C**
> *Why others are wrong:* A misreads the OR/AND structure — Path A (no findings) is independent of the practice rating, but if findings *do* exist (which a PI typically implies), Path B kicks in and PI fails condition (a). B is a fabrication. D — auditors do not "waive" practices; they may rule items NA, which is different.

---

## 6. One-Page Cheat Sheet

> [!summary] Print this. Memorize this.

### Identity
- **ISO/IEC 29110** = lifecycle profile for **VSEs ≤ 25 people**, parent: ISO/IEC 12207.
- **Part 4-1** = *what* (Profile / goals).
- **Part 5-1** = *how* (Processes / 22 artifacts).

### Two Processes — 10 Activities
- **PM (4):** Planning → Plan Execution → Assessment & Control → Closure.
- **SI (6):** Initiation → Requirements → **Design (incl. Test Cases)** → Construction → Integration & Tests → Delivery.

### 22 Work Products — Memorize the 4 buckets
- **PM evidence (11):** SoW, Plan, Repository (+Backup), Meeting Rec, Progress Rec, Correction Reg, Change Req, Verification, Validation, Acceptance Rec.
- **Reqs/Design (4):** Reqs Spec, Design, Traceability, **Test Cases & Procedures**.
- **Build/Integration (4):** Components, Software, Software Config, Test Report.
- **Delivery (3):** Operation Guide, User Doc, Maintenance Doc.

### Ratings — The "No Red" Rule
| Code | Band | Pass-contributing? |
|------|------|--------------------|
| NI   | 0–15 % (Red) | ❌ auto-fail |
| PI   | >15–50 % | ❌ |
| LI   | >50–90 % | ✅ |
| CI   | >90–100 % | ✅ |
| NA   | — | excluded |

**Goal Satisfied ⇔** *(no findings)* **OR** *(all practices LI/CI **AND** weaknesses not significant)*.
**Profile PASS ⇔** All LI/CI **AND no Red**.

### Roles
- **CB** = certifies (authority).
- **CLA** = leads audit, owns contract.
- **ATM** = checks docs, rates artifacts.
- **COB** = mentors candidate CLAs.
- **CU** = customer (Sponsor, 1BPOC, Site-Co, Project Owner, Interviewees).

### Project Selection
- **2 projects**, status Go-live/Maintenance/Closed-by-Condition within **3–6 months**.
- Must cover **PM + SI** and map to all **22 work products**.
- Interviewees: **PM, SA/BA, Developer, Tester**.

### Schedule
- **Assessment:** Kickoff (1) + Confirm (1) + Pre-Onsite Doc Review (2) + Onsite Interview (2) = **6 days**.
- **Final Report:** ≤ 60 days.
- **Feedback to CB:** ≤ 30 days.
- **Surveillance #1:** day **365**.
- **Surveillance #2 / Expiration:** day **730**.
- **Reminder:** 90 days before; **Confirmation:** 60 days before.

### Numbers to Memorize
| Number | Meaning |
|--------|---------|
| 25     | Max VSE employees |
| 2      | Required processes (PM, SI) |
| 4      | PM activities |
| 6      | SI activities |
| 10     | Total activities |
| 22     | Required work products |
| 2      | Projects assessed |
| 6      | Days of assessment |
| 60 / 30 | Final report / feedback deadlines (days) |
| 365 / 730 | Surveillance #1 / #2 (days from registration) |
| 90 / 60 | Reminder / confirmation lead times (days) |

---

## Appendix — Suggested Obsidian Workflow

> [!tip] How to use this in Obsidian
> 1. Drop this file into your vault.
> 2. Use `Ctrl/Cmd + O` to jump between sections via the heading outline.
> 3. The callouts (`> [!note]`, `> [!danger]`, `> [!question]`) render as colored boxes — use them to scan visually 1 hour before the exam.
> 4. Tag any question you got wrong with `#review-before-exam` and use `Ctrl/Cmd + Shift + F` to filter.
