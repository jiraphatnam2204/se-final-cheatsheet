# 📚 Software Quality Assurance — Exam Study Guide
> **Exam in 3 days** | Based on Pressman's *Software Engineering: A Practitioner's Approach*, 8th Ed.
> Optimized for Obsidian: use **Ctrl+Click** on links, enable Reading View for best formatting.

---

## 1. 🔑 Core Concepts

| Term | Precise Definition |
|---|---|
| **Software Quality Assurance (SQA)** | A set of organizational activities encompassing process management, formal reviews, testing strategy, documentation control, standards compliance, and measurement — designed to *prevent* defects before they occur. |
| **Quality of Design** | The set of characteristics that designers intentionally specify for a product (e.g., materials grade, tolerances, performance specifications). It represents *intended* quality. |
| **Quality of Conformance** | The degree to which the final product adheres to its design specifications during manufacturing or development. It measures *actual* adherence to intent. |
| **Quality Control (QC)** | A **reactive** process consisting of inspections, reviews, and tests applied to a finished or in-progress work product to **identify and correct** defects. Includes a feedback loop to the producing process. |
| **Quality Assurance (QA)** | A **proactive** process of analysis, auditing, and reporting activities focused on the **development process** itself, aiming to prevent defects from being introduced. |
| **Formal Technical Review (FTR)** | A structured software quality activity conducted as a formal meeting (3–5 participants, ≤2 hrs) to uncover errors, verify requirements compliance, and ensure adherence to standards. Includes both **Inspection** and **Walkthrough** as subtypes. |
| **Inspection** | The most formal and most effective subtype of FTR; follows a rigidly defined process with clear roles, checklists, and a planned schedule. |
| **Walkthrough** | A formal but slightly less rigid FTR subtype; the producer guides reviewers through the work product. Still classified as FTR. |
| **SQA Plan (IEEE Std 730-1984)** | A documented plan governing all quality assurance activities for a project, including evaluations, audits, applicable standards, error reporting procedures, and feedback mechanisms. |
| **Cost of Quality** | The total cost associated with achieving quality, decomposed into: **prevention costs**, **appraisal costs**, and **failure costs** (internal + external). |
| **Defect Removal Efficiency (DRE)** | A metric measuring the effectiveness of pre-delivery error removal: `DRE = E / (E + D)`, where E = errors found before delivery, D = defects found after delivery. Ideal value = **1**. |
| **Correctness** | The degree to which software operates according to its specification. Measured by **defects per KLOC**. |
| **Maintainability** | The ease with which software can be corrected, adapted, or enhanced. Measured indirectly by **MTTC** (Mean Time To Change) and **MTTR** (Mean Time To Repair). |
| **Integrity** | The degree to which a system resists unauthorized access or attack. Computed via: `Integrity = Σ[1 – (threat × (1 – security))]`. |
| **Usability** | The degree to which a system is easy to learn and use. Measured by learning time, efficiency gain, and subjective assessment. |
| **Availability** | The probability that software is operational at a given point in time. Formula: `Availability = [MTTF / (MTTF + MTTR)] × 100%`. |
| **Reliability** | The ability of software to perform its required function without failure over time. Measured by **MTBF = MTTF + MTTR**. |
| **MTTF** | Mean Time To Failure — average time the system operates correctly before the next failure. |
| **MTTR** | Mean Time To Repair — average time required to restore the system after a failure. |
| **MTBF** | Mean Time Between Failure — total cycle time: `MTBF = MTTF + MTTR`. |
| **Productivity Metrics** | Metrics measuring software development output as a function of effort and time applied. |
| **Quality Metrics** | Metrics measuring the "fitness for use" of produced work products. |
| **Process Indicators** | Metrics that provide insight into the efficacy of the software process, used to drive process improvement. |
| **Project Indicators** | Metrics that assess ongoing project status, uncover critical problem areas, and allow task adjustment. |
| **McCall's Quality Factors** | A classification of software quality across three viewpoints: **Product Operation**, **Product Revision**, and **Product Transition**. |

