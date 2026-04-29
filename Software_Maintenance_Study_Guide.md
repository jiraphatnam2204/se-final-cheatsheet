# 📚 Software Maintenance — Exam Study Guide

> **Exam in 3 days** | Source: ISO/IEC 14764:2022 · Pressman & Maxim · Rajlich & Bennett
> Tags: #SoftwareEngineering #Maintenance #ExamPrep

---

## 1. Core Concepts

### Software Maintenance
> *The totality of activities required to provide cost-effective support to a software system, performed during both **pre-delivery** and **post-delivery** stages.* (ISO/IEC 14764:2022)

- **Pre-delivery activities**: Planning for future post-delivery operations (e.g., designing for maintainability from the start).
- **Post-delivery activities**: Changing software, providing user training, operating a help desk.

---

### Maintainability
> *The capability of a software product to be modified to improve it, correct it, or adapt it to changes in environment and in requirements.* (ISO/IEC 25010)

| Sub-characteristic | Definition |
|--------------------|------------|
| **Modularity** | A change to one component has minimal impact on other components. |
| **Reusability** | The product or its parts can be used in more than one system. |
| **Analyzability** | The ability to assess the impact of a change, diagnose failures, and identify parts to modify. |
| **Modifiability** | The product can be changed without introducing defects or degrading quality. |
| **Testability** | Tests can be performed effectively and efficiently on the product. |

---

### Software Maintenance vs. Software Evolution

| Dimension | Software Maintenance | Software Evolution |
|-----------|---------------------|--------------------|
| **Primary Goal** | Fix bugs; preserve existing functionality | Create new or better designs to support new functionality |
| **Scope of Change** | Minimal; keep the system running | Broader; changes functionality/properties |
| **Design Change** | No change to core design | New but related designs from existing ones |
| **Trigger** | Failure or fault | New requirement, performance need, platform change |

> ⚠️ **Key nuance (Lientz & Swanson)**: Continued development that changes functionality *as experienced by the customer* is called **software evolution**, not maintenance in the strict sense.

---

### The Four Maintenance Types

| Type | Posture | Trigger | Goal |
|------|---------|---------|------|
| **Corrective** | Reactive | Failure discovered post-delivery | Repair defects |
| **Adaptive** | Reactive | Environment change (OS, DB, hardware) | Adapt system to new environment |
| **Perfective** | Proactive | No fault; improvement desired | Enhance UX, performance, or maintainability |
| **Preventive** | Proactive | Potential fault identified before failure | Prevent future failures |

---

### Software Maintenance Life Cycle (SMLC)
> *The structured process governing how maintenance activities are planned, executed, and closed throughout a software system's operational lifetime.*

#### Staged Model (Rajlich & Bennett, 2000)

| Stage | Description |
|-------|-------------|
| **Initial Development** | First version of the system built. |
| **Evolution** | First running version deployed; feedback received — quick patches, new features, environment adaptations. |
| **Servicing** | Loss of evolvability; only minor and emergency fixes, no major functionality changes. |
| **Phaseout** | Software still in use but no active servicing. |
| **Closedown** | Software withdrawn or migrated to a replacement system. |

#### Change Mini-Cycle Model (Yau et al., 1978)
> *The micro-level process applied to each individual change request.*

Steps:
1. **Change Request** — Problem report or enhancement request from users, customers, or management.
2. **Analyze and Plan Change**
   - *Program Comprehension*: Understand which parts of the system will be affected.
   - *Change Impact Analysis*: Identify consequences, estimate resources, perform cost-benefit analysis, decide whether to proceed.
3. **Implement Change**
   - *Restructuring (Refactoring)*: Restructure code to facilitate understanding and change.
   - *Change Propagation*: Ensure the change is consistently reflected throughout the entire system.
4. **Verify and Validate** — Code review, regression testing, new test execution.
5. **Documentation Change** — Update all documentation to remain consistent with the modified code.

> 🔁 If further changes are required after verification, the cycle loops back to *Implement Change*. If the change request is rejected, a new request may be selected.

---

### Maintenance Effort Distribution (Pressman & Maxim, 9th ed.)

```
Perfective  ████████████████████████████████ 50%
Adaptive    ████████████████ 25%
Corrective  █████████████ 21%
Preventive  ██ 4%
```

> 🔑 **Critical insight**: The majority of maintenance effort (~50%) is *perfective* — improvements and enhancements, not bug fixes. This contradicts the common assumption that maintenance is primarily about fixing bugs.

---

### Maintenance Team

