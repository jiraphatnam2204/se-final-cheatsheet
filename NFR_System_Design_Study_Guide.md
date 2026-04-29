# 📚 System Design Based on Non-Functional Requirements — Exam Study Guide

> **Exam in 3 days. Use this guide with active recall: cover each section and test yourself before reading answers.**

---

## 1. Core Concepts

### Physical Architecture Layer Design
The design phase that determines **how a system is distributed across hardware**, specifying which software components run on which machines, and which networks connect them. It encompasses both hardware and software components.

### Software Components (Four Layers)
| Layer | Responsibility |
|---|---|
| **Data Storage** | Persistent storage — files, databases |
| **Data Access Logic** | DAM classes, SQL queries; reads/writes data |
| **Application Logic** | Problem domain / business rules processing |
| **Presentation Logic** | HCI layer; what the user sees and interacts with |

### Non-Functional Requirements (NFRs)
Constraints on a system that are *not* about what the system does, but **how well it does it**. NFRs govern architecture selection and software placement decisions. They are categorized as:
- **Operational** — technical environment, system integration, portability, maintainability
- **Performance** — speed, capacity, availability & reliability
- **Security** — system value, access control, encryption/authentication, virus control
- **Cultural & Political** — customization, multilingual support, legal compliance

### Server-Based Architecture
All four application functions (data storage, data access, application, presentation) reside on a single server/mainframe. Clients are dumb terminals that only send keystrokes and display output.

### Client-Based Architecture
All processing logic (data access, application, presentation) resides on the client (microcomputer). The server stores data only. All data is downloaded to the client for processing.

### Client-Server Architecture
Processing is **balanced** between client and server. The predominant architecture in modern systems. Exists in two forms:
- **Thin Client**: Client handles presentation logic only; server handles application logic, data access, and storage.
- **Thick Client**: Client handles both presentation and application logic; server handles data access and storage.

### Two-Tiered Architecture
A specific client-server arrangement with one client tier and one server tier. The client may be thin or thick.

### Three-Tiered Architecture
Client-server split into three layers: the **client** (presentation), an **application server** (application logic), and a **database server** (data access + storage).

### N-Tier Architecture
Application logic is split across **two or more application servers**, with data logic on separate database server(s). Common in e-commerce. Maximizes scalability and load balancing but places high network demands.

### Cloud Computing
A model treating IT infrastructure as a utility/commodity via a **pay-as-you-go** model. Three service models:
- **IaaS** (Infrastructure as a Service): Raw compute, storage, networking — e.g., AWS EC2, S3
- **PaaS** (Platform as a Service): Development platforms — e.g., Google App Engine
- **SaaS** (Software as a Service): End-user applications — e.g., Salesforce, Google Apps

Three deployment models: **Private**, **Public**, **Hybrid**.

### Ubiquitous Computing / Internet of Things (IoT)
Computing embedded into everyday objects (ubiquitous computing) that are then connected to the Internet (IoT). Enabled by RFID, QR codes, Bluetooth, ZigBee, WiFi-Direct.

### Deployment Diagram
A UML diagram representing the physical deployment of software artifacts onto hardware nodes. Elements:
- **Node**: A computational resource (server, client, network device)
- **Artifact**: A deployable piece of software or database installed on a node
- **Communication Path**: A link between nodes (labeled with protocol, e.g., `<<LAN>>`, `<<TCP/IP>>`)

### Type-Level vs. Instance-Level Deployment Diagram
- **Type-level**: Shows classes of hardware (e.g., "Dell PowerEdge R310") — general, reusable
- **Instance-level**: Shows specific named instances (e.g., "WebServer1 : Dell PowerEdge R310") — concrete deployment

### Network Model
A diagram depicting the **geographic locations** of major system components and how they interconnect. Complements the deployment diagram by conveying spatial complexity and communication infrastructure cost.

### Green IT
Practices that reduce the **environmental impact** of IT: e-waste reduction, energy-efficient data centers, cloud virtualization to reduce physical servers, Energy Star compliance, paperless office initiatives.

---

## 2. Mechanisms & How Things Work

### How NFRs Drive Architecture Selection

The process flows as follows:

