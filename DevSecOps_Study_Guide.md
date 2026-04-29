# DevOps, CI/CD & DevSecOps — Exam Study Guide
**Course:** 2110423 Software Engineering
**Target:** Multiple-Choice Exam in 3 Days

---

## Table of Contents
- [[#1. Core Concepts]]
- [[#2. Mechanisms & How Things Work]]
- [[#3. Comparisons & Trade-offs]]
- [[#4. Common Misconceptions & Traps]]
- [[#5. High-Probability Exam Questions]]
- [[#6. One-Page Cheat Sheet Summary]]

---

## 1. Core Concepts

### DevOps
> A combined approach integrating software **Dev**elopment and IT **Op**eration**s**, unifying people, processes, and technologies to continuously deliver value to customers. DevOps is a **culture and set of practices**, not a specific technology or framework.

**CSIRO/SEI Definition:** A set of practices that reduce the turnaround time from committing a change to a system and implementing it into production, while maintaining high quality.

---

### CALMS Framework
> The five pillars used to assess and guide a DevOps adoption. Stands for **C**ulture, **A**utomation, **L**ean, **M**easurement, **S**haring.

| Pillar | Core Idea |
|--------|-----------|
| **Culture** | Cross-functional collaboration; breaking down silos between Dev and Ops |
| **Automation** | Automated tests, continuous delivery, automated deploys, configuration as code |
| **Lean** | Continuous improvement; being agile; eliminating low-value activities; embracing failure |
| **Measurement** | Tracking key metrics (lead time, failure rate, MTTR, active users, feedback) |
| **Sharing** | Sharing knowledge, responsibilities, feedback, tools, and pipeline standards across teams |

---

### DORA Metrics
> Four key engineering metrics defined by **DevOps Research and Assessment** to measure the performance of a software delivery pipeline.

| Metric | Definition |
|--------|------------|
| **Lead Time for Changes** | Duration from code commit to deployable state |
| **Change Failure Rate** | Percentage of code changes requiring hotfixes or remediation after production |
| **Deployment Frequency** | How often new code is deployed to production |
| **Mean Time to Recovery (MTTR)** | How long to recover from a partial or total service failure |

---

### Continuous Integration (CI)
> A practice where developers **frequently merge code into a shared repository**; each commit triggers an **automated build and test** process to provide fast feedback on code quality.

**CI Pipeline Steps:** Code Commit → Automated Build → Automated Testing → Integration Testing

---

### Continuous Delivery (CD)
> An extension of CI where code that passes all tests is **ready to be deployed to production** at any time, but deployment is triggered **manually** (human approval gate exists).

---

### Continuous Deployment (CD)
> Every change that passes the automated pipeline is **automatically deployed to production** with no manual intervention. Includes automated rollback and production monitoring.

---

### DevSecOps
> An evolution of DevOps that integrates security practices **throughout the entire SDLC** from the start ("Shift Left"), rather than treating security as a final phase. Security is treated as **everyone's responsibility**.

---

### Shift Left
> The principle of moving security activities **earlier** (to the left) in the development pipeline — catching vulnerabilities at code commit time rather than post-deployment. Earlier detection = cheaper and faster remediation.

---

### SAST (Static Application Security Testing)
> Automated analysis of **source code** to identify security vulnerabilities **without executing the program**. Works by parsing code into an Abstract Syntax Tree (AST), applying security rules, and tracking data flow.

**Key Tools:** SonarQube, Semgrep, Checkmarx, CodeQL

---

### DAST (Dynamic Application Security Testing)
> Security testing performed **while the application is running**, simulating external attacks from an attacker's perspective to find runtime vulnerabilities.

**Key Tools:** OWASP ZAP, Burp Suite, Acunetix, nuclei

---

### SCA (Software Composition Analysis)
> Automated analysis of **third-party/open-source dependencies** to identify known security vulnerabilities, license compliance issues, and outdated components.

**Key Tools:** Snyk, OWASP Dependency Check

---

### RASP (Runtime Application Self-Protection)
> Security technology **embedded within the application itself** that monitors internal behavior in real-time during production, detects anomalies/attacks, and blocks or terminates malicious requests.

---

### WAF (Web Application Firewall)
> A security solution sitting between the client and backend (as a **reverse proxy**) that monitors, filters, and blocks malicious HTTP/HTTPS traffic. Helps prevent SQL injection and XSS.

**Key Products:** AWS WAF, Cloudflare WAF, Azure WAF, Google Cloud Armor

---

### SIEM (Security Information and Event Management)
> A set of tools for detecting and reporting threats, auditing security, and performing digital forensics by collecting, aggregating, correlating, and alerting on security event data.

---

### Infrastructure as Code (IaC)
> Defining servers, networks, databases, and cloud resources in **code files** (e.g., Terraform, Kubernetes YAML) instead of manual configuration. Makes infrastructure repeatable, reviewable, consistent, and rapidly recreatable.

**Key Tools:** Terraform, Pulumi, Ansible, Puppet, Chef

---

### GitOps
> A strategy where **application and infrastructure configuration live in Git repositories**; changes happen via pull requests; automated systems detect Git changes and apply them to environments. The live system is continuously reconciled to match the Git state.

---

### GitHub Actions
> A CI/CD platform integrated into GitHub that automates workflows triggered by repository events. Core components: **Events, Jobs, Steps, Actions, Runners**.

---

## 2. Mechanisms & How Things Work

### How the CI/CD Pipeline Works

```
CODE COMMIT
    ↓
AUTOMATED BUILD        ← CI begins here
    ↓
AUTOMATED TESTING (Unit + Integration)
    ↓
RELEASE ARTIFACT
    ↓
[Manual Approval Gate]  ← Continuous DELIVERY stops here
    ↓
AUTOMATED DEPLOY TO PRODUCTION  ← Continuous DEPLOYMENT automates this
    ↓
OPERATE & MONITOR
```

---

### How SAST Works (Step-by-Step)

1. **Parse** source code into an **Abstract Syntax Tree (AST)** — a structured representation of code syntax
2. **Apply security rules and patterns** against the AST (e.g., detect SQL concatenation, weak crypto)
3. **Track data flow** through the application to find tainted data paths
4. **Identify potential vulnerabilities** (e.g., SQL injection, hardcoded secrets)
5. **Report findings** with file location, line number, severity, CWE/OWASP classification, and recommended fix

> **Example:** Semgrep detecting `cursor.execute(f"SELECT * FROM users WHERE id={user_id}")` → flags CWE-89 SQL Injection, recommends parameterized queries.

---

### How DAST Works (Step-by-Step)

1. **Discovery & Crawling** — maps the application's attack surface (URLs, forms, APIs)
2. **Analysis & Profiling** — understands application behavior and parameters
3. **Attack Simulation** — sends malicious payloads (SQLi, XSS, etc.) against the running app
4. **Response Analysis** — examines HTTP responses for indicators of vulnerability
5. **Verification & Confirmation** — confirms findings are true positives
6. **Reporting** — produces vulnerability report with severity and remediation guidance

---

### How RASP Works (Step-by-Step)

1. **Embedded** inside the application runtime (not external)
2. **Monitors** internal application behavior: data flow, method calls, application state
3. **Detects** anomalies or attack patterns in real-time
4. **Prevents/Responds** by blocking requests, terminating sessions, or shutting down affected components
5. **Feeds back** alerts to application owners/security teams

---

### How GitHub Actions Executes a Workflow

```
EVENT triggered (push, pull_request, schedule, etc.)
    ↓
WORKFLOW file loaded from .github/workflows/*.yml
    ↓
RUNNER assigned (ubuntu-latest, windows-latest, macos)
    ↓
JOB 1 executes on Runner 1:
    Step 1: Run Action (e.g., actions/checkout@v4)
    Step 2: Run Shell Script
    Step 3: Run Shell Script
    Step 4: Run Action (e.g., semgrep ci)
    ↓
JOB 2 executes on Runner 2 (parallel or sequential):
    Steps...
```

> Each **Runner** executes exactly **one Job** at a time. Multiple runners enable parallel job execution.

---

### How SIEM Works (Step-by-Step)

1. **Data Collection** — gathers logs from apps, servers, network devices, security tools
2. **Aggregation & Normalization** — unifies data from heterogeneous sources into a common format
3. **Correlation & Analytics** — applies rules to detect patterns indicating threats
4. **Alerting & Response** — generates alerts, triggers playbooks, notifies security teams
5. **Retention & Reporting** — stores events for compliance and forensic investigation

---

## 3. Comparisons & Trade-offs

### Continuous Delivery vs. Continuous Deployment

| Dimension | Continuous Delivery | Continuous Deployment |
|-----------|--------------------|-----------------------|
| **Deployment Trigger** | Manual approval gate required | Fully automated — no human intervention |
| **Risk Level** | Lower — human reviews before production | Higher — requires extremely robust automated testing |
| **Speed to Production** | Slightly slower (approval step) | Fastest possible |
| **When to Use** | Regulated industries, critical systems | Mature pipelines with high test coverage |
| **Pipeline Gate** | ✅ Toggle/Manual gate exists | ❌ No manual gate |

---

### SAST vs. DAST

| Dimension | SAST | DAST |
|-----------|------|------|
| **When Run** | On source code (pre-execution) | On running application |
| **What It Finds** | Code-level vulnerabilities (injection, weak crypto) | Runtime vulnerabilities (auth bypass, config errors) |
| **False Positives** | Higher (no runtime context) | Lower (confirmed at runtime) |
| **Requires Running App?** | ❌ No | ✅ Yes |
| **Pipeline Stage** | Early CI (Gate #1) | Late CD, pre-deploy (Gate #5) |
| **Examples** | SonarQube, Semgrep, CodeQL | OWASP ZAP, Burp Suite |

---

### RASP vs. WAF

| Dimension | RASP | WAF |
|-----------|------|-----|
| **Location** | Inside the application | External (reverse proxy, network edge) |
| **Visibility** | Internal app behavior, data flow | HTTP/HTTPS request/response traffic only |
| **Granularity** | Application-level context | Protocol/request-level filtering |
| **Blind Spots** | Encrypted internal logic attacks | Attacks within valid HTTP traffic |
| **Example** | Sqreen, Imperva RASP | AWS WAF, Cloudflare WAF |

---

### DevOps vs. DevSecOps Core Principles

| # | DevOps Principle | DevSecOps Principle |
|---|-----------------|---------------------|
| 1 | Collaboration: break silos between Dev & Ops | Collaboration: break silos between Dev, Ops & **Security** |
| 2 | Automation: tests and integrations | Automation: **security testing** and compliance |
| 3 | CI/CD: merge code, release code | **Shift Left**: move security earlier in SDLC |
| 4 | Continuous Monitoring: system performance | Continuous Monitoring: **real-time security** feedback |
| 5 | Rapid Response: bug fixes, change requests | Rapid Response: **vulnerability remediation** |

---

### IaC Tools Comparison

| Tool | Primary Use | License |
|------|------------|---------|
| Terraform | IaC for cloud automation | OSS/Paid |
| Pulumi | IaC using general-purpose languages | OSS/Paid |
| Ansible | Configuration management & automation | OSS |
| Puppet | Declarative configuration management | OSS/Paid |
| Chef | Declarative configuration management | OSS/Paid |

---

### Top DevOps Tools 2025 (LinkedIn Job Data)

| Rank | Tool | Category |
|------|------|----------|
| 1 | GitHub Actions | CI/CD |
| 2 | Terraform | IaC |
| 3 | Kubernetes | Container Orchestration |
| 4 | ArgoCD | GitOps CD |
| 5 | Docker | Containerization |
| 6 | Jenkins | CI/CD |
| 7 | Prometheus | Monitoring |
| 8 | Ansible | Config Management |
| 9 | Vault | Secrets Management |
| 10 | Pulumi | IaC |

---

## 4. Common Misconceptions & Traps

> ⚠️ These are high-risk wrong-answer traps for multiple-choice exams.

---

### ⚠️ Trap 1: "DevOps is a technology/framework"
**Correct:** DevOps is a **culture and set of practices**. There is no single DevOps technology, standard, or framework. Tools support DevOps, but they do not define it.

---

### ⚠️ Trap 2: "Continuous Delivery = Continuous Deployment"
**Correct:** These are **distinct**. Continuous Delivery means code is *ready* for production but requires a **manual approval gate**. Continuous Deployment *automatically* pushes every passing change to production with **no manual step**.

---

### ⚠️ Trap 3: "SAST runs on the running application"
**Correct:** SAST analyzes **source code without execution**. DAST runs against a **live, running application**. Mixing these up is a classic exam trap.

---

### ⚠️ Trap 4: "RASP is an external firewall"
**Correct:** RASP is **embedded inside** the application runtime. WAF is the external component sitting as a reverse proxy. RASP has internal visibility; WAF only sees network-level traffic.

---

### ⚠️ Trap 5: "Shift Left means moving deployment earlier"
**Correct:** Shift Left means moving **security practices earlier** in the SDLC — finding vulnerabilities at the code/commit stage rather than post-deployment. It has nothing to do with the deployment schedule itself.

---

### ⚠️ Trap 6: "Each Runner can run multiple jobs simultaneously"
**Correct:** Each GitHub Actions Runner can run **exactly one job at a time**. Parallel execution requires **multiple runners**.

---

### ⚠️ Trap 7: "SCA (Software Composition Analysis) scans your own code"
**Correct:** SCA specifically analyzes **third-party and open-source dependencies** (libraries, packages). SAST scans your own first-party source code.

---

### ⚠️ Trap 8: "SIEM is a single tool"
**Correct:** SIEM is a **set of tools** (a concept/category), not a single product. It encompasses data collection, aggregation, correlation, and alerting capabilities.

---

### ⚠️ Trap 9: "The 'M' in CALMS stands for Monitoring"
**Correct:** The 'M' stands for **Measurement** — quantitative metrics about the pipeline and system (lead time, failure rate, user counts, MTTR). Monitoring is a separate DevOps phase concept.

---

### ⚠️ Trap 10: "Lead Time for Changes measures how long deployment takes"
**Correct:** Lead Time for Changes measures the duration **from code commit to deployable state** — the entire pipeline duration, not just the deployment step itself.

---

## 5. High-Probability Exam Questions

---

### Q1
**Which of the following best defines DevOps according to the CSIRO/SEI definition?**

- A) A set of tools for automating software builds and deployments
- B) A set of practices to reduce turnaround time from committing a change to implementing it in production while maintaining quality ✅
- C) A framework for managing agile sprints and backlogs
- D) A methodology for integrating security into the software development lifecycle

> **Correct: B** — The CSIRO/SEI definition explicitly focuses on *practices*, *turnaround time*, and *quality*. A is wrong because DevOps is not defined by tools. C describes Agile. D describes DevSecOps.

---

### Q2
**What is the key difference between Continuous Delivery and Continuous Deployment?**

- A) Continuous Delivery deploys to staging; Continuous Deployment deploys to production
- B) Continuous Delivery requires passing all tests; Continuous Deployment does not
- C) Continuous Delivery requires a manual approval gate before production deployment; Continuous Deployment is fully automated ✅
- D) Continuous Delivery is faster than Continuous Deployment

> **Correct: C** — The defining distinction is the presence (Delivery) vs. absence (Deployment) of a **manual approval gate** before production. Both deploy to production; the trigger mechanism differs.

---

### Q3
**In the CALMS framework, which pillar is most directly associated with "configuration as code" and "automated deploys"?**

- A) Culture
- B) Lean
- C) Measurement
- D) Automation ✅

> **Correct: D** — The Automation pillar explicitly covers automated tests, continuous delivery, automated deploys, and configuration as code. Culture covers collaboration; Lean covers waste elimination; Measurement covers metrics.

---

### Q4
**A developer commits code containing `cursor.execute(f"SELECT * FROM users WHERE id={user_id}")`. Which DevSecOps security gate would most likely detect this vulnerability FIRST?**

- A) Gate #5 — DAST, because it simulates SQL injection attacks at runtime
- B) Gate #1 — SAST, because it analyzes source code statically without executing it ✅
- C) Gate #6 — RASP, because it monitors runtime application behavior
- D) Gate #2 — SCA, because it analyzes dependency vulnerabilities

