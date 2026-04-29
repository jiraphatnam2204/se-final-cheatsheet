# 🧠 Microservice Architecture — Exam Study Guide
> Based on: *Intro to Microservice Patterns* (Richardson, Manning 2019)
> Exam in: **3 days** | Format: Multiple Choice

---

## 1. Core Concepts

| Term | Precise Definition |
|---|---|
| **Software Architecture** | The set of structures needed to reason about a computing system, comprising software *elements*, *relations* among them, and *properties* of both (Bass et al.). |
| **Architectural View** | A tuple of (elements, relations, properties) that describes one dimension of a system's architecture. Architecture is multi-dimensional and requires multiple views. |
| **Non-Functional Requirements (NFRs)** | Also called *quality attributes* or *"-ilities"*; the second category of requirements beyond functional behaviour, defining how well a system performs its functions (e.g., scalability, maintainability, reliability). |
| **Monolithic Architecture** | An architectural style that structures the entire application as a **single executable component** (one deployable unit). Captured in the *Implementation View*. |
| **Microservice Architecture** | An architectural style that structures an application as a **set of loosely coupled services organised around business capabilities**. Each service is an independently deployable component. |
| **Monolithic Hell** | The state reached when a large monolith becomes so complex that agile development and deployment become practically impossible — characterised by slow builds, painful merges, unreliable releases, and technology lock-in. |
| **Service (in MSA)** | An independently deployable component with its own API (Command/Query), Event Publisher/Subscriber, private database, and an SLA. Communicates via synchronous (REST, gRPC) or asynchronous (events, commands) protocols. |
| **Scale Cube** | A three-dimensional model for scaling applications: X-axis (horizontal duplication), Y-axis (functional decomposition), Z-axis (data partitioning). |
| **X-axis Scaling** | Scale by *cloning* — running N identical instances of an application behind a load balancer. |
| **Y-axis Scaling** | Scale by *functional decomposition* — splitting the application into separate services by function (e.g., Order Service, Customer Service). This is the microservice dimension. |
| **Z-axis Scaling** | Scale by *data partitioning* — each instance handles a subset of the data routed by a request attribute (e.g., userId). |
| **Development Velocity** | The rate at which a team can deliver features to production. Directly governed by Maintainability, Testability, and Deployability. |
| **DevOps / Continuous Delivery** | A set of practices for rapid, frequent, and reliable software delivery. Requires testability, deployability, and autonomous teams — all enabled by microservices. |
| **Distributed Monolith** | The worst-case anti-pattern: incorrect decomposition of services that results in a system with the drawbacks of *both* monolithic and microservice architectures and the benefits of neither. |
| **Saga Pattern** | A mechanism used in microservices to maintain data consistency *across services* without distributed transactions, using a sequence of local transactions coordinated via events or commands. |
| **API Composition** | A query pattern where a dedicated component calls multiple services and combines their results to answer a query that spans service boundaries. |
| **CQRS (Command Query Responsibility Segregation)** | A pattern that separates read (query) and write (command) models. Used in microservices to implement cross-service queries via read-optimised views. |
| **API Gateway** | An entry-point component that routes requests from clients (e.g., mobile apps) to the appropriate backend services. |
| **Domain-Driven Design (DDD) Subdomains** | A technique for decomposing a system into services aligned with distinct business capabilities or bounded contexts. |

---

## 2. Mechanisms & How Things Work

### 2.1 How a Monolith Grows into Monolithic Hell

```
Step 1: Single team builds a small, manageable monolithic application.
        → Maintainability, Testability, Deployability are acceptable.

Step 2: Application grows in business value → more features added.
        → Codebase expands; more teams work on the same repository.

Step 3: Multiple teams share one codebase and one deployment pipeline.
        → Communication/coordination overhead increases dramatically.
        → Build pipeline has a backlog; changes queue for manual testing.

Step 4: Symptoms emerge:
        - IDE slows down; startup times become long.
        - Complexity intimidates developers; bugs become hard to isolate.
        - Build is frequently in an unreleasable state due to merge conflicts.
        - Scaling is inefficient (must scale entire app even for one bottleneck).
        - Technology stack becomes obsolete and hard to upgrade.
        - Frequent production outages due to lack of testability.

Result → MONOLITHIC HELL: Agile development and deployment become impossible.
```