---

## 2. ⚙️ Mechanisms & How Things Work

### 2.1 The SQA Framework (End-to-End)

```
1. Define software process & standards
2. Conduct Formal Technical Reviews (FTRs) at each phase
3. Apply multitiered testing strategy
4. Control documentation and change records
5. Ensure standards compliance via audits
6. Collect measures → compute metrics → evaluate indicators → improve process
```

### 2.2 How Cost of Quality Decomposes

```
Cost of Quality
├── Prevention Costs     → Quality planning, test equipment, training
├── Appraisal Costs      → Reviews, inspections, calibration, testing
└── Failure Costs
    ├── Internal         → Rework, repair (found before delivery)
    └── External         → Complaints, returns, warranty, help support (found after delivery)
```
> **Key insight:** The later a defect is found, the **exponentially higher** the correction cost. A defect found in Field Operation costs 40–1000× more than one found during Requirements.

### 2.3 Relative Cost of Correcting Errors by Phase

| Phase | Relative Cost Multiplier |
|---|---|
| Requirements (Reg.) | 1× (baseline) |
| Design | 3–6× |
| Code | 10× |
| Development Test | 15–40× |
| System Test | 30–70× |
| Field Operation | **40–1000×** |

### 2.4 FTR Meeting Process

```
Step 1: Producer distributes work product materials to reviewers
Step 2: Each reviewer prepares independently (≤2 hours)
        → Skim for structure, then annotate; pose comments as questions
Step 3: Review meeting is held (≤2 hours, 3–5 participants)
        → Review leader facilitates; recorder documents issues
        → Raise issues; do NOT resolve them during the meeting
Step 4: Review summary report produced
        → Answers: What was reviewed? Who reviewed it? Findings & conclusions?
Step 5: Identified errors are tracked and resolved post-meeting
```

**Review Team Roles:**
- **Review Leader** — Plans, facilitates, and controls the meeting
- **Producer** — Author of the work product under review
- **Reviewer(s)** — Technical evaluators who identify defects
- **Recorder** — Documents all identified issues and decisions

### 2.5 Software Metrics Collection Process

```
Inputs: Software Engineering Process + Software Project + Software Product
        ↓
  [Data Collection]
        ↓ (measures)
  [Metrics Computation]
        ↓ (metrics)
  [Metrics Evaluation]
        ↓ (indicators)
  Decision: Improve process / Adjust project / Accept product
```

### 2.6 Integrity Calculation

For each threat type *i*:
```
Per-threat integrity_i = 1 – (threat_i × (1 – security_i))
Overall Integrity       = Σ [1 – (threat_i × (1 – security_i))]
```
**Example:** Threat = 0.25, Security = 0.95
→ `Integrity = 1 – (0.25 × (1 – 0.95)) = 1 – (0.25 × 0.05) = 1 – 0.0125 = 0.9875`

### 2.7 SQA Plan Structure (IEEE Std 730-1984)

| Section | Content |
|---|---|
| **Initial** | Purpose, scope, processes covered |
| **Reference** | All applicable documents and standards |
| **Management** | SQA organizational placement, tasks, roles, responsibilities |
| **Document** | Work products produced, minimum set of deliverables |
| **Standards, Practices, Conventions** | Applicable standards, metrics to collect |
| **Reviews and Audits** | Identifies reviews and audits to be conducted |
| **Test** | Reference to test plan, test record-keeping |
| **Problem Reporting & Corrective** | Error/defect reporting, tracking, resolving procedures; responsibility assignments |

---

## 3. ⚖️ Comparisons & Trade-offs

### 3.1 QA vs. QC

