# 📚 Software Configuration Management — Exam Study Guide
> **Exam in:** 3 Days | **Source:** SE Slide 15 (Pressman 8th Ed.)
> **Tags:** #SoftwareEngineering #SCM #ExamPrep

---

## 1. Core Concepts

### Software Configuration Management (SCM)
The discipline of **identifying, organizing, and controlling modifications** to software artifacts throughout the entire software lifecycle. Its primary goal is to maximize productivity by minimizing errors introduced by uncontrolled change.

> ⚠️ SCM begins when a project **starts** and terminates when the software is **taken out of operation** — not just during development.

---

### Software Configuration
The **collective set of all outputs** produced during the software process, comprising:
- Computer programs (source code, executables)
- Documents describing the programs (requirements, design specs, manuals)
- Data (test cases, recorded results)

---

### Software Configuration Item (SCI)
Any **discrete unit of information** created as part of the software engineering process that is subject to version control and change management.

## Examples of SCIs

| Category       | Examples                                                                        |
| -------------- | ------------------------------------------------------------------------------- |
| Planning       | Project plan                                                                    |
| Requirements   | Requirements specification, graphical analysis models                           |
| Design         | Data design, architectural design, module design, interface design descriptions |
| Implementation | Source code                                                                     |
| Testing        | Test plan, test procedures, test cases, recorded results                        |
| Delivery       | Operation manual, user manual                                                   |

**SCI Identification Attributes:**
Each SCI must be described with:
- **Name** — unique identifier
- **Description** — includes SCI type (document / program / data), project identifier, change/version information
- **Resource list** — what the object provides, processes, references, or requires

---

### Basic Object vs. Aggregate Object
| Type | Definition |
|---|---|
| **Basic Object** | A unit of text created during a single phase (e.g., one source file, one spec section) |
| **Aggregate Object** | A collection of basic objects and/or other aggregate objects (e.g., the full design specification) |

---

### Baseline
A **formally reviewed and agreed-upon specification** of an SCI (or set of SCIs) that serves as the foundation for further development. Changes to a baseline may only be made through **formal change control procedures**.

- A baseline is established at a **milestone**, approved via **Formal Technical Review (FTR)**
- Baselines enable traceability and rollback across the lifecycle

**Baseline Progression (Lifecycle Mapping):**
```
System Engineering     → System Specification (baseline)
Requirements Analysis  → Software Requirements Specification (baseline)
Software Design        → Design Specification (baseline)
Coding                 → Source Code (baseline)
Testing                → Test Plans / Procedures / Data (baseline)
Release                → Operational System (baseline)
```

---

### Formal Technical Review (FTR)
A structured peer review process used to **evaluate SCIs** before they are approved and stored as a baseline. FTRs are distinct from — but complementary to — configuration audits.

---

### Software Configuration Audit
A verification activity that **complements the FTR** by checking aspects not covered during review. It answers:
- Has the change been made as specified?
- Has a review been conducted?
- Have SCM procedures (notation, recording, reporting) been followed?
- Have all related SCIs been updated?
- Have the change date and author been recorded?

---

### Configuration Status Reporting (CSR)
Also called **status accounting**. A CSR entry is created whenever:
1. An SCI is assigned new or updated identification
2. A change is approved by the change control authority
3. A configuration audit is conducted

Output is typically stored in an **on-line database** for project-wide visibility.

---

### Evolution Graph
A directed graph representing the **change history** of a specific SCI object across versions. Branches represent divergent development lines (e.g., a bug-fix branch and a feature branch derived from the same version).

---

## 2. Mechanisms & How Things Work

### The SCI Lifecycle in a Project Database

```
Software Engineering Tasks
         │
         ▼
       [SCIs]
         │
         ▼
  Formal Technical Reviews
    ┌────┴────┐
    │         │
[rejected] [approved]
    │         │
[modified]  stored in Project Database
    │              │
    └──────────────┤
              extracted by SCM controls
                   │
                   ▼
         back to engineering tasks
```

**Key insight:** SCIs cycle through engineering → review → approval → storage → extraction for modification. This loop enforces controlled change.

---

### Change Control Process (Three-Stage Pipeline)