### 2.2 How X-axis Scaling Works

```
1. Deploy N *identical* copies of the application.
2. Place a load balancer in front of all instances.
3. Load balancer distributes incoming requests across N instances using
   a load-balancing algorithm (e.g., round-robin, least-connections).
4. Each instance shares access to the same underlying data store.

Effect: Improves capacity (throughput) and availability.
Limitation: Does NOT reduce complexity; every instance still runs all features.
```

### 2.3 How Z-axis Scaling Works

```
1. Deploy N instances of the application (similar to X-axis).
2. Each instance is responsible for only a *subset* of the data
   (e.g., Instance 1: Users a–h, Instance 2: Users i–p, Instance 3: Users r–z).
3. A router inspects a request attribute (e.g., userId in Authorization header)
   and routes each request to the instance responsible for that data partition.

Effect: Scales both transaction volume and data volume.
Limitation: Still does not reduce application complexity.
```

### 2.4 How Y-axis Scaling (Microservices) Works

```
1. Identify distinct business capabilities (e.g., Orders, Customers, Reviews).
2. Decompose the monolith: each capability becomes an independent service.
3. Each service:
   - Has its own source code repository.
   - Has its own automated deployment pipeline (e.g., Jenkins CI).
   - Has its own private database (data is NOT shared directly).
   - Exposes an API (REST, gRPC, events).
4. An API Gateway routes external client requests to the correct service.
5. Each service can itself be independently scaled using X-axis or Z-axis.

Effect: Reduces complexity, enables autonomous teams, independent deployability.
```

### 2.5 How the Process/Organisation/Architecture Triangle Works

```
Three mutually enabling pillars:

  [Process: DevOps / CD]
       /\
      /  \
Enables   Enables
    /      \
[Organisation] ←Enables← [Architecture]
Small, autonomous teams    Microservice architecture

- MSA → improves testability & deployability → enables DevOps/CD (Process).
- MSA → each team owns one or more services → enables small autonomous teams (Organisation).
- Autonomous teams → can independently operate their service → enables MSA (Architecture).
```

---

## 3. Comparisons & Trade-offs

### 3.1 Monolith vs. Microservice Architecture

| Dimension | Monolith (Small) | Monolith (Large) | Microservice Architecture |
|---|---|---|---|
| **Maintainability** | ✅ Good | ❌ Poor — overwhelming complexity | ✅ Good — small, focused codebases |
| **Testability** | ✅ Good | ❌ Poor — long test cycles, outages | ✅ Good — services tested in isolation |
| **Deployability** | ✅ Good | ❌ Poor — painful merges, long pipeline | ✅ Good — independent deployments |
| **Scalability** | ✅ Simple (X-axis) | ❌ Inefficient — must scale everything | ✅ Fine-grained — scale per service |
| **Technology Flexibility** | ✅ Simple | ❌ Locked in — hard to upgrade | ✅ High — pick stack per service |
| **Team Autonomy** | ✅ Single team | ❌ Poor — coordination overhead | ✅ High — teams own services |
| **Operational Complexity** | ✅ Low | ✅ Low (but broken) | ❌ High — distributed systems complexity |
| **Data Consistency** | ✅ Simple (ACID DB) | ✅ Simple | ❌ Hard — requires Sagas |
| **Cross-service Queries** | ✅ Simple SQL joins | ✅ Simple | ❌ Hard — requires API Composition/CQRS |
| **When to use** | Small/simple apps, startups | ❌ Avoid | Large/complex apps at scale |