| Dimension | Quality Assurance (QA) | Quality Control (QC) |
|---|---|---|
| **Nature** | Proactive | Reactive |
| **Focus** | The *process* | The *product* |
| **Goal** | Prevent defects | Identify & correct defects |
| **Timing** | Throughout development | After or during production |
| **Activities** | Auditing, analysis, reporting | Inspections, reviews, testing |
| **Analogy** | Building the right fence before the cow escapes | Chasing the cow after it escapes |

### 3.2 Review Types by Formality & Effectiveness

| Rank (Most → Least Effective) | Review Type | Formality Level |
|---|---|---|
| 1 | **Inspection (FTR)** | Highest — planned schedule, checklists, defined roles |
| 2 | **Walkthrough (FTR)** | High — structured but producer-led |
| 3 | **Formal Presentation** | Medium-High |
| 4 | **Informal Presentation** | Medium |
| 5 | **Peer Group Review** | Low-Medium |
| 6 | **Casual Conversation** | Lowest |

### 3.3 McCall's Quality Factor Viewpoints

| Viewpoint | Quality Factors |
|---|---|
| **Product Operation** (using it) | Correctness, Reliability, Usability, Integrity, Efficiency |
| **Product Revision** (changing it) | Maintainability, Flexibility, Testability |
| **Product Transition** (porting it) | Portability, Reusability, Interoperability |

### 3.4 Development Cost: Reviews Conducted vs. Not Conducted

| Condition | Errors Found | Avg Cost/Error | Total Cost |
|---|---|---|---|
| Reviews conducted — during design | 22 | 1.5 | 33 |
| Reviews conducted — before test | 36 | 6.5 | 234 |
| Reviews conducted — during test | 15 | 15 | 315 |
| Reviews conducted — after release | 3 | 67 | 201 |
| **Reviews conducted TOTAL** | **76** | — | **783** |
| No reviews — before test | 22 | 6.5 | 143 |
| No reviews — during test | 82 | 15 | 1230 |
| No reviews — after release | 12 | 67 | 804 |
| **No reviews TOTAL** | **116** | — | **2177** |

> **Conclusion:** Conducting reviews reduces total defect correction cost by ~64% in this dataset. Errors are caught earlier (cheaper phases) and fewer defects reach later (expensive) phases.

### 3.5 MTTF vs. MTTR vs. MTBF

| Metric | Measures | Want It To Be |
|---|---|---|
| MTTF | Time until failure (uptime) | **High** |
| MTTR | Time to restore after failure | **Low** |
| MTBF | Full failure cycle = MTTF + MTTR | **High** |

---

## 4. ⚠️ Common Misconceptions & Exam Traps

> **Read these carefully — these are the most likely sources of wrong-answer choices.**

### Trap 1: Confusing QA and QC
- **Trap:** QA tests the product; QC manages the process.
- **Correct:** It's the **opposite**. QA = process-focused, proactive. QC = product-focused, reactive.

### Trap 2: Walkthrough is NOT QC — it is FTR (which is QA)
- FTR (including both Inspection and Walkthrough) is a **QA activity**, not QC.

### Trap 3: Inspection vs. Walkthrough
- Both are FTR. **Inspection** is more formal (stricter planning, checklists).
- **Walkthrough** is still classified as FTR, not informal.

### Trap 4: DRE = 1 is ideal, not DRE = 0
- Higher DRE = more errors caught before delivery = better.
- `DRE = E/(E+D)` → ideally all defects are found pre-delivery → D=0 → DRE=1.

### Trap 5: MTBF ≠ MTTF
- `MTBF = MTTF + MTTR` — a common confusion.
- MTBF is the **full cycle** between failure starts; MTTF is only the **operational uptime** portion.

### Trap 6: Integrity formula direction
- `Integrity = 1 – (threat × (1 – security))` per threat type.
- Higher security → (1 – security) is smaller → integrity is higher. This is directionally correct but easy to invert under exam pressure.

### Trap 7: "Raise issues, don't resolve them" in FTR
- The meeting is for **identifying** problems, not for debugging or designing solutions. Resolution happens separately.

