# 📚 Software Project Management — Exam Study Guide

> **Exam in 3 days** | Source: Lecture Slides (Senivongse) | Format: Obsidian-compatible Markdown

---

## 1. Core Concepts

> Every key term defined precisely as it would appear in an exam context.

| Term | Definition |
|---|---|
| **Project** | A *temporary* endeavor undertaken to create a *unique* product, service, or result. Contrast with an Operation, which is ongoing and sustains the business. |
| **Operation** | Ongoing, repeatable work done to sustain a business (e.g., customer support, system monitoring). NOT a project. |
| **Project Management** | The application of knowledge, skills, tools, and techniques to project activities in order to meet project requirements. |
| **Deliverable** | A specific, tangible product or service produced and delivered as part of a project. |
| **Project Manager** | The person assigned to lead the project on a day-to-day basis by the performing organization. |
| **Project Sponsor** | A senior, business-oriented person who makes high-level decisions and provides resources for the project. |
| **Project Stakeholders** | Any individuals, groups, or organizations that may affect, be affected by, or perceive themselves to be affected by a project. Includes the sponsor, team, customers, suppliers, and even opponents. |
| **Project Quadruple Constraints** | The four competing constraints of every project: **Scope, Time, Cost, and Quality**. Changing one automatically affects the others. |
| **Work Breakdown Structure (WBS)** | A hierarchical decomposition of the total scope of work. Can be organized by product (problem decomposition) or by phase (process decomposition). Items are named using **nouns**. |
| **System Request** | The document produced during Project Initiation that captures the project sponsor, business need, business requirements, business value, and constraints. |
| **Feasibility Analysis** | An evaluation of whether a proposed project is worth pursuing, covering Technical, Economic, and Organizational dimensions. |
| **Person-Month (PM)** | The unit of effort in COCOMO II; the amount of work one person performs in one month (= 152 person-hours = 19 person-days). |
| **Function Point (FP)** | A language-independent unit of measure for the *functional size* of software, used for early estimation before coding begins. Standardized as ISO 20926:2009. |
| **COCOMO II** | Constructive Cost Estimation Model (v2.2000). A parametric model that predicts software effort (PM) and schedule from size (KLOC), Scale Factors, and Effort Multipliers. |
| **Risk** | An uncertainty that can have a negative or positive effect on meeting project objectives. |
| **Risk Register** | A document used to record identified risks, their analysis (probability + impact), planned responses, and current status. |
| **Gantt Chart** | A bar chart that maps WBS activities against a timeline; used to track schedule for the whole project. |
| **Burndown Chart** | An Agile chart tracking remaining story points vs. time within a *specific sprint/iteration*. |
| **NPV (Net Present Value)** | Sum of all discounted cash flows (benefits minus costs) over the project's lifetime. A positive NPV indicates the project adds value. |
| **ROI (Return on Investment)** | `(NPV / Discounted Total Costs) × 100%`. Measures profitability efficiency. General criterion: ROI > 10%. |
| **Break-Even Point (BEP)** | The point in time at which cumulative discounted benefits equal cumulative discounted costs. Calculated at the first year with a positive discounted cash flow. |
| **Velocity (Agile)** | The number of story points a team completes per sprint. Used to estimate total sprints and project duration. |
| **Scale Factors (SF)** | Five project-level parameters in COCOMO II (PREC, FLEX, RESL, TEAM, PMAT) that influence the exponent E in the effort equation, affecting economies/diseconomies of scale. |
| **Effort Multipliers (EM)** | Seventeen cost drivers in COCOMO II (product, platform, personnel, project factors) that multiply the base effort estimate. Nominal rating = **1.00**. |

---

## 2. Mechanisms & How Things Work

### 2.1 Project Management Process Groups (PMBOK)

```
Initiating → Planning → Executing → Monitoring & Controlling → Closing
```

- **Initiating**: Define the project, identify the problem, get approval (System Request + Feasibility).
- **Planning**: Scope (WBS), estimation, schedule (Gantt), team staffing, risk register.
- **Executing**: Development work, sprints, daily scrums.
- **Monitoring & Controlling**: Burndown charts, cost tracking, risk tracking, change management.
- **Closing**: Sprint retrospective, project completion review.