### 3.2 Scale Cube Axes Compared

| Axis | Strategy | What it solves | What it does NOT solve |
|---|---|---|---|
| **X-axis** | Clone N identical instances | Throughput, availability | Complexity |
| **Y-axis** | Decompose by function (microservices) | Complexity, team autonomy | Volume per service (needs X or Z) |
| **Z-axis** | Partition by data attribute | Data volume, transaction volume | Complexity |

### 3.3 Microservice Benefits vs. Drawbacks

| Benefits | Drawbacks |
|---|---|
| Enables continuous delivery/deployment | Finding the right service decomposition is hard |
| Small, maintainable, understandable codebases | Distributed systems are inherently complex |
| Independent testability and deployability | Requires unfamiliar patterns (Sagas, CQRS, API Composition) |
| Services scale independently | Cross-service features require careful coordinated rollouts |
| Teams are autonomous and loosely coupled | Deciding *when* to adopt MSA is difficult |
| Fault isolation — one service failure doesn't cascade | Risk of building a *distributed monolith* if decomposed incorrectly |
| Freedom to evolve technology stack per service | Higher operational overhead (multiple pipelines, observability, etc.) |

---

## 4. Common Misconceptions & Traps

> ⚠️ These are high-likelihood wrong-answer choices in MCQ format.

1. **"Microservices = silver bullet"**
   → **FALSE.** The slide explicitly states "Microservices ≠ silver bullet." Referencing Fred Brooks: no single technique promises an order-of-magnitude improvement by itself.

2. **"A monolith is always bad"**
   → **FALSE.** Small monoliths have *good* maintainability, testability, and deployability. The monolith is a valid choice for small or early-stage applications.

3. **"X-axis scaling solves complexity"**
   → **FALSE.** X-axis scaling only improves capacity/availability. It runs identical copies of the whole application and does *nothing* to reduce code complexity.

4. **"Y-axis scaling = running multiple instances"**
   → **FALSE.** Running multiple instances is X-axis. Y-axis is *functional decomposition* — splitting the application into distinct services by business capability.

5. **"Microservices allow services to share a database"**
   → **FALSE.** A core principle: *a service's data is private*. Other services must access it through the service's API, not directly via the database.

6. **"Microservices eliminate the need for data consistency mechanisms"**
   → **FALSE.** Because each service has its own database, distributed transactions are unavailable. You *must* use Sagas for cross-service data consistency.

7. **"A startup should immediately adopt microservices"**
   → **FALSE.** The slide explicitly states startups should begin with a monolith. Adopt microservices when complexity becomes the primary problem.

8. **"Incorrect service decomposition is just suboptimal"**
   → **FALSE.** Incorrect decomposition produces a *distributed monolith* — the worst outcome, combining the drawbacks of both architectures with the benefits of neither.

9. **"Architecture is one-dimensional"**
   → **FALSE.** Architecture is multi-dimensional (like a building having structural, electrical, plumbing views). Software architecture requires multiple views: Logical View, Implementation View, etc.

10. **"Z-axis is the same as Y-axis — both split things"**
    → **FALSE.** Z-axis splits *similar* things (same application, different data partitions). Y-axis splits *different* things (different services by business function).

---

## 5. High-Probability Exam Questions

---

**Q1.** Which of the following *best* defines the microservice architectural style?

- A) An application structured as a single executable component for simplicity
- B) An application structured as a set of loosely coupled services organised around business capabilities ✅
- C) An application that runs on multiple servers behind a load balancer
- D) An application that uses multiple databases, one per team

**Answer: B**
- A → Definition of *monolithic* architecture.
- C → Describes X-axis scaling, not microservices specifically.
- D → Having multiple databases is a *consequence* of MSA, not its definition.

---

**Q2.** A system uses a router to direct requests from Users a–h to Instance 1, Users i–p to Instance 2, and Users r–z to Instance 3. Which scaling strategy does this describe?