> **Correct: B** — SAST operates on source code before execution and would flag this SQL concatenation pattern (CWE-89) at the earliest CI gate. DAST would only find it later on a running application. RASP would catch it in production. SCA checks third-party libraries, not your own code.

---

### Q5
**Which DORA metric measures the percentage of code changes that require hotfixes or other remediation after reaching production?**

- A) Lead Time for Changes
- B) Deployment Frequency
- C) Mean Time to Recovery (MTTR)
- D) Change Failure Rate ✅

> **Correct: D** — Change Failure Rate measures the proportion of deployments that cause production failures requiring remediation. MTTR measures recovery *duration*, not the *rate* of failures. Lead Time and Deployment Frequency measure speed, not quality.

---

### Q6
**A security tool is embedded directly within an application's runtime and can block a malicious SQL injection attempt before the database query executes. Which technology is this?**

- A) WAF (Web Application Firewall)
- B) SIEM (Security Information and Event Management)
- C) RASP (Runtime Application Self-Protection) ✅
- D) SAST (Static Application Security Testing)

> **Correct: C** — RASP is embedded *inside* the application and operates with internal visibility. WAF sits externally as a reverse proxy and can only inspect HTTP traffic. SIEM aggregates and alerts on events but does not block attacks inline. SAST is pre-execution code analysis.