- A **separate maintenance team** is often employed, distinct from the original development team.
- The maintenance team uses the **same tools, environments, and processes** as the development team.
- Stakeholders involved: users, customers, operators, analysts, designers, developers, testers, QA personnel, DBAs, network administrators.

---

### DevOps and Agile in Maintenance Context

| Practice | Description |
|----------|-------------|
| **Continuous Development** | Software delivered in multiple increments; maintenance is embedded in each cycle. |
| **Continuous Testing/Integration/Deployment** | Automated pipelines reduce the gap between development and maintenance. |
| **Continuous Monitoring** | Operations staff embedded in the dev team proactively monitor production systems for emerging problems. |

> DevOps collapses the traditional boundary between "development" and "maintenance" — maintenance becomes a continuous, integrated activity rather than a separate post-delivery phase.

---

## 2. Mechanisms & How Things Work

### How the Change Mini-Cycle Works (Causal Logic)

```
Change Request
     │
     ▼
[Rejected?] ──YES──► Loop back / Select new request
     │ NO
     ▼
Analyze & Plan
  ├── Program Comprehension (understand scope of impact)
  └── Change Impact Analysis (cost/benefit, resource estimation)
     │
     ▼
Implement Change
  ├── Restructuring (refactor for clarity before modification)
  └── Change Propagation (apply change across all affected modules)
     │
     ▼
Verify & Validate
  ├── Code Review
  ├── Regression Testing (ensure nothing else broke)
  └── New Tests (if new behavior introduced)
     │
     ▼
[Further changes needed?] ──YES──► Back to Implement Change
     │ NO
     ▼
Documentation Change ──► DONE (feeds back as new change request if needed)
```

**Why this matters**: The ripple effect of a change is why *Change Impact Analysis* is critical. Skipping it leads to cascading defects — fixing one thing breaks another.

---

### How the Staged Model Progresses

The transition between stages is driven by **evolvability** — the system's capacity to absorb major changes:

1. Development ends → system enters **Evolution** (still highly evolvable).
2. Accumulated changes degrade internal structure → **loss of evolvability** → system enters **Servicing**.
3. Business decides to stop servicing → **Phaseout**.
4. System formally decommissioned → **Closedown**.

> The transition from Evolution to Servicing is a critical engineering inflection point. It often results from **technical debt accumulation** — poorly documented changes, unstructured code, insufficient modularity.

---

## 3. Comparisons & Trade-offs

### Corrective vs. Preventive Maintenance

| Dimension | Corrective | Preventive |
|-----------|-----------|-----------|
| **Posture** | Reactive | Proactive |
| **Trigger** | Failure has already occurred | Fault detected before it causes failure |
| **Cost** | Higher (system may be down) | Lower long-term (avoids outages) |
| **Example** | Program aborts in production | Adding type-checking before null pointer crash |

### Perfective vs. Adaptive Maintenance

| Dimension | Perfective | Adaptive |
|-----------|-----------|---------|
| **Posture** | Proactive | Reactive |
| **Trigger** | Internal desire for improvement | External environment change |
| **Changes functionality?** | May add features | Usually preserves functionality, changes integration |
| **Example** | Refactoring for readability | Porting to new OS or database version |

### Separate Maintenance Team vs. Same Development Team

| Factor | Separate Team | Same Team (DevOps model) |
|--------|--------------|--------------------------|
| **Domain knowledge** | Lower initially | Higher |
| **Continuity** | Risk of knowledge loss | Better continuity |
| **Focus** | Dedicated to maintenance | Risk of context-switching overhead |
| **Modern practice** | Traditional waterfall | Preferred in Agile/DevOps |

---

## 4. Common Misconceptions & Exam Traps

> ⚠️ These are the most likely sources of wrong answer choices.

1. **"Maintenance is only a post-delivery activity."**
   → **FALSE.** ISO/IEC 14764 explicitly defines maintenance as including *pre-delivery* activities (planning, designing for maintainability).

2. **"Corrective maintenance is the most time-consuming type."**
   → **FALSE.** Perfective maintenance consumes ~50% of effort. Corrective is only ~21%.

3. **"Adaptive maintenance is proactive."**
   → **FALSE.** Adaptive is *reactive* — it responds to external environment changes. Perfective and Preventive are the *proactive* types.

4. **"Software evolution and software maintenance are the same thing."**
   → **NUANCED.** Maintenance can *include* evolution (Lientz & Swanson), but in Godfrey & German's definition, evolution specifically refers to *new designs supporting new functionality*, while maintenance preserves existing design.