1. **Gather NFRs** during requirements analysis (operational, performance, security, cultural/legal).
2. **Map each NFR category** to the architectural characteristics that best satisfy it (refer to the NFR-architecture implication matrix on slide 19).
3. **Select the architecture** (server-based, client-based, thin or thick client-server) that best satisfies the dominant NFRs, given cost and constraint trade-offs.
4. **Determine software component placement**: decide which of the four layers (data storage, data access, application, presentation) goes onto which physical node.
5. **Produce deployment diagrams and network models** to formalize and communicate the design.

### How Client-Server Thin/Thick Tiering Works

```
THIN CLIENT:
  [Client PC]                [Server]
  Presentation Logic   <-->  Application Logic
                             Data Access Logic
                             Data Storage

THICK CLIENT:
  [Client PC]                [Server]
  Presentation Logic   <-->  Data Access Logic
  Application Logic          Data Storage
```

- In **thin**: the server does the heavy lifting; client is essentially a browser or display renderer.
- In **thick**: client executes business logic locally; requires more powerful client machines but reduces server load.

### How N-Tier Scaling Works

```
[Client] --> [Web Server] --> [App Server 1] --> [DB Server]
                          --> [App Server 2] --> [DB Server]
```

- Web server receives requests and distributes to application servers.
- Application servers are independently scalable (add/replace without touching other tiers).
- Database server is accessed via JDBC or similar protocol.
- **Horizontal scaling (scale out)**: add more application server instances.
- **Vertical scaling (scale up)**: replace an application server with a more powerful one.

### How UML Deployment Diagrams Are Constructed

1. Identify all **hardware nodes** (clients, servers, network devices).
2. Identify all **software artifacts** to be deployed (executables, JARs, WARs, DBs).
3. Assign artifacts to nodes using **nesting** (artifact inside a node box) or **deploy dependency arrows** (dashed arrow labeled `<<deploy>>`).
4. Connect nodes with **communication paths** labeled by protocol (e.g., `<<HTTP>>`, `<<TCP/IP>>`, `<<LAN>>`).
5. Optionally use **stereotypes** to clarify node type (`<<device>>`, `<<execution environment>>`) and artifact type (`<<executable>>`, `<<database>>`).
6. Use **property strings** `{key=value}` for additional metadata (e.g., `{OS=Linux}`).
7. Add **dependency arrows** (dashed with open arrowhead) between artifacts when one depends on another.

---

## 3. Comparisons & Trade-offs

### Architecture Characteristics Matrix

| Characteristic | Server-Based | Client-Based | Client-Server |
|---|---|---|---|
| Cost of Infrastructure | Very High | Medium | **Low** |
| Cost of Development | Medium | **Low** | High |
| Ease of Development | **Low** | High | Low–Medium |
| Interface Capabilities | Low | **High** | **High** |
| Control & Security | **High** | Low | Medium |
| Scalability | Low | Medium | **High** |

> **Key insight**: No single architecture wins on all dimensions. Architecture selection is a **trade-off analysis** driven by which NFRs are most critical for the system.

### Thin vs. Thick Client-Server

| Dimension | Thin Client | Thick Client |
|---|---|---|
| Client processing | Presentation only | Presentation + Application |
| Server load | Higher | Lower |
| Client hardware req. | Low | Higher |
| Deployment ease | Easy (web-based) | Complex (install on each client) |
| Portability | High (HTML/XML standards) | Low |
| Offline capability | None | Possible |

### Cloud Service Models

| Model | Customer Controls | Provider Controls | Example |
|---|---|---|---|
| IaaS | OS, Apps, Data | Hardware, network | AWS EC2 |
| PaaS | Apps, Data | OS, runtime, hardware | Google App Engine |
| SaaS | Data only | Everything else | Salesforce |

### NFR-to-Architecture Implication Matrix (Critical for Exam)

| NFR Type | Server-Based | Client-Based | Thin Client-Server | Thick Client-Server |
|---|---|---|---|---|
| System Integration | ✓ | | ✓ | ✓ |
| Portability | | | ✓ | |
| Maintainability | ✓ | | ✓ | |
| Speed Requirements | | | ✓ | ✓ |
| Capacity Requirements | | | ✓ | ✓ |
| Availability/Reliability | ✓ | | ✓ | ✓ |
| High System Value (Security) | ✓ | | ✓ | |
| Access Control | ✓ | | | |
| Encryption/Authentication | | | ✓ | ✓ |
| Virus Control | ✓ | | | |
| Multilingual | | | ✓ | |
| Customization | | | ✓ | |
| Legal Requirements | ✓ | | ✓ | ✓ |