#### Stage I — Request Evaluation
1. A **need for change** is recognized (by user or developer)
2. A **Change Request** is submitted
3. Developer evaluates the change
4. A **Change Report** is generated
5. **Change Control Authority (CCA)** decides:
   - **Approved** → proceed to Stage II
   - **Denied** → user is notified; request closed

#### Stage II — Implementation
1. People are assigned to the affected SCIs
2. SCI is **checked out** from the project database (like `git checkout`)
3. The change is **made**
4. Change is **reviewed/audited**
5. A **new baseline** is established for testing purposes

#### Stage III — Integration & Release
1. **SQA and testing** activities are performed
2. Changed SCI is **checked in** (like `git commit/push`)
3. SCI is **promoted** for inclusion in the next release
4. The **appropriate version is rebuilt**
5. Change is **reviewed/audited** again
6. **All changes are included** in the final release

> **Analogy to Git:** Check-out ≈ `git branch` + `git checkout`, Check-in ≈ `git commit` + `git push` to main, Baseline ≈ a tagged release commit.

---

### Sources of Change (Why Changes Occur)
Understanding the root cause matters for exam questions about SCM motivation:
- New business conditions or market pressures
- New customer requirements (functional or data-related)
- Organizational restructuring (team changes)
- Budget and schedule constraints

---

## 3. Comparisons & Trade-offs

### SCM vs. Software Maintenance

| Dimension | SCM | Software Maintenance |
|---|---|---|
| **Scope** | Entire software lifecycle | Post-delivery phase only |
| **When it starts** | When project begins | After software is delivered |
| **When it ends** | When software is retired | When software is retired |
| **Purpose** | Control and track changes during development | Correct, adapt, and enhance delivered software |
| **Overlap?** | Yes — SCM supports maintenance activities | Maintenance generates new SCIs managed by SCM |

> **Key trap:** Maintenance ≠ SCM. SCM is active *throughout*; maintenance begins *after delivery*.

---

### Basic Object vs. Aggregate Object

| Dimension | Basic Object | Aggregate Object |
|---|---|---|
| **Granularity** | Single unit of text (one phase) | Collection of basic/aggregate objects |
| **Example** | One source file, one spec section | Full design spec, entire test suite |
| **Versioning** | Individual versioning | Versioned as a composite |

---

### FTR vs. Configuration Audit

| Dimension | Formal Technical Review (FTR) | Configuration Audit |
|---|---|---|
| **Primary focus** | Technical correctness and quality of the SCI | Process compliance and completeness of change documentation |
| **When conducted** | Before an SCI becomes a baseline | After a change is implemented |
| **Who is responsible** | Peer engineers | SCM team / QA |
| **Output** | Approval or rejection of SCI | Audit report confirming SCM compliance |

---

## 4. Common Misconceptions & Traps

> These are the most likely sources of **wrong answer choices** in a multiple-choice exam.

| #   | Misconception                                                  | Correct Understanding                                                                              |
| --- | -------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| 1   | SCM starts after software is delivered                         | SCM begins when the project **starts** and ends when the software is **retired**                   |
| 2   | Software Maintenance and SCM are the same                      | They are **different**; maintenance is post-delivery; SCM spans the full lifecycle                 |
| 3   | A baseline can be changed by any developer at any time         | Baselines can **only** be changed through **formal change control procedures**                     |
| 4   | A Change Request being submitted means the change will be made | The CCA must **approve** it; the request can be **denied**                                         |
| 5   | FTR and Configuration Audit serve the same purpose             | FTR checks technical quality; audit checks **SCM process compliance**                              |
| 6   | CSR is only triggered when a change is approved                | CSR entries are created for **three events**: new ID assignment, approval, and audit               |
| 7   | SCIs only include source code                                  | SCIs include plans, specs, designs, test artifacts, and manuals — **all software process outputs** |
| 8   | An aggregate object is just a big file                         | An aggregate object is a **structured collection** of basic and/or other aggregate objects         |
| 9   | The evolution graph shows future planned changes               | It shows the **historical change record** of an object                                             |
| 10  | Configuration audit replaces FTR                               | Audit **complements** FTR — it does not replace it                                                 |

---

## 5. High-Probability Exam Questions

---

**Q1.** According to the IEEE definition, which of the following is a necessary condition for a specification to be considered a "baseline"?