5. **"Preventive maintenance fixes bugs."**
   → **INCORRECT.** Preventive maintenance addresses faults that *have not yet become failures*. It is about averting failure, not repairing it. Corrective maintenance fixes actual bugs.

6. **"The maintenance team uses different tools than the development team."**
   → **FALSE.** The maintenance team uses the *same* development environments, test environments, tools, and processes.

7. **"The Servicing stage allows major functionality changes."**
   → **FALSE.** Servicing only permits minor and emergency fixes. Major changes are the characteristic of the *Evolution* stage.

8. **"Refactoring is a type of corrective maintenance."**
   → **INCORRECT.** Refactoring (restructuring) is associated with *perfective* maintenance (improving code quality without changing functionality) and is also a sub-step in the Change Mini-Cycle's implementation phase.

---

## 5. High-Probability Exam Questions

---

**Q1.** According to ISO/IEC 14764, software maintenance is best defined as:

- A) The process of fixing bugs after software delivery
- B) The totality of activities required to provide cost-effective support, spanning both pre- and post-delivery stages ✅
- C) The process of rewriting software to add new features
- D) Activities performed exclusively after system deployment

> **Explanation**: A is too narrow (only bugs, only post-delivery). C describes development, not maintenance. D contradicts ISO/IEC 14764, which explicitly includes pre-delivery planning activities.

---

**Q2.** A team upgrades their application's database driver to support PostgreSQL 15 after the vendor dropped support for PostgreSQL 12. This is an example of:

- A) Corrective Maintenance
- B) Perfective Maintenance
- C) Adaptive Maintenance ✅
- D) Preventive Maintenance

> **Explanation**: The change is *reactive* to an *environmental change* (database version). No bug was fixed (not corrective). No internal quality improvement (not perfective). No pre-emptive fault prevention (not preventive).

---

**Q3.** Which maintenance type consumes the largest share of maintainer effort, according to Pressman & Maxim?

- A) Corrective (21%)
- B) Adaptive (25%)
- C) Perfective (50%) ✅
- D) Preventive (4%)

> **Explanation**: This is a direct recall question but frequently answered incorrectly because students assume "bug fixing" (corrective) dominates.

---

**Q4.** In the Change Mini-Cycle model, what is the primary purpose of *Change Impact Analysis*?

- A) To write new test cases for the changed module
- B) To identify consequences of a change, estimate resources, and decide whether to implement it ✅
- C) To refactor code before making the actual change
- D) To update documentation after the change is deployed

> **Explanation**: A is part of Verify & Validate. C describes Restructuring. D is the Documentation Change step. Impact Analysis is explicitly about evaluating the *feasibility and scope* of the change before committing.

---

**Q5.** A developer discovers that a rarely-executed code path contains a null pointer dereference that has never triggered in production. She adds a null-guard before it ships. This is:

- A) Corrective Maintenance
- B) Perfective Maintenance
- C) Adaptive Maintenance
- D) Preventive Maintenance ✅

> **Explanation**: The fault has *not yet become a failure*. Preventive maintenance is specifically defined as correcting potential faults before damage occurs. Corrective requires the failure to have already occurred.

---

**Q6.** According to the Staged Model (Rajlich & Bennett), what event marks the transition from the Evolution stage to the Servicing stage?

- A) The software reaches end-of-life and is withdrawn
- B) The software is deployed to end users for the first time
- C) The software loses its evolvability due to accumulated changes ✅
- D) The development team is disbanded

> **Explanation**: A describes Closedown. B is the transition from Initial Development to Evolution. D is not a defined transition trigger in the model. Loss of evolvability — typically from technical debt — drives the Evolution → Servicing transition.

---

**Q7.** Which of the following is a sub-characteristic of *Maintainability* per ISO/IEC 25010?

- A) Reliability
- B) Portability
- C) Analyzability ✅
- D) Usability

> **Explanation**: Reliability, Portability, and Usability are separate quality characteristics in ISO/IEC 25010. Analyzability, along with Modularity, Reusability, Modifiability, and Testability, are sub-characteristics of Maintainability.

---

**Q8.** How does the DevOps model fundamentally change the traditional view of software maintenance?

- A) It eliminates the need for corrective maintenance entirely
- B) It separates the maintenance team from the development team
- C) It embeds operations staff into the development team, making maintenance a continuous activity ✅
- D) It replaces preventive maintenance with automated testing

> **Explanation**: DevOps collapses the dev/ops boundary. B is the *opposite* of DevOps. A and D are false — DevOps does not eliminate categories of maintenance.