---

## 4. Common Misconceptions & Traps

> ⚠️ These are high-probability wrong answer choices. Know them cold.

- **Trap 1 — "Client-based is cheapest overall"**: Client-based has medium infrastructure cost, not the lowest. **Client-server has the lowest** infrastructure cost, though its development cost is highest.

- **Trap 2 — "Server-based is the most scalable"**: Server-based has **low** scalability. It upgrades in large, expensive increments. Client-server is the most scalable.

- **Trap 3 — "Thick client = more secure"**: Thick clients push logic to the client, creating **more attack surface**. Server-based and thin client-server are more appropriate for high-security requirements.

- **Trap 4 — "Thin client handles both presentation and application logic"**: Thin client handles **presentation logic only**. Thick client handles presentation **and** application logic.

- **Trap 5 — "N-tier and three-tier are the same"**: Three-tier is a specific case with three distinct tiers. N-tier generalizes this: application logic may be distributed across **multiple** application servers, not just one.

- **Trap 6 — "PaaS gives the most control to the customer"**: IaaS gives the most control (OS, middleware, apps, data). SaaS gives the least. PaaS is in between.

- **Trap 7 — "Portability is best served by thick client-server"**: Portability is best served by **thin client-server** because web-based standards (HTML, XML) are platform-agnostic. Thick clients require platform-specific installation.

- **Trap 8 — "Maintainability favors client-based because it's simple"**: Maintainability is **poor** in client-based and thick client-server because software must be **reinstalled on every client machine** when updated. Server-based and thin client-server are better.

- **Trap 9 — "Deployment diagram nodes only represent hardware"**: Nodes can be **hardware nodes** (physical devices) **or software nodes** (execution environments like JVMs, EJB containers). Both are valid nodes.

- **Trap 10 — "A network model and a deployment diagram are the same thing"**: A network model focuses on **geographic locations** and communication infrastructure complexity. A deployment diagram focuses on **software artifact placement** on hardware. Deployment diagram notation *can be used* to draw a network model, but they serve different purposes.

- **Trap 11 — "IoT and ubiquitous computing are synonyms"**: Ubiquitous computing = computing embedded in objects. IoT = those objects **connected via the Internet**. IoT is a superset of ubiquitous computing in terms of connectivity.

---

## 5. High-Probability Exam Questions

---

### Q1
**A system requires that software updates be deployed without visiting individual user machines. Which architecture best satisfies this maintainability requirement?**

- A) Client-based architecture
- B) Thick client-server architecture
- C) Thin client-server architecture
- D) N-tier architecture with thick clients

**✅ Correct Answer: C**

> **Explanation**: Thin client-server centralizes application and data logic on the server. Updates are made once on the server and immediately available to all clients. **A** and **B** are wrong because they distribute logic to client machines, requiring reinstallation. **D** is wrong for the same reason — thick client adds client-side logic.

---

### Q2
**Which architecture characteristic does client-server architecture score HIGHEST on compared to server-based and client-based?**

- A) Ease of development
- B) Control and security
- C) Scalability
- D) Cost of infrastructure

**✅ Correct Answer: C**

> **Explanation**: Client-server scores High on scalability; servers can be added incrementally. **A** — ease of development is Low-Medium for client-server, High for client-based. **B** — security is High for server-based, not client-server. **D** — infrastructure cost is Low for client-server, but this is not where it uniquely excels vs. the others in terms of being the *highest* differentiator.

---

### Q3
**In a three-tiered client-server architecture, where does application logic reside?**

- A) On the client machine
- B) On the database server
- C) Distributed equally across all tiers
- D) On a dedicated application server separate from the database server

**✅ Correct Answer: D**

> **Explanation**: Three-tier separates concerns into client (presentation), application server (application logic), and database server (data access + storage). **A** would describe a thick two-tier client. **B** conflates the application and data tiers. **C** is incorrect; each tier has a specific responsibility.

---

### Q4
**A company requires that personal data cannot be transferred out of EU countries. Which NFR category does this represent?**