---

### Q7
**In GitHub Actions, which component is responsible for executing the steps of a job?**

- A) Event
- B) Action
- C) Workflow
- D) Runner ✅

> **Correct: D** — A Runner is the server that executes workflow jobs when triggered. An Event *triggers* the workflow. A Workflow is the *definition* of the automated process. An Action is a reusable *task* within a step.

---

### Q8
**What does "Shift Left" mean in the context of DevSecOps?**

- A) Moving the deployment pipeline to an earlier time slot in the sprint calendar
- B) Integrating security testing and review earlier in the software development lifecycle ✅
- C) Moving operations responsibilities to the development team
- D) Prioritizing left-side pipeline stages (code, build) over right-side stages (deploy, monitor)

> **Correct: B** — Shift Left refers to moving security *activities* earlier in the SDLC timeline (toward the "left" of the pipeline diagram). The goal is earlier, cheaper vulnerability detection. It has no literal scheduling or organizational reallocation meaning.

---

### Q9
**Which of the following correctly describes the role of Software Composition Analysis (SCA) in a DevSecOps pipeline?**

- A) It analyzes first-party source code for logic errors and security vulnerabilities
- B) It scans the running application by simulating real-world attack scenarios
- C) It identifies vulnerabilities, license issues, and outdated components in third-party open-source dependencies ✅
- D) It monitors production traffic and blocks malicious HTTP requests