---

### 2.2 Project Initiation Process

```
Step 1: Project Sponsor identifies a business need → writes System Request
Step 2: Approval committee reviews System Request
Step 3: Feasibility Analysis is conducted (Technical + Economic + Organizational)
Step 4: Project is approved or rejected based on feasibility
```

---

### 2.3 Feasibility Analysis — Three Dimensions

#### Technical Feasibility ("Can we build it?")
Factors assessed:
1. Familiarity with the functional area
2. Familiarity with the technology stack
3. Project size and complexity
4. Compatibility with existing systems

#### Economic Feasibility ("Should we build it?")
Uses **Cost-Benefit Analysis** over 3–5 years.

**Key formulas:**

$$\text{Discount Factor} = \frac{1}{(1+r)^t}$$

$$\text{PV} = \text{Amount} \times \text{Discount Factor}$$

$$\text{NPV} = \sum(\text{Discounted Benefits} - \text{Discounted Costs})$$

$$\text{ROI} = \frac{\text{NPV}}{\sum \text{Discounted Total Costs}} \times 100$$

$$\text{BEP} = \frac{|\text{Cumulative Discounted Cash Flow (prev. year)}|}{\text{Discounted Cash Flow (BEP year)}} + \text{Previous Year}$$

**General decision criteria:**
- NPV > 0
- ROI > 10%
- BEP within the projected period (e.g., within 5 years)

#### Organizational Feasibility ("Will they use it?")
- Alignment with business goals
- Impact on stakeholders (work, income, health)
- Public impacts: security, privacy, environment, economy

---

### 2.4 Function Point Analysis (FPA) — Step by Step

**Step 1: Identify the 5 basic function types**

| Category | Type | Direction | Involves Derived Data? | Updates ILF? |
|---|---|---|---|---|
| Transaction | **EI** (External Input) | Outside → Inside | No | Yes (CUD) |
| Transaction | **EO** (External Output) | Inside → Outside | **Yes** (derived/calculated) | No |
| Transaction | **EQ** (External Inquiry) | Retrieve only | **No** (raw retrieval) | No |
| Data | **ILF** (Internal Logical File) | Inside boundary | — | Maintained by EI |
| Data | **EIF** (External Interface File) | Outside boundary | — | Maintained by *another* app |

**Step 2: Rate the complexity of each function**
- **Transactions (EI, EO, EQ)**: Count **FTR** (files referenced) and **DET** (data elements) → look up rating table (Low/Average/High) → get unadjusted FP value (e.g., Low EI = 3 FP)
- **Data (ILF, EIF)**: Count **RET** (record subgroups) and **DET** → look up rating table → get FP value (e.g., Low ILF = 7 FP)

**Step 3: Sum all FP values → Total Unadjusted Function Points (UFP)**

**Step 4: Convert to KLOC (for COCOMO II input)**
$$\text{KLOC} = \frac{\text{UFP} \times \text{Gearing Factor}_\text{language}}{1000}$$

Example gearing factors: Java = 53, C = 97, SQL = 21, Python (approx) = 50

---

### 2.5 COCOMO II Post-Architecture Model

$$PM = 2.94 \times \text{Size}^E \times \prod_{i=1}^{17} EM_i$$

$$E = 0.91 + 0.01 \times \sum_{j=1}^{5} SF_j$$

**Interpretation:**
- When all SFs = 0 (Extra High rating), E = 0.91 → slight **economies of scale** (larger projects are proportionally cheaper per unit).
- When all SFs are Very Low/Nominal, E > 1.0 → **diseconomies of scale** (larger projects cost disproportionately more per unit).
- All 17 EMs have a **nominal value of 1.00** — ratings above nominal increase effort, below nominal decrease effort.