- A) Performance — Capacity Requirements
- B) Security — Access Control Requirements
- C) Cultural/Political — Legal Requirements
- D) Operational — System Integration Requirements

**✅ Correct Answer: C**

> **Explanation**: Laws and government regulations that impose constraints on the system are classified as Legal Requirements under the Cultural/Political NFR category. **B** is wrong — access control defines who within the system can access what, not cross-border data transfer law. **D** and **A** do not address legal/regulatory compliance.

---

### Q5
**A thin client-server architecture is preferred over a thick client-server architecture for multilingual requirements because:**

- A) The thin client stores language packs locally for faster rendering
- B) Separating presentation logic from application logic allows different language interfaces without changing backend logic
- C) Thin clients have more processing power for language translation
- D) Application servers in thin client architectures natively support Unicode

**✅ Correct Answer: B**

> **Explanation**: Thin client-server separates the presentation layer (which handles language rendering) from application logic. You can deploy multiple language-specific front-ends while keeping the backend unchanged. **A** is false — thin clients do minimal local processing. **C** and **D** are fabricated technical claims not supported by the material.

---

### Q6
**Which of the following is a UML deployment diagram artifact, NOT a node?**

- A) A web server running Apache HTTP Server
- B) A database server machine
- C) A compiled Java JAR file deployed on an application server
- D) A client workstation

**✅ Correct Answer: C**

> **Explanation**: An **artifact** is a deployable piece of software (executable, JAR, WAR, database schema). **A**, **B**, and **D** are all hardware computational resources — these are **nodes**.

---

### Q7
**In cloud computing service models, which model gives the customer the LEAST control over the underlying infrastructure?**

- A) IaaS
- B) PaaS
- C) SaaS
- D) Hybrid deployment

**✅ Correct Answer: C**

> **Explanation**: In SaaS, the customer only controls their data. The provider manages everything else — application, runtime, OS, servers. **IaaS** gives the most control (OS upward). **PaaS** is intermediate. **D** refers to a deployment model (public/private mix), not a service model.

---

### Q8
**A system has a high security value and strict access control requirements. Which architecture is MOST appropriate?**

- A) Client-based architecture
- B) N-tier architecture with thick clients
- C) Server-based architecture
- D) Thin client-server with no firewall

**✅ Correct Answer: C**

> **Explanation**: Server-based architecture concentrates all software in one location on a mainframe OS, which is more secure than microcomputer OSes and provides a single point of control. **A** (client-based) has Low control and security. **B** (thick client N-tier) distributes logic to clients, increasing attack surface. **D** eliminates a critical security component.

---

### Q9
**What is the primary distinction between a TYPE-LEVEL and an INSTANCE-LEVEL deployment diagram?**

- A) Type-level shows software artifacts; instance-level shows only hardware
- B) Type-level shows generic classes of nodes; instance-level shows specific named instances
- C) Type-level is used for planning; instance-level is only for documentation
- D) Instance-level diagrams cannot show communication paths

**✅ Correct Answer: B**

> **Explanation**: A type-level diagram uses generic type names (e.g., "Dell PowerEdge R310"), while an instance-level diagram names specific instances (e.g., "WebServer1 : Dell PowerEdge R310", using the `instanceName : TypeName` convention). **A**, **C**, and **D** are all false distinctions not defined in the material.

---

### Q10
**An e-commerce company experiences uneven load: sometimes the web front-end is overwhelmed, sometimes the business logic servers are. They want to scale each component independently. Which architecture BEST supports this?**

- A) Server-based architecture
- B) Two-tiered thick client-server
- C) N-tier architecture
- D) Client-based architecture

**✅ Correct Answer: C**

> **Explanation**: N-tier architecture separates concerns across multiple independently scalable servers (web server, multiple application servers, database server). Load balancing can be applied at each tier independently. **A** cannot scale components independently. **B** has only two tiers; scaling is coarser. **D** pushes load to clients, not server-side scale.

---

## 6. One-Page Cheat Sheet Summary