### Trap 8: SQA group does NOT report to the development team
- The SQA group maintains **independence** from the software engineering team precisely to provide unbiased oversight. They report to senior management, not the project team.

### Trap 9: Correctness is measured in defects/KLOC, not MTTR
- MTTR measures **Maintainability**, not Correctness.

### Trap 10: Cost of quality — "appraisal" ≠ "failure"
- Appraisal = reviews, inspections, calibration (catching before release).
- Failure costs = rework (internal) + warranty/complaints (external). These are distinctly separate categories.

---

## 5. 🎯 High-Probability Exam Questions

### Q1
**Which of the following best describes the distinction between Quality Assurance and Quality Control?**

- A) QA is reactive; QC is proactive
- B) QA focuses on the finished product; QC focuses on the process
- C) QA is proactive and process-focused; QC is reactive and product-focused ✅
- D) QA and QC are identical in scope but differ only in timing

**Explanation:**
- A is reversed.
- B is reversed.
- C is correct by definition.
- D is false — they differ in both nature and focus.

---

### Q2
**A software system has MTTF = 400 hours and MTTR = 8 hours. What is its availability?**

- A) 98.0%
- B) 96.2%
- C) 98.04% ✅
- D) 50.0%

**Explanation:**
`Availability = [MTTF / (MTTF + MTTR)] × 100 = [400 / 408] × 100 ≈ 98.04%`
- A and B use incorrect denominators.
- D reflects no understanding of the formula.

---

### Q3
**A system faces two threat types. Threat1 has probability 0.3 with security 0.9. Threat2 has probability 0.5 with security 0.8. What is the system's overall integrity?**

- A) 0.85
- B) 1.67
- C) 0.97 ✅
- D) 0.73

**Explanation:**
- `Integrity1 = 1 – (0.3 × 0.1) = 1 – 0.03 = 0.97`
- `Integrity2 = 1 – (0.5 × 0.2) = 1 – 0.10 = 0.90`
- Overall = 0.97 + 0.90 = 1.87 → *Note: the formula sums per-threat values; if the exam expects the summed result, check phrasing.*
- If the question asks for Integrity for **Threat1 only**: answer is **0.97** ✅

---

### Q4
**Before delivery, 80 errors are found. After delivery, 20 defects are reported. What is the Defect Removal Efficiency (DRE)?**

- A) 0.20
- B) 0.80 ✅
- C) 4.00
- D) 0.25

**Explanation:**
`DRE = E / (E + D) = 80 / (80 + 20) = 80 / 100 = 0.80`
- A confuses D/(E+D).
- C inverts the formula.
- D is E/D only.

---

### Q5
**Which review type is considered MOST effective and MOST formal in the effectiveness scale?**

- A) Walkthrough
- B) Peer Group Review
- C) Informal Presentation
- D) Inspection ✅

**Explanation:**
Inspection is ranked highest on the formality/effectiveness scale. Walkthrough is also FTR but slightly less formal. Peer review and informal presentation rank lower.

---

### Q6
**Which cost category does "rework after a bug is found internally before release" belong to?**

- A) Prevention cost
- B) Appraisal cost
- C) Internal failure cost ✅
- D) External failure cost

**Explanation:**
Internal failure costs arise from defects discovered **before delivery** (rework, repair). External failure costs arise **after delivery** (returns, warranty, complaints). Prevention and appraisal are pre-defect investment categories.

---

### Q7
**Why does the SQA group NOT report directly to the software engineering project team?**

- A) SQA lacks the technical skill to understand the project
- B) SQA must maintain independence to provide unbiased quality oversight ✅
- C) SQA only reports to customers, not internal teams
- D) It is purely an administrative convention with no functional significance

**Explanation:**
Independence is the operational rationale. If SQA reported to the development team, it would be subject to the same schedule pressures and managerial incentives that compromise quality decisions.

---

### Q8
**In an FTR meeting, a reviewer identifies an architectural flaw. What is the correct action?**