- A) It has been written by a senior engineer
- B) It has been formally reviewed and approved through formal change control
- C) It has been deployed to a production environment
- D) It has passed all unit tests

> ✅ **Answer: B**
> A baseline is formally reviewed, agreed upon, and can only be changed via formal change control procedures. A — seniority is not a criterion. C — baselines exist before deployment. D — testing is a separate concern.

---

**Q2.** Which of the following correctly describes the relationship between SCM and software maintenance?

- A) SCM and maintenance are the same activity, just named differently
- B) SCM begins after maintenance ends
- C) Maintenance occurs after delivery; SCM spans the entire software lifecycle
- D) SCM replaces the need for maintenance

> ✅ **Answer: C**
> The slide explicitly states: "Maintenance occurs after delivery; SCM begins when a software project begins." A, B, D directly contradict this.

---

**Q3.** A developer wants to modify a baselined SCI. What is the correct first action under SCM?

- A) Directly edit the SCI in the project database
- B) Submit a change request and wait for Change Control Authority approval
- C) Notify the project manager via email and proceed
- D) Create a new SCI with a different name

> ✅ **Answer: B**
> The Change Control Process Stage I requires a change request → evaluation → CCA decision before any modification. A — direct editing bypasses all controls. C — informal notification is not sufficient. D — creating a new SCI avoids the traceability mechanism.

---

**Q4.** Which of the following is NOT a valid Software Configuration Item (SCI)?

- A) Test procedures and recorded results
- B) Architectural design specification
- C) A developer's personal to-do list
- D) The project plan

> ✅ **Answer: C**
> SCIs are formally created artifacts of the software engineering process. A personal to-do list is not part of any formal software engineering output. A, B, D are all explicitly listed as SCIs.

---

**Q5.** In the Baselines SCIs lifecycle diagram, what happens to an SCI after it is extracted from the project database?

- A) It is immediately promoted to the next release
- B) It is placed under SCM controls and returned to software engineering tasks for modification
- C) It is permanently deleted from the database
- D) It is automatically approved as the new baseline

> ✅ **Answer: B**
> Extracted SCIs go through SCM controls and re-enter the engineering task loop for modification. A — promotion comes much later. C — deletion is not part of the flow. D — approval requires FTR, not automatic on extraction.

---

**Q6.** What distinguishes a Configuration Status Report (CSR) entry trigger from an FTR?

- A) CSR is triggered by technical quality failures; FTR is triggered by approvals
- B) CSR records administrative change events (ID updates, approvals, audits); FTR evaluates technical correctness
- C) CSR and FTR are triggered by the same events
- D) CSR replaces FTR in agile projects

> ✅ **Answer: B**
> CSR (status accounting) is event-driven by administrative milestones. FTR is a technical peer review. A — CSR is not triggered by failures. C — triggers are distinct. D — the slide makes no such claim.

---

**Q7.** An evolution graph for an SCI object shows branching from version 1.2 into 1.3 and 2.0 simultaneously. What does this represent?

- A) A critical error that must be corrected immediately
- B) Two independent lines of development (e.g., a maintenance branch and a new major version) derived from the same baseline
- C) That version 1.3 is a prerequisite for version 2.0
- D) The system has been released twice under different names

> ✅ **Answer: B**
> Evolution graphs model change history, and branching represents divergent development paths from a common baseline — a standard SCM pattern. A — branching is expected and controlled. C — the graph shows derivation, not dependency. D — there is no naming change implied.

---

**Q8.** At which stage of the Change Control Process is the "Check-out" operation performed?

- A) Stage I — when the change request is submitted
- B) Stage II — after the CCA approves the change
- C) Stage III — after testing is complete
- D) Before any stage, as part of project initialization

> ✅ **Answer: B**
> Check-out occurs in Stage II after CCA approval. Stage I is evaluation only. Stage III is for check-in. D is incorrect — check-out is change-specific, not a one-time initialization.

---

**Q9.** Which of the following events does NOT trigger a Configuration Status Reporting (CSR) entry?

- A) An SCI is assigned a new identification
- B) A change request is submitted by a user
- C) A configuration audit is conducted
- D) A change is approved by the CCA