```
╔══════════════════════════════════════════════════════════════════════╗
║       SYSTEM DESIGN — NFR EXAM CHEAT SHEET                          ║
╠══════════════════════════════════════════════════════════════════════╣
║ FOUR SOFTWARE LAYERS (in order, server → client):                   ║
║  1. Data Storage  2. Data Access Logic  3. Application Logic        ║
║  4. Presentation Logic                                               ║
╠══════════════════════════════════════════════════════════════════════╣
║ ARCHITECTURES & WHO DOES WHAT:                                       ║
║  Server-Based: Server does ALL four layers. Client = dumb terminal.  ║
║  Client-Based: Client does 3 layers (DAL, App, Pres). Server=data.  ║
║  Thin C-S:     Client = Presentation only. Server = rest.           ║
║  Thick C-S:    Client = Presentation + App. Server = data layers.   ║
║  3-Tier:       Client=Pres | App Server=App | DB Server=Data.       ║
║  N-Tier:       Multiple app servers + load balancing.               ║
╠══════════════════════════════════════════════════════════════════════╣
║ ARCHITECTURE CHARACTERISTICS (memorize the extremes):               ║
║  Cheapest infrastructure   → Client-Server                          ║
║  Cheapest development      → Client-Based                           ║
║  Easiest to develop        → Client-Based                           ║
║  Best security/control     → Server-Based                           ║
║  Most scalable             → Client-Server (N-Tier = most of all)   ║
║  Best interface capability → Client-Based or Client-Server          ║
╠══════════════════════════════════════════════════════════════════════╣
║ NFR → ARCHITECTURE MAPPING:                                          ║
║  Portability          → Thin Client-Server (web standards)          ║
║  Maintainability      → Server-Based OR Thin C-S (no reinstall)     ║
║  Speed/Capacity       → Thin or Thick C-S (per-tier scaling)        ║
║  Availability         → Thin/Thick C-S (redundant servers per tier) ║
║  High Security Value  → Server-Based (mainframe OS) or Thin C-S     ║
║  Access Control       → Server-Based (single control point)         ║
║  Encryption/Auth      → Thin or Thick C-S (internet tools)          ║
║  Virus Control        → Server-Based (reduce desktop functions)      ║
║  Multilingual/Custom  → Thin C-S (decouple presentation layer)      ║
║  Legal Compliance     → Thin/Thick C-S or Server-Based              ║
╠══════════════════════════════════════════════════════════════════════╣
║ NFR CATEGORIES:                                                       ║
║  Operational: technical env, integration, portability, maintainability║
║  Performance: speed, capacity, availability & reliability            ║
║  Security: system value, access control, encryption, virus           ║
║  Cultural/Political: customization, multilingual, legal              ║
╠══════════════════════════════════════════════════════════════════════╣
║ CLOUD:                                                                ║
║  IaaS = most control (you manage OS up)                              ║
║  PaaS = middle ground                                                ║
║  SaaS = least control (manage data only)                             ║
║  Deployments: Private | Public | Hybrid                              ║
╠══════════════════════════════════════════════════════════════════════╣
║ DEPLOYMENT DIAGRAM:                                                   ║
║  Node = hardware or execution environment (3D box shape)             ║
║  Artifact = deployable software (file icon with dog-ear)             ║
║  Comm Path = link between nodes (labeled with <<protocol>>)          ║
║  Dependency = dashed arrow (artifact to artifact or node)            ║
║  Type-level: generic type name  |  Instance-level: name : Type       ║
╠══════════════════════════════════════════════════════════════════════╣
║ NETWORK MODEL vs DEPLOYMENT DIAGRAM:                                  ║
║  Network Model = geographic scope + communication complexity         ║
║  Deployment Diagram = where software artifacts run on hardware        ║
║  (Deployment notation CAN be used for network models)                ║
╠══════════════════════════════════════════════════════════════════════╣
║ KEY TRAPS:                                                            ║
║  ✗ Thin client ≠ handles application logic (only presentation)       ║
║  ✗ Server-based ≠ most scalable (it's the LEAST scalable)           ║
║  ✗ Client-based ≠ cheapest infrastructure (C-S is cheaper)          ║
║  ✗ Thick client ≠ more secure than thin client                       ║
║  ✗ N-tier ≠ three-tier (N-tier splits app logic across >1 server)   ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

## Tags
`#systems-design` `#NFR` `#architecture` `#exam-prep` `#UML` `#deployment-diagram` `#cloud-computing` `#client-server`

---

*Source: Dennis, Wixom & Tegarden — Systems Analysis and Design with UML, 5th Edition*