**5 Scale Factors:**
1. **PREC** — Precedentedness (how familiar is the team with this type of project?)
2. **FLEX** — Development flexibility (how rigidly are requirements fixed?)
3. **RESL** — Architecture/Risk resolution (how well-defined is the architecture?)
4. **TEAM** — Team cohesion (how well do stakeholders cooperate?)
5. **PMAT** — Process maturity (CMM level of the organization)

**Development Labor Cost:**
$$\text{Cost} = PM \times \text{Average Labor Rate per Person-Month}$$

---

### 2.6 Project Estimation Approaches Summary

```
Problem Decomposition → Analogous Estimation → LOC-based or Story-Point-based
Problem Decomposition → Parametric Estimation → FPA → KLOC → COCOMO II
Process Decomposition → Analogous Estimation → Effort (PM) per activity
```

---

### 2.7 Team Size Calculation

$$\text{Number of People} = \frac{\text{Person-Months}}{\text{Time to Complete (months)}}$$

$$\text{Communication Channels} = \frac{N(N-1)}{2}$$

**Key insight**: Adding people increases communication overhead exponentially. This is why Scrum teams are capped at ~10 people.

---

### 2.8 Risk Management Process

```
1. Risk Identification → document potential negative/positive events
2. Risk Analysis → assess Probability × Impact → assign priority/rank
3. Risk Response → choose strategy: Avoid, Accept, Transfer, Mitigate, or Escalate
4. Risk Monitoring → track status throughout project
```

All documented in the **Risk Register**.

---

## 3. Comparisons & Trade-offs

### 3.1 Project vs. Operation

| Dimension | Project | Operation |
|---|---|---|
| Duration | Temporary (defined start/end) | Ongoing |
| Output | Unique product/service | Repeated/standardized output |
| Example | Developing a new app | Running customer support post-launch |

---

### 3.2 WBS: Problem Decomposition vs. Process Decomposition

| Dimension | Problem (Product) Decomposition | Process (Phase) Decomposition |
|---|---|---|
| Organized by | System components/features | Development phases (initiation, planning, sprints, closing) |
| Used in Agile | Product Backlog (Epics → User Stories) | Scrum WBS (Initiating → Sprint 1 → Sprint 2 → Closing) |
| Naming convention | Nouns (e.g., "Registration System") | Nouns/verbs (phase names + activity names) |

---

### 3.3 Estimation Methods Comparison

| Method | Type | Input | Accuracy | Best Used When |
|---|---|---|---|---|
| Analogous (LOC-based) | Analogous | Expert judgment + historical LOC | Low-Medium | Early; historical data available |
| Analogous (PM-based) | Analogous | Historical PM per function | Low-Medium | Early planning |
| Story Point-based | Analogous | Team velocity | Medium | Agile projects; after backlog is estimated |
| FPA | Parametric | Requirements (EI/EO/EQ/ILF/EIF counts) | Medium-High | Before coding; language-independent |
| COCOMO II | Parametric | KLOC + SFs + EMs | High | After architecture is defined |

---

### 3.4 Gantt Chart vs. Burndown Chart

| Dimension | Gantt Chart | Burndown Chart |
|---|---|---|
| Scope | Entire project | Single sprint/iteration |
| X-axis | Calendar time | Days in sprint |
| Y-axis | Task bars (% complete) | Remaining story points |
| Primary use | Schedule planning & whole-project tracking | Sprint progress monitoring |
| Methodology fit | Traditional + Agile | Agile (Scrum) |

---

### 3.5 Organizational Structures

| Structure | Team Reports To | Project Manager Authority | Best For |
|---|---|---|---|
| Functional | Functional unit head | Low | Stable, specialized work |
| Project (Projectized) | Program Manager | High | Dedicated project work |
| Matrix | Both functional + project managers | Medium (shared) | Organizations running many projects simultaneously |

---

### 3.6 Risk Response Strategies

| Strategy | Description | When to Use |
|---|---|---|
| **Avoidance** | Eliminate the risk cause | Risk is high-impact and preventable |
| **Acceptance** | Deal with it if it happens | Risk is low-impact or unavoidable |
| **Transference** | Shift to a third party (insurance, contract) | Risk can be contractually assigned |
| **Mitigation** | Reduce probability/impact | Risk is significant but partially controllable |
| **Escalation** | Notify higher authority | Risk is outside project manager's authority |