- A) Y-axis scaling
- B) X-axis scaling
- C) Z-axis scaling ✅
- D) Functional decomposition

**Answer: C**
- A → Y-axis splits by *function* (different services), not by data partitions.
- B → X-axis runs *identical* instances with a load balancer — no request-attribute-based routing.
- D → Functional decomposition is another name for Y-axis, not Z-axis.

---

**Q3.** Which combination of quality attributes is *most directly* impacted by development velocity in the architecture discussed?

- A) Scalability, Security, Reliability
- B) Maintainability, Testability, Deployability ✅
- C) Availability, Fault Tolerance, Evolvability
- D) Performance, Usability, Portability

**Answer: B**
- The slide explicitly highlights Maintainability, Testability, and Deployability as the "-ilities" that drive development velocity.
- A, C, D → These are valid NFRs but are not the ones specifically framed as governing velocity in this material.

---

**Q4.** A startup has just launched a food-delivery application with a small team and limited complexity. According to the principles in this lecture, what architecture should they adopt?

- A) Microservice architecture from day one for future scalability
- B) A distributed monolith to get the benefits of both styles
- C) A monolithic architecture ✅
- D) Z-axis scaled architecture with multiple data partitions

**Answer: C**
- A → Premature adoption of MSA introduces operational overhead without solving a real problem.
- B → A distributed monolith is an *anti-pattern*, not a deliberate design choice.
- D → Z-axis scaling addresses data volume problems, not startup-phase complexity.

---

**Q5.** What is the primary danger of decomposing a system into microservices *incorrectly*?

- A) The system will fail to scale horizontally
- B) The services will be unable to use REST APIs
- C) A distributed monolith will be created, inheriting the drawbacks of both architectures ✅
- D) The system will revert automatically to a monolithic architecture

**Answer: C**
- A → Scaling is a separate concern from decomposition correctness.
- B → REST API usage is independent of decomposition quality.
- D → There is no automatic reversion; the distributed monolith persists until refactored.

---

**Q6.** In microservice architecture, how is data consistency *across multiple services* typically maintained?

- A) By sharing a single central database across all services
- B) By using distributed ACID transactions
- C) By using the Saga pattern ✅
- D) By using Z-axis data partitioning

**Answer: C**
- A → Violates the core MSA principle that each service owns its private data.
- B → Distributed ACID transactions are impractical across service boundaries in MSA and are avoided.
- D → Z-axis partitioning is a scaling strategy, not a consistency mechanism.

---

**Q7.** Which of the following is NOT a symptom of monolithic hell?

- A) The build is frequently in an unreleasable state due to painful merges
- B) Each team has its own independent deployment pipeline ✅
- C) The application takes a very long time to start up
- D) The technology stack is difficult to upgrade

**Answer: B**
- B → Independent deployment pipelines per team are a *benefit* of microservice architecture, the *solution* to monolithic hell, not a symptom of it.
- A, C, D → All are explicitly listed as symptoms of monolithic hell in the slides.

---

**Q8.** According to the Scale Cube model, which axis directly corresponds to the microservice architecture pattern?

- A) X-axis — horizontal duplication
- B) Y-axis — functional decomposition ✅
- C) Z-axis — data partitioning
- D) All three axes equally

**Answer: B**
- Y-axis is explicitly labelled as the microservice dimension in the Scale Cube (splitting by function/service).
- A → X-axis is load-balanced cloning.
- C → Z-axis is data-level partitioning.
- D → MSA is specifically enabled by Y-axis; X and Z can be applied *within* individual services afterward.

---

**Q9.** What does it mean that "architecture is multi-dimensional" in the context of software systems?

- A) The system must run on multiple physical machines
- B) A system can only be understood through a single, comprehensive diagram
- C) Architecture requires multiple views (elements, relations, properties) to fully describe a system ✅
- D) The system must use multiple programming languages