---

**Q9.** Which two maintenance types are classified as *proactive*?

- A) Corrective and Adaptive
- B) Adaptive and Perfective
- C) Perfective and Preventive ✅
- D) Corrective and Preventive

> **Explanation**: Corrective and Adaptive are *reactive* — they respond to problems or environmental changes that have already occurred. Perfective and Preventive are *proactive* — initiated without an immediate failure trigger.

---

**Q10.** In the Staged Model, what characterizes the *Phaseout* stage?

- A) The software is actively maintained with minor bug fixes
- B) The software is being migrated to a replacement system
- C) The software is in use but receives no servicing ✅
- D) The software is being refactored for a new platform

> **Explanation**: A describes Servicing. B and D describe Closedown. Phaseout is specifically the period where the software is *still operational* but *support has been discontinued*.

---

## 6. One-Page Cheat Sheet Summary

```
╔══════════════════════════════════════════════════════════════╗
║          SOFTWARE MAINTENANCE — EXAM CHEAT SHEET             ║
╠══════════════════════════════════════════════════════════════╣
║ DEFINITION (ISO/IEC 14764)                                   ║
║  • Cost-effective support across PRE + POST delivery          ║
║  • Pre-delivery = planning; Post-delivery = fixing/training   ║
╠══════════════════════════════════════════════════════════════╣
║ 4 TYPES — Key: Reactive vs. Proactive                        ║
║  REACTIVE:   Corrective (fix failure) | Adaptive (env change) ║
║  PROACTIVE:  Perfective (improve)     | Preventive (pre-empt) ║
╠══════════════════════════════════════════════════════════════╣
║ EFFORT DISTRIBUTION (Pressman)                               ║
║  Perfective 50% > Adaptive 25% > Corrective 21% > Prev 4%    ║
╠══════════════════════════════════════════════════════════════╣
║ MAINTAINABILITY SUB-CHARACTERISTICS (ISO/IEC 25010)          ║
║  Modularity | Reusability | Analyzability                     ║
║  Modifiability | Testability                                  ║
╠══════════════════════════════════════════════════════════════╣
║ STAGED MODEL (Rajlich & Bennett)                             ║
║  Init Dev → Evolution → Servicing → Phaseout → Closedown     ║
║  Evolution: evolvable, all change types                       ║
║  Servicing: loss of evolvability, minor/emergency only        ║
║  Phaseout: in use, NO servicing                              ║
║  Closedown: withdrawn/migrated                               ║
╠══════════════════════════════════════════════════════════════╣
║ CHANGE MINI-CYCLE (Yau et al.)                               ║
║  1. Change Request                                           ║
║  2. Analyze & Plan (Comprehension + Impact Analysis)         ║
║  3. Implement (Restructuring + Propagation)                  ║
║  4. Verify & Validate (Review + Regression + New Tests)      ║
║  5. Documentation Change                                     ║
╠══════════════════════════════════════════════════════════════╣
║ MAINTENANCE TEAM                                             ║
║  • Often SEPARATE from dev team                              ║
║  • Uses SAME tools/environments as dev team                  ║
║  • DevOps: blurs the boundary (continuous maintenance)        ║
╠══════════════════════════════════════════════════════════════╣
║ MAINTENANCE vs. EVOLUTION                                    ║
║  Maintenance = preserve design, fix/adapt                    ║
║  Evolution = new design, new functionality                   ║
╠══════════════════════════════════════════════════════════════╣
║ WHY MAINTENANCE IS HARD                                      ║
║  • No traceability | Unstructured code                       ║
║  • Insufficient domain knowledge | Poor documentation        ║
║  • Ripple effect | Lack of change stability                  ║
║  • Myopic view (treated as post-delivery only)               ║
╠══════════════════════════════════════════════════════════════╣
║ CRITICAL TRAPS                                               ║
║  ✗ "Maintenance is only post-delivery" → WRONG               ║
║  ✗ "Corrective = most effort" → WRONG (Perfective = 50%)     ║
║  ✗ "Adaptive is proactive" → WRONG (it is REACTIVE)          ║
║  ✗ "Preventive fixes bugs" → WRONG (fault not yet a failure)  ║
║  ✗ "Servicing allows major changes" → WRONG                  ║
╚══════════════════════════════════════════════════════════════╝
```

---

*Study Guide generated from: ISO/IEC 14764:2022 | ISO/IEC 25010 | Pressman & Maxim (9th ed.) | Rajlich & Bennett (2000) | Yau et al. (1978) | Godfrey & German (2008)*