---

## 4. Common Misconceptions & Traps

> ⚠️ These are high-probability wrong-answer traps in MCQs.

- **Trap 1 — EO vs. EQ confusion**: An **EO** produces *derived/calculated* data (e.g., a sales report with totals). An **EQ** retrieves *raw* data with no calculation and does NOT update any ILF. "Display of inquiry data" = EQ, NOT EO.

- **Trap 2 — EIF is NOT maintained by your application**: An EIF is data that lives *outside* your system boundary and is maintained by *another* application's EI. Your system only reads it.

- **Trap 3 — Cancel buttons are NOT counted as DETs**: In FPA, action keys (buttons) that do NOT change the state of the application (like "Cancel") are excluded from DET counts.

- **Trap 4 — PM covers the whole project, not just coding**: COCOMO II's PM estimate includes analysis, design, coding, AND testing — not coding alone.

- **Trap 5 — Adding people doesn't linearly reduce time**: Due to exponential communication overhead `N(N-1)/2`, doubling the team does NOT halve the schedule (Brooks's Law territory).

- **Trap 6 — NPV > 0 is necessary but not sufficient**: You also need ROI > 10% AND BEP within the projected period. Don't select "approved because NPV > 0" alone.

- **Trap 7 — Nominal EM rating = 1.00, not zero**: All 17 effort multipliers at nominal = 1.00, meaning they have NO effect on the estimate. Extra High SF rating = 0.00 (maximum benefit).

- **Trap 8 — WBS items use NOUNS; activities use VERBS**: WBS items (scope) are named with nouns ("Registration System"). Activities (schedule) are named with verbs ("Develop Registration System").

- **Trap 9 — Operations are NOT projects**: Customer support, system monitoring after launch, ongoing manufacturing = Operations. Building the system = Project.

- **Trap 10 — Story points may not be available early**: Story point estimation requires a defined product backlog, which may not exist during early project planning. This limits its use for initial project-level estimates.

- **Trap 11 — EIF counts as both EIF (for you) and ILF (for the other system)**: When your system reads external data maintained by another application, that data is an EIF for your project and an ILF for the other project.

---

## 5. High-Probability Exam Questions

---

**Q1.** A team is assigned to maintain and monitor a banking system's servers after the system has launched. This activity is best classified as:

- A) A project, because it involves a defined team with specific responsibilities
- B) An operation, because it is ongoing and sustains the business
- C) A project, because it creates a unique result each time an issue is resolved
- D) An operation, because no deliverable is produced

> ✅ **Answer: B**
> *A is wrong* — having a defined team doesn't make something a project. *C is wrong* — uniqueness of individual tasks doesn't override the ongoing nature. *D is wrong* — operations can produce deliverables (e.g., monitoring reports).

---

**Q2.** A user inquiry screen allows users to search for product information. The output displays the product name, price, and category directly from the database with no calculations. Which FPA function type does this represent?

- A) External Input (EI)
- B) External Output (EO)
- C) External Inquiry (EQ)
- D) Internal Logical File (ILF)

> ✅ **Answer: C**
> *A is wrong* — EI is data going INTO the system to update an ILF. *B is wrong* — EO involves derived/calculated output. *D is wrong* — ILF is a data store, not a transaction.

---

**Q3.** In COCOMO II, what is the effect on estimated effort (PM) if all 5 Scale Factors are rated at their Extra High level?

- A) PM increases significantly because all SFs are maximized
- B) PM decreases because E = 0.91, indicating economies of scale
- C) PM is unchanged because SFs cancel each other out
- D) PM cannot be calculated without knowing the Effort Multipliers

> ✅ **Answer: B**
> Extra High SF = 0.00 for each factor. Sum of SFs = 0. E = 0.91 + 0.01×0 = 0.91 < 1.0 → economies of scale → effort grows *slower* than size. *A is wrong* — Extra High rating is the BEST (lowest SF value). *C and D are wrong* — SFs directly affect E in the formula.