- A) Immediately redesign the architecture during the meeting
- B) Assign the fix to the producer and extend the meeting until resolved
- C) Record the issue and continue; resolution happens outside the meeting ✅
- D) Discard the issue if it cannot be resolved within the 2-hour window

**Explanation:**
FTR guidelines explicitly state: **raise issues, do not resolve them**. The meeting records defects; a separate post-review activity handles fixes.

---

### Q9
**A project measured 45 defects per KLOC. Which McCall quality factor does this metric directly address?**

- A) Maintainability
- B) Reliability
- C) Correctness ✅
- D) Integrity

**Explanation:**
Correctness is explicitly measured by **defects/KLOC**. Maintainability uses MTTC/MTTR. Reliability uses MTBF. Integrity uses the threat/security formula.

---

### Q10
**Which of the following is a Project Indicator, as opposed to a Process Indicator?**

- A) Average defect injection rate across all organizational projects over 5 years
- B) Organization-wide test coverage baseline
- C) Current sprint velocity deviation from estimated story points ✅
- D) Industry benchmark for code review defect density

**Explanation:**
Project indicators assess the **status of an ongoing project** and uncover problem areas before they go critical. Process indicators provide organizational insight to improve the general software process. Options A, B, D are process/organizational level metrics.

---

## 6. 📋 One-Page Cheat Sheet

### SQA Fundamentals
- **QA = Proactive, process-focused** | **QC = Reactive, product-focused**
- SQA encompasses: quality management, FTRs, testing strategy, documentation control, standards compliance, measurement
- SQA group is **independent** of the SE project team → reports to senior management

### Cost of Quality
- **Prevention** → training, planning
- **Appraisal** → reviews, inspections, testing
- **Internal Failure** → rework (pre-delivery)
- **External Failure** → warranty, complaints (post-delivery)
- Error correction cost grows **exponentially** from Requirements (1×) to Field (40–1000×)

### Formal Technical Reviews (FTR)
- Includes: **Inspection** (most formal) > **Walkthrough** > formal presentation > informal > peer > casual
- Constraints: **3–5 people**, prep ≤2 hrs, meeting ≤2 hrs
- Roles: Leader, Producer, Reviewer(s), Recorder
- Rule: **Raise issues; do NOT resolve them during the meeting**
- Output: Summary report (What? Who? Findings?)

### Key Formulas
```
DRE         = E / (E + D)                            → ideal = 1
Integrity_i = 1 – (threat_i × (1 – security_i))
Availability= [MTTF / (MTTF + MTTR)] × 100%
MTBF        = MTTF + MTTR
```

### Quality Metrics (McCall)
| Metric | Measurement |
|---|---|
| Correctness | Defects / KLOC |
| Maintainability | MTTC, MTTR |
| Integrity | Threat × Security formula |
| Usability | Learning time, subjective |
| Availability | MTTF / (MTTF + MTTR) |
| Reliability | MTBF = MTTF + MTTR |

### McCall's Three Viewpoints
- **Operation**: Correctness, Reliability, Usability, Integrity, Efficiency
- **Revision**: Maintainability, Flexibility, Testability
- **Transition**: Portability, Reusability, Interoperability

### SQA Plan Sections (IEEE Std 730-1984)
Initial → Reference → Management → Document → Standards → Reviews/Audits → Test → Problem Reporting

### Metrics Process
`Data Collection → Measures → Metrics Computation → Metrics → Metrics Evaluation → Indicators`

### Critical Traps to Avoid on Exam
1. QA ≠ QC — don't flip them
2. Both Inspection AND Walkthrough are FTR (= QA, not QC)
3. DRE ideal = 1, not 0
4. MTBF = MTTF + MTTR (not equal to MTTF alone)
5. Correctness → defects/KLOC | Maintainability → MTTR/MTTC (don't swap)
6. In FTR: **raise issues, don't resolve them**
7. SQA group ≠ part of SE team (independence is the point)