> **Correct: C** — SCA targets third-party/open-source *dependencies*, not your own code (that is SAST). Runtime attack simulation is DAST. Production traffic blocking is WAF.

---

### Q10
**Which statement about GitOps is correct?**

- A) GitOps means storing only application code in Git; infrastructure is managed manually
- B) GitOps relies on developers directly pushing changes to the production environment
- C) In GitOps, the live system is continuously reconciled to match the desired state defined in Git ✅
- D) GitOps replaces the need for CI/CD pipelines entirely

> **Correct: C** — GitOps' defining characteristic is **continuous reconciliation** — automated systems ensure the live environment matches what is declared in Git. Infrastructure *and* application config both live in Git (ruling out A). Changes go through pull request workflows, not direct pushes (ruling out B). GitOps complements CI/CD; it does not replace it (ruling out D).

---

## 6. One-Page Cheat Sheet Summary

```
╔══════════════════════════════════════════════════════════════╗
║          DevOps / CI/CD / DevSecOps — CHEAT SHEET           ║
╚══════════════════════════════════════════════════════════════╝

── DEVOPS ──────────────────────────────────────────────────────
• DevOps = Culture + Practices (NOT a tool/framework/standard)
• Goal: Reduce turnaround from commit → production, maintain quality
• CALMS: Culture | Automation | Lean | Measurement | Sharing
  - Automation = automated tests, deploys, config as code
  - Measurement = metrics, NOT "monitoring"

── DORA METRICS ────────────────────────────────────────────────
• Lead Time for Changes — commit → deployable state
• Change Failure Rate   — % deployments needing hotfix
• Deployment Frequency  — how often code goes to prod
• MTTR                  — recovery time from failure

── CI/CD PIPELINE ──────────────────────────────────────────────
• CI:  Code Commit → Build → Test → Integration Test
• CD (Delivery):    + Manual Approval Gate → Ready for Prod
• CD (Deployment):  + Automated Deploy → No Manual Gate
• KEY TRAP: Delivery ≠ Deployment (gate vs. no gate)

── DEVSECOPS ────────────────────────────────────────────────────
• DevSecOps = DevOps + Security integrated from the START
• "Shift Left" = security EARLIER in SDLC (not later)
• Security = everyone's responsibility

── SECURITY GATES ──────────────────────────────────────────────
Gate 1 - SAST:  Static analysis, NO execution, source code only
              → Tools: SonarQube, Semgrep, CodeQL
Gate 2 - SCA:   Third-party dependencies, license compliance
              → Tools: Snyk, OWASP Dependency Check
Gate 3 - Artifact: Container image scanning → Trivy
Gate 4 - IaC:   Infrastructure as code security → Checkov
Gate 5 - DAST:  Running app, simulates attacks, runtime
              → Tools: OWASP ZAP, Burp Suite
Gate 6 - Runtime: RASP (internal), WAF (external), SIEM

── SAST vs DAST ─────────────────────────────────────────────────
SAST: Source code, pre-execution, early CI, higher false positives
DAST: Running app, post-deploy, later CD, lower false positives

── RASP vs WAF ──────────────────────────────────────────────────
RASP: INSIDE app, internal behavior visibility, blocks internally
WAF:  OUTSIDE app, reverse proxy, HTTP/HTTPS traffic filtering

── SIEM ─────────────────────────────────────────────────────────
• Set of tools (not one tool): Collect → Aggregate → Correlate
  → Alert → Retain/Report

── GITHUB ACTIONS ───────────────────────────────────────────────
• Event    → triggers workflow (push, PR, schedule, webhook)
• Workflow → YAML file in .github/workflows/
• Job      → set of steps on ONE Runner
• Step     → shell script OR an Action
• Action   → reusable task (checkout, setup-node, docker build)
• Runner   → server executing ONE job at a time (Linux/Win/macOS)

── IAC & GITOPS ─────────────────────────────────────────────────
• IaC: Infrastructure defined in code files (Terraform, K8s YAML)
• GitOps: Config in Git → PR workflow → auto-reconcile live system
• Top 2025 tools: GitHub Actions, Terraform, Kubernetes, ArgoCD

── CRITICAL TRAPS ───────────────────────────────────────────────
❌ DevOps IS a technology         ✅ DevOps is a culture/practice
❌ Delivery = Deployment          ✅ Delivery needs manual gate
❌ SAST runs on live app          ✅ DAST runs on live app
❌ RASP is an external firewall   ✅ RASP is embedded in app
❌ Shift Left = earlier deploy    ✅ Shift Left = earlier security
❌ Each runner = multiple jobs    ✅ Each runner = ONE job
❌ SCA scans your code            ✅ SCA scans dependencies
❌ CALMS M = Monitoring           ✅ CALMS M = Measurement
```

---

*Study tip: For each security tool question, ask yourself: (1) Does it analyze code or a running system? (2) Is it internal or external to the app? (3) What pipeline stage does it block? These three questions resolve ~80% of DevSecOps MCQs.*