---

**Q4.** Which of the following correctly distinguishes an EIF from an ILF in Function Point Analysis?

- A) An EIF is maintained by the application being measured; an ILF is maintained by an external system
- B) An ILF is maintained by the application being measured; an EIF is maintained by another application
- C) An EIF stores derived data; an ILF stores raw transactional data
- D) An ILF and EIF are interchangeable terms for the same concept

> ✅ **Answer: B**
> *A is wrong* — this reverses the definitions. *C is wrong* — neither definition involves derived vs. raw data. *D is wrong* — they are distinct concepts.

---

**Q5.** A project has an estimated NPV of $500,000, discounted total costs of $2,000,000, and the break-even point occurs at year 4 of a 5-year projection. What is the ROI, and is the project economically feasible?

- A) ROI = 25%; Feasible — all three criteria are met
- B) ROI = 25%; Not feasible — ROI must exceed 25%
- C) ROI = 20%; Feasible — NPV is positive
- D) ROI = 20%; Not feasible — BEP at year 4 is too late

> ✅ **Answer: A**
> ROI = (500,000 / 2,000,000) × 100 = **25%**. Criteria: NPV > 0 ✓, ROI > 10% ✓, BEP within 5 years ✓. *B is wrong* — 10% is the threshold, not 25%. *C and D* use incorrect ROI formula.

---

**Q6.** According to the lecture, what is the primary reason Agile Scrum teams are capped at approximately 10 people?

- A) Agile methodology requires all team members to have equal skill sets
- B) The number of communication channels grows as N(N-1)/2, making larger teams exponentially harder to coordinate
- C) Scrum sprints can only accommodate a maximum of 10 user stories
- D) Larger teams violate the Agile principle of self-organization

> ✅ **Answer: B**
> *A is wrong* — Scrum teams are cross-functional but not necessarily equal-skilled. *C is wrong* — sprint capacity is determined by velocity and story points, not headcount. *D is wrong* — self-organization is a principle but is not the primary reason for the size cap.

---

**Q7.** During WBS creation, a project manager lists "Login System" as a WBS item and "Implement Login System" as an activity. Which statement about this is correct?

- A) Both are incorrect — WBS items and activities must use the same naming convention
- B) "Login System" is correctly named (noun); "Implement Login System" is correctly named (verb) for an activity
- C) "Login System" should use a verb; "Implement Login System" should use a noun
- D) WBS items represent activities in the schedule, so both should use verbs

> ✅ **Answer: B**
> WBS items = **nouns** (deliverables/scope). Activities = **verbs** (work to be done). *A, C, D* all violate this convention.

---

**Q8.** A project includes a "Generate Monthly Sales Report" function that calculates total revenue, average order value, and growth rate from the internal sales database. Which function type does this represent?

- A) External Inquiry (EQ)
- B) External Input (EI)
- C) External Output (EO)
- D) Internal Logical File (ILF)

> ✅ **Answer: C**
> The output contains **derived/calculated** data (revenue totals, averages, growth rates). This is the defining characteristic of an EO. *A is wrong* — EQ has NO derived data. *B is wrong* — EI brings data IN to update a file. *D is wrong* — ILF is a data store.

---

**Q9.** Which risk response strategy is illustrated when a project manager includes a clause in a supplier's contract requiring server replacement within 48 hours at a negotiated cost?

- A) Risk Avoidance
- B) Risk Acceptance
- C) Risk Mitigation
- D) Risk Transference (partial)

> ✅ **Answer: D**
> Contractually shifting the financial consequence and replacement responsibility to the supplier = transference. It is *partial* because the project still bears some impact (downtime). *A is wrong* — avoidance eliminates the risk cause entirely. *B is wrong* — acceptance means dealing with it yourself. *C is wrong* — mitigation reduces probability, not shifts liability.

---

**Q10.** An agile team has a product backlog of 200 story points. Their velocity is 50 story points per sprint, and each sprint is 2 weeks long. How many months will the project take?