**Answer: C**
- The slide draws an analogy to building architecture (structural, electrical, plumbing, mechanical blueprints). Software architecture similarly requires multiple views.
- A → Physical distribution is an operational concern, not an architectural view concept.
- B → The opposite is true — one view is insufficient.
- D → Multiple languages are a technology choice enabled by MSA, not a definition of multi-dimensional architecture.

---

**Q10.** Which of the following represents the *correct relationship* between Process, Organisation, and Architecture in modern software development?

- A) Process dictates Architecture, which then dictates Organisation — a one-way hierarchy
- B) Each of the three pillars mutually enables the others in a virtuous cycle ✅
- C) Organisation is independent and has no influence on Architecture
- D) Architecture is determined solely by technical constraints, unrelated to team structure

**Answer: B**
- The slide shows a triangle where: MSA enables DevOps/CD (Process), MSA enables autonomous teams (Organisation), and autonomous teams enable MSA (Architecture) — all mutually reinforcing.
- A → The relationship is cyclic and bidirectional, not a strict hierarchy.
- C → Conway's Law (implicitly referenced) shows team structure and architecture are tightly coupled.
- D → Contradicts the slide's explicit "Teams own services" and the Architecture ↔ Organisation relationship.

---

## 6. One-Page Cheat Sheet Summary

### Architecture Fundamentals
- **Architecture** = structures of (elements, relations, properties) — multi-dimensional → described by multiple *views*
- **Goal of architecture** → satisfy **non-functional requirements** (NFRs / "-ilities")
- **Key -ilities for velocity**: Maintainability, Testability, Deployability

### Monolith
- **Definition**: Single executable component (one deployable unit)
- **Small monolith** → ✅ good -ilities | **Large monolith** → ❌ monolithic hell
- **Monolithic hell symptoms**: slow IDE, long startup, painful merges, unreleasable builds, scaling difficulty, tech stack lock-in, frequent outages

### Microservices
- **Definition**: Loosely coupled services organised around **business capabilities**
- **Each service**: independent deployment, private database, own API (REST/gRPC/events), own SLA
- **Data is private** — no shared databases between services
- **API Gateway** routes external requests to services

### Scale Cube (memorise all 3)
| Axis | Name | Method | Solves |
|---|---|---|---|
| X | Horizontal duplication | Clone + load balance | Throughput |
| Y | Functional decomposition | Split by function = **Microservices** | Complexity |
| Z | Data partitioning | Route by attribute (e.g., userId) | Data volume |

> Each microservice is itself scalable via X and Z axes.

### MSA Benefits
- Enables CI/CD for large complex apps
- Small, fast, understandable codebases
- Independent deployability & scalability per service
- Autonomous teams (own their service end-to-end)
- Better fault isolation
- Freedom to evolve tech stack per service

### MSA Drawbacks
- Finding right service boundaries is hard → risk of **distributed monolith** (worst outcome)
- Distributed systems complexity → requires Sagas, API Composition, CQRS
- Cross-service features need coordinated rollout plans
- Hard to know *when* to adopt → **startups: start monolith first**

### Critical Rules
1. Microservices ≠ silver bullet (Fred Brooks)
2. Small app → Monolith | Complex app → Microservices
3. Wrong decomposition → Distributed Monolith (drawbacks of both, benefits of neither)
4. Cross-service consistency → **Sagas** (no distributed transactions)
5. Cross-service queries → **API Composition** or **CQRS views**
6. Process + Organisation + Architecture → mutually enabling triangle

### High-Value Definitions (verbatim-level)
> "The microservice architecture is an architectural style that structures an application as a **set of loosely coupled services organised around business capabilities**."

> "The monolithic architecture is an architectural style that structures the application as a **single executable component**."

> "Software architecture = structures comprising software **elements**, **relations** among them, and **properties** of both." — Bass et al.