> ✅ **Answer: B**
> The slide specifies three CSR triggers: new/updated ID assignment, change approval, and audit completion. A change request *submission* alone does not trigger a CSR entry — only *approval* does.

---

**Q10.** Which statement most accurately reflects the purpose of a Software Configuration Audit relative to an FTR?

- A) The audit replaces the FTR for complex SCIs
- B) The audit checks process compliance for aspects not covered during the FTR
- C) The FTR is performed after the audit to verify technical quality
- D) Both serve identical purposes but are performed by different teams

> ✅ **Answer: B**
> The slide states the audit "complements the FTR by assessing configuration objects not considered during review." A — no replacement relationship. C — sequence is inverted (FTR before audit). D — their purposes are explicitly different.

---

## 6. One-Page Cheat Sheet

```
╔══════════════════════════════════════════════════════════════════╗
║         SCM — RAPID REFERENCE CHEAT SHEET                       ║
╠══════════════════════════════════════════════════════════════════╣
║ WHAT IS SCM?                                                     ║
║  • Identify, organize, control modifications to SW artifacts     ║
║  • Goal: maximize productivity, minimize change-induced errors   ║
║  • Active from project START to software RETIREMENT              ║
║  • SCM ≠ Maintenance (maintenance = post-delivery only)          ║
╠══════════════════════════════════════════════════════════════════╣
║ KEY TERMS                                                        ║
║  • SCI  = any formal artifact (plan, spec, code, test, manual)  ║
║  • Basic Object = single unit of text from one phase            ║
║  • Aggregate Object = collection of basic/other aggregates      ║
║  • Baseline = formally reviewed+approved SCI; change only via   ║
║               formal change control procedures                   ║
║  • Evolution Graph = change history tree for an SCI             ║
╠══════════════════════════════════════════════════════════════════╣
║ BASELINE LIFECYCLE SEQUENCE                                      ║
║  System Spec → SW Req Spec → Design Spec → Source Code          ║
║  → Test Plans/Procedures/Data → Operational System              ║
╠══════════════════════════════════════════════════════════════════╣
║ CHANGE CONTROL PROCESS (3 STAGES)                                ║
║  I.  Request → Evaluate → CCA Decision (approve or deny)        ║
║  II. Assign → Check-OUT → Modify → Review → New baseline        ║
║  III.Test (SQA) → Check-IN → Promote → Rebuild → Release        ║
╠══════════════════════════════════════════════════════════════════╣
║ SCI DATABASE LIFECYCLE                                           ║
║  Engineering Tasks → SCIs → FTR → [approved] → Project DB       ║
║  Project DB → [extracted] → SCM Controls → back to Engineering   ║
╠══════════════════════════════════════════════════════════════════╣
║ FTR vs. AUDIT                                                    ║
║  FTR   = technical correctness review (before baseline)         ║
║  Audit = process compliance check (after change; complements FTR)║
╠══════════════════════════════════════════════════════════════════╣
║ CSR TRIGGERS (3 events)                                          ║
║  1. SCI assigned new/updated identification                      ║
║  2. Change APPROVED (not just submitted)                         ║
║  3. Configuration audit conducted                                ║
╠══════════════════════════════════════════════════════════════════╣
║ SOURCES OF CHANGE                                                ║
║  • New business/market conditions                                ║
║  • New customer requirements (functional or data)               ║
║  • Team/organizational restructuring                            ║
║  • Budget and schedule constraints                              ║
╠══════════════════════════════════════════════════════════════════╣
║ CRITICAL TRAPS                                                   ║
║  ✗ SCM starts after delivery  →  ✓ starts at project inception   ║
║  ✗ Baseline = any reviewed doc →  ✓ must pass formal FTR        ║
║  ✗ Change request = change approved → ✓ CCA must approve        ║
║  ✗ Audit replaces FTR          →  ✓ audit COMPLEMENTS FTR       ║
║  ✗ SCI = only source code     →  ✓ all SW process artifacts     ║
║  ✗ CSR triggered by submission →  ✓ triggered by APPROVAL       ║
╚══════════════════════════════════════════════════════════════════╝
```

---

*Study Guide generated from SE Slide 15 — Pressman 8th Edition, Chapter on Software Configuration Management.*