- A) 1 month
- B) 2 months
- C) 4 months
- D) 8 months

> ✅ **Answer: B**
> Sprints needed = 200 / 50 = **4 sprints**. Duration = 4 sprints × 2 weeks = 8 weeks = **2 months**. *A is wrong* — 1 month = 4 weeks = 2 sprints. *C and D* miscalculate the sprint count or duration.

---

## 6. One-Page Cheat Sheet Summary

> 🗂 Print this or pin it in Obsidian as your last-day review.

### 📌 Key Definitions
- **Project** = temporary + unique. **Operation** = ongoing + repeating.
- **Deliverable** = tangible output of a project.
- **Quadruple Constraints** = Scope, Time, Cost, Quality (all interdependent).
- **WBS** = hierarchical scope list. Items = **nouns**. Activities = **verbs**.
- **System Request** = initiation document (need, requirements, value, constraints).

### 📌 Feasibility Analysis
- **Technical** = Can we build it? (familiarity, size, compatibility)
- **Economic** = Should we build it? (NPV > 0, ROI > 10%, BEP within period)
- **Organizational** = Will they use it? (goals alignment, public impact)

### 📌 Economic Metrics
- `PV = Amount × 1/(1+r)^t`
- `NPV = Σ(Discounted Benefits − Discounted Costs)`
- `ROI = NPV / Σ Discounted Costs × 100`
- `BEP = |Cum. CF prev. year| / CF BEP year + prev. year`

### 📌 FPA — 5 Function Types
| Type | Direction | Derived? | Updates ILF? |
|---|---|---|---|
| EI | In → System | No | Yes |
| EO | System → Out | **Yes** | No |
| EQ | Retrieve only | **No** | No |
| ILF | Inside boundary | — | Via EI |
| EIF | Outside boundary | — | By other app |

- **Transactions** rated by FTR + DET
- **Data** rated by RET + DET
- Cancel buttons = NOT counted as DET

### 📌 COCOMO II
- `PM = 2.94 × Size^E × ΠEM`
- `E = 0.91 + 0.01 × ΣSF`
- 5 SFs: PREC, FLEX, RESL, TEAM, PMAT
- Extra High SF = **0.00** (best); Nominal EM = **1.00**
- 17 EMs across Product, Platform, Personnel, Project
- `Cost = PM × Labor Rate/Month`

### 📌 Agile Estimation
- `Sprints = Total Story Points / Velocity`
- `Duration = Sprints × Sprint Length`
- `Cost = Team Size × Rate × Duration`
- Story points unavailable at early project planning stage

### 📌 Team & Communication
- `Channels = N(N-1)/2` → exponential growth
- Scrum: ≤ 10 people, self-organizing, cross-functional
- Traditional: hierarchical with functional/technical leads

### 📌 Risk Management
1. **Identify** → Risk Register entry
2. **Analyze** → Probability × Impact matrix (Grade A–N)
3. **Respond** → Avoid / Accept / Transfer / Mitigate / Escalate
4. **Monitor** → Track throughout project

### 📌 Scheduling Tools
- **Gantt Chart** = Whole project, timeline bars, tasks with dependencies
- **Burndown Chart** = Per sprint, remaining story points over time

### 📌 Key Exam Traps
1. EO = derived data; EQ = raw retrieval (no calculation)
2. EIF is maintained by *another* app, not yours
3. Cancel buttons NOT counted in FPA
4. PM = whole project effort, not just coding
5. NPV > 0 alone ≠ economically feasible (need ROI + BEP too)
6. Extra High SF rating = 0.00 (lowest SF = lowest diseconomy)
7. Nominal EM = 1.00 (not zero, not blank)

---

*Study guide prepared for exam use. All content derived from: Senivongse, T. — Software Project Management lecture slides; PMBOK 7th Ed.; COCOMO II Manual v2.1; Dennis et al., Systems Analysis & Design 6th Ed.; Pressman & Maxim, Software Engineering 9th Ed.; Schwalbe, IT Project Management 9th Ed.*
