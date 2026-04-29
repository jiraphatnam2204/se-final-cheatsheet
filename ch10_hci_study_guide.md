# Chapter 10: Human–Computer Interaction Layer Design
## Exam Study Guide

---

## 1. Core Concepts

### Human–Computer Interaction (HCI) Layer
The software layer responsible for managing all interactions between human users and the system; distinct from **system interfaces**, which are machine-to-machine.

### UI Design Principles — Mnemonic: **LCA UCM**
Six foundational principles governing all interface design decisions:
1. Layout
2. Content Awareness
3. Aesthetics
4. User Experience
5. Consistency
6. Minimal User Effort

---

### Layout
The structured arrangement of screen areas, each serving a consistent, self-contained purpose:
- **Top area** → Navigation and commands
- **Middle area** → Information input or output (Reports & Forms)
- **Bottom area** → Status information

Areas should follow the natural reading flow of the target user population (Western users: left-to-right, top-to-bottom).

---

### Content Awareness
The degree to which a user understands what information is currently displayed and where they are within the system. Achieved through:
- Titles on **all** interfaces
- Field labels within each area
- Menus showing current location and navigation path
- Date and version numbers on forms and reports

---

### Aesthetics
The visual quality of an interface achieved through whitespace, color, and typography.

**Information Density by User Expertise:**

| User Type | Preferred Density | Screen Occupation |
|---|---|---|
| Novice / Infrequent | Lower density | < 50% |
| Expert / Frequent | Higher density | > 50% |

**Typography Rules:**
- Serif fonts → most readable for **printed reports**
- Sans-serif fonts → most readable for **computer screens** and headings in printed reports
- Avoid italics, underlining, and ALL CAPS (except titles)
- Use same font and approximately same size throughout

**Color Rules:**
- Use color sparingly — only to strengthen the message
- Most readable: **black on white**
- Least readable: blue on red
- Be aware of cultural color associations

---

### User Experience (UX)
Encompasses two distinct, sometimes conflicting dimensions:

| Dimension | Target User | Context |
|---|---|---|
| **Ease of learning** | Novices, infrequent users | Large user populations, consumer apps |
| **Ease of use** | Experts, frequent users | Specialized/professional systems |

- **Complementary:** Both lead to the same design decisions
- **Conflicting:** Designer must choose which group to prioritize
- Solution: Provide **multiple interfaces** (e.g., GUI ribbon + keyboard shortcuts)

---

### Consistency
The property whereby all parts of a system behave uniformly, allowing users to transfer learned behaviors from one part to another.

**Key areas of consistency:**
- Navigation controls
- Terminology — use the same descriptors on forms and reports

---

### Minimal User Effort
The design goal of minimizing task completion effort.

> **Three-Click Rule:** Users should be able to go from the main menu to their target information in no more than **three mouse clicks**.

Additional principles:
- Users should not have to think about how to navigate
- Number of clicks should relate to task complexity
- Minimize the number of words on the screen

---

### Windows Navigation Diagram (WND)
A UML-based artifact analogous to a **behavioral state machine** applied to the user interface.

- **Boxes** = UI components (window, form, report, button)
- **Arrows** = transitions between components
  - Single arrow → no return to calling state
  - Double arrow → return to calling state required
- **Stereotypes `<<>>`** → indicate interface type (e.g., `<<Window>>`, `<<Form>>`, `<<Report>>`)

---

### Use Scenario
A narrative description of **one specific path** through an essential use case, documenting the steps a user takes to accomplish a task. Used as the starting point for UI design. Document the most common cases so designs optimize for typical usage.

---

### Interface Standards
System-wide design conventions covering five elements:

| Element | Definition | Example |
|---|---|---|
| **Metaphor** | Real-world concept mapped to a UI concept | Shopping cart |
| **Objects** | Names of domain entities in the UI | CD, Artist, Book |
| **Actions** | Names of commands/verbs | Buy vs. Purchase; Search vs. Find |
| **Icons** | Visual symbols on buttons/forms/reports | Diskette = Save |
| **Templates** | Standard layout applied to all screens | Header/nav/content/status zones |

---

### Interface Prototyping
Mock-up techniques in increasing order of fidelity:

| Type | Description | Interactive Evaluation? |
|---|---|---|
| **Storyboard** | Hand-drawn pictures of screens | No |
| **Windows Layout Diagram** | Computer-generated storyboard | No |
| **HTML Prototype** | Linked web pages | Yes |
| **Language Prototype** | Built in target language, no real functionality | Yes (Formal Usability) |

---

### Navigation Design
The UI component enabling users to move through the system and receive status feedback.

**Basic Principles:**
- Prevent users from making mistakes
- Simplify recovery when mistakes occur
- Use a consistent grammar order (e.g., object→action: File ► Open)

---

### Input Validation
Pre-entry checks performed in order of increasing cost:

1. **Completeness** — all required fields present
2. **Format** — correct data type and structure (e.g., MM/DD/YYYY)
3. **Range** — numeric values within min/max bounds
4. **Check digit** — mathematical self-verification of numeric codes (e.g., SSN)
5. **Consistency** — logical relationships between fields (e.g., birth year < marriage year)
6. **Database check** — verification against stored records *(most expensive → performed last)*

---

### Output Design
Design of system-generated reports.

**Basic Principles:**
- Understand report usage and frequency (detail vs. summary; real-time vs. batch)
- Provide only what is needed; place most important information at the top
- Minimize bias in graphical displays

---

### Real Use Case
An implementation-specific use case that describes system interactions using actual UI element names (e.g., "clicks the Add Client Link"). Derived by evolving essential use cases after interface design. Multi-platform systems require one real use case **per platform**.

---

## 2. Mechanisms & How Things Work

### UI Design Process (Iterative Cycle)

```
         Use Scenario Development
                  ↓
         Interface Structure Design (WND)
                  ↓
         Interface Standards Design
                  ↓
         Interface Design Prototyping
                  ↓
         Interface Evaluation
                  ↓
    (feeds back to Use Scenario Development)
```

**Step 1 — Use Scenario Development**
Select common paths through essential use cases. Write as simple narratives. This drives design decisions toward the most frequent usage patterns.

**Step 2 — Interface Structure Design**
Construct WNDs. Identify all screens, forms, and reports. Define transition types (single vs. double arrows). Assign stereotypes.

**Step 3 — Interface Standards Design**
Define metaphors, object names, action names, icons, and layout templates. Enforces system-wide consistency before any screen is built.

**Step 4 — Interface Design Prototyping**
Build mock-ups in increasing fidelity. Goal: externalize and validate the design *before* construction begins, reducing costly rework.

**Step 5 — Interface Evaluation**
Gather feedback from users on the prototype. Multiple cycles are expected before finalizing.

---

### Input Validation Sequence (Order Matters)

```
Completeness → Format → Range → Check Digit → Consistency → Database
     ↑                                                           ↑
  cheapest                                                  most expensive
  (always first)                                            (always last)
```

---

### Error Message Design (Correct Order)

```
1. State the PROBLEM first
2. Provide SPECIFIC DETAIL about what is wrong
3. Suggest a CORRECTIVE ACTION
4. Use POLITE language (no blame)
5. Include a MESSAGE NUMBER (for help desk reference)
```

> ❌ Wrong order: "Enter correct phone number. Phone number is invalid."
> ✅ Correct order: "Phone number is invalid. Enter correct phone number."

---

### Delay Message Rules
- **Trigger threshold:** Operation takes **more than 7 seconds**
- Must allow user to **cancel** the operation
- Should indicate **estimated duration**

---

## 3. Comparisons & Trade-offs

### Ease of Learning vs. Ease of Use

| Dimension | Target User | Context | Example |
|---|---|---|---|
| Ease of Learning | Novices, infrequent users | Large user populations | GUI file explorer |
| Ease of Use | Experts, frequent users | Specialized systems | Command-line interface |
| Relationship | Can be complementary | Can be conflicting | Word has GUI ribbon AND keyboard shortcuts |

---

### Interface Evaluation Methods

| Method | Who Evaluates | How | Prototype Required |
|---|---|---|---|
| **Heuristic** | Experienced UI designers | Formal checklist vs. known principles | Any |
| **Walkthrough** | Design team + users | Team presents and explains prototype | Any |
| **Interactive** | Users + team member (one-on-one) | User works through real use scenarios | HTML or Language only |
| **Formal Usability Testing** | Users alone in lab | Video/software records behavior | Language prototype only |

---

### Menu Types

| Menu Type | When to Use | Key Rule |
|---|---|---|
| **Menu bar** | Main system menu | Items always ONE WORD; lead to other menus, never directly perform actions |
| **Drop-down** | Second-level from menu bar | Avoid abbreviations; items can be multi-word |
| **Pop-up** | Expert shortcuts | Must duplicate functionality elsewhere; novices miss them |
| **Tab menu** | Multiple related settings | Avoid more than one row of tabs |
| **Tool bar** | Expert shortcuts | All buttons same size; must have tooltips |
| **Image map** | Only when graphic adds meaning | Must convey which areas are clickable |

---

### Message Types

| Type | When to Use | Key Rule |
|---|---|---|
| **Error** | User did something not permitted | Always explain reason + suggest corrective action |
| **Confirmation** | User selects potentially dangerous action | Explain cause + suggest possible action; include options beyond OK/Cancel |
| **Acknowledgment** | System completed a task | Use **seldom or never** — updated screen state is sufficient |
| **Delay** | Activity takes > 7 seconds | Allow cancel; indicate estimated duration |
| **Help** | In all systems | Context-sensitive; organized by table of contents or keyword search |

---

### Output Report Types

| Type | Use When | Key Property |
|---|---|---|
| **Detail report** | Full info on specific items needed | Read cover-to-cover; response to a query |
| **Summary report** | Brief comparison across many items | Sort order is critical |
| **Turnaround document** | Customer must return output as input | Serves as both output AND input (e.g., credit card bill + payment stub) |
| **Graphs** | Comparison or trend visualization | Avoid 3D; bar > pie for comparison; start Y-axis at 0 |

---

### Graph Bias: Biased vs. Unbiased

| Biased Form | Problem | Correct Form |
|---|---|---|
| Y-axis not starting at 0 | Exaggerates differences between bars | Start Y-axis at 0 |
| 3D pie chart | Front slices appear visually larger | Use 2D pie chart |
| 3D bar chart | Depth distorts value comparison | Use 2D bar chart |

---

### Navigation Control Types

| Category | Type | Best For |
|---|---|---|
| Hardware | Keyboard, mouse, touch screen, microphone | Physical interaction |
| Software: Language | Command language (e.g., UNIX, SQL) | Expert users — high efficiency, high learning curve |
| Software: Language | Natural language (e.g., Google Search) | Broad populations |
| Software: Menu | Various menu types | Discoverability, novice-friendly |
| Software | Direct manipulation (drag-and-drop, resize) | Intuitive spatial tasks |
| Software | Voice recognition | Hands-free / accessibility contexts |

---

### Input Control Types

| Control | Use When |
|---|---|
| Text box | Free alphanumeric input |
| Number box | Numeric input with auto-formatting |
| Password box | Hidden input; no cut/copy |
| Check box | Multiple items can be selected simultaneously |
| Radio button | Items are mutually exclusive |
| List box / Drop-down list | Present a fixed set of choices |
| Slider | Value along a continuous scale |
| Combo box | Select from list OR type custom value |

---

## 4. Common Misconceptions & Traps

> ⚠️ These are high-risk wrong-answer choices in multiple-choice exams.

---

**Trap 1: Essential use case = Real use case**
These are **different artifacts**. Essential use cases are platform-independent (analysis phase). Real use cases are UI-specific and reference actual interface elements (e.g., "clicks the Add Client Link"). Multi-platform apps need one real use case per platform.

---

**Trap 2: The three-click rule is an absolute law**
It is a **guideline**, not a strict rule. The number of clicks should relate to task complexity and be unambiguous. The true goal is minimizing user effort.

---

**Trap 3: Acknowledgment messages should always be shown**
The material explicitly states acknowledgment messages should be used **"seldom or never"** because expert users find unnecessary confirmations annoying. The best acknowledgment is an updated screen (e.g., the new item appears in the list).

---

**Trap 4: Pop-up menus are the primary navigation for all users**
Pop-up menus are designed for **experienced users only**. They are frequently overlooked by novices, so any functionality in a pop-up **must be duplicated** in a more visible menu.

---

**Trap 5: Ease of learning and ease of use always conflict**
They are sometimes **complementary** (leading to the same design decisions) and sometimes conflicting. Never assume they always oppose each other.

---

**Trap 6: Database validation should be performed first**
Database checks are the most computationally expensive and are always performed **last**, after all simpler checks have passed.

---

**Trap 7: Interactive evaluation works with storyboards**
Interactive evaluation requires a working prototype — **HTML prototype minimum**. It cannot be used with storyboards or windows layout diagrams.

---

**Trap 8: Serif fonts are best for screen reading**
Serif fonts are most readable for **printed reports**. **Sans-serif fonts** are most readable for computer screens and headings in printed reports.

---

**Trap 9: Confirmation messages should be used frequently**
Confirmation messages are used **only when users select potentially dangerous actions** (like deleting a file). Overuse degrades UX.

---

**Trap 10: The WND is the same as a use case diagram**
A WND is analogous to a **behavioral state machine**, not a use case diagram. It shows transitions between UI components, not actor-system relationships.

---

**Trap 11: Error messages should state the fix first**
Stating the corrective action before the problem description "can be confusing." The **problem must be stated first**, followed by detail, then the corrective action.

---

## 5. High-Probability Exam Questions

---

**Q1.** A UI designer is creating an interface for air traffic controllers who use the system continuously for 8-hour shifts. Which design priority is most appropriate?

- A) Ease of learning, with a heavily menu-driven interface
- B) Ease of use, with keyboard shortcuts and dense information display
- C) Aesthetics, with extensive whitespace and minimal information density
- D) Consistency only, with no accommodation for expert shortcuts

**✅ Correct Answer: B**

> **Explanation:** Frequent, expert users benefit most from ease of use — efficient task completion once the system is learned. A) prioritizes novices. C) low density suits novices, not experts. D) ignores the legitimate performance need for efficiency tools.

---

**Q2.** Which of the following correctly describes the relationship between a Windows Navigation Diagram (WND) and a behavioral state machine?

- A) A WND is identical to a behavioral state machine with no differences
- B) A WND is analogous to a behavioral state machine, applied specifically to the user interface
- C) A WND replaces the use case diagram in the design phase
- D) A WND describes machine-to-machine interfaces, not user interfaces

**✅ Correct Answer: B**

> **Explanation:** The slides explicitly state WND is "similar to a behavioral state machine for a user interface." A) overstates the equivalence. C) confuses artifact types. D) is the definition of a system interface, not a WND.

---

**Q3.** In input validation, which check must always be performed last, and why?

- A) Format check, because it requires parsing the data structure
- B) Consistency check, because it compares multiple fields
- C) Database check, because it is the most computationally expensive
- D) Range check, because numeric validation is complex

**✅ Correct Answer: C**

> **Explanation:** Database checks require querying external storage and are performed only after all cheaper checks pass. A), B), and D) are all less expensive than a database query and are performed earlier in the sequence.

---

**Q4.** A product manager argues that all error messages should lead with the corrective action to save users time. What does UI design theory say about this?

- A) This is correct — users only want to know what to do, not what went wrong
- B) This is incorrect — stating the corrective action before the problem is confusing; the problem should be stated first
- C) Both orderings are equivalent and the choice is purely stylistic
- D) Error messages should contain only a message number and no text explanation

**✅ Correct Answer: B**

> **Explanation:** The slides explicitly state that stating the corrective action before the problem explanation "can be confusing." The correct order is: problem → detail → corrective action. D) is partially true — message numbers supplement text for help desk purposes; they do not replace explanations.

---

**Q5.** Which type of prototype is required to conduct formal usability testing?

- A) Storyboard
- B) Windows layout diagram
- C) HTML prototype
- D) Language prototype

**✅ Correct Answer: D**

> **Explanation:** Formal usability testing is conducted in labs using language prototypes where users work without assistance. Storyboards and WLDs cannot support interactive evaluation at all. HTML prototypes support interactive evaluation, but formal usability testing specifically requires a language prototype.

---

**Q6.** An interface displays minimal information with large amounts of white space and simple labels. According to UI design principles, this interface is most appropriate for:

- A) Expert users in a specialized financial trading system
- B) Infrequent or novice users in a system with a large user population
- C) Any user population equally, since minimalism is universally optimal
- D) Systems where performance requirements prohibit rendering complex layouts

**✅ Correct Answer: B**

> **Explanation:** Low information density (<50% occupied) is preferred by novice and infrequent users. A) Expert users prefer higher density. C) is false — density preference varies by expertise level. D) conflates performance requirements with aesthetic/density principles.

---

**Q7.** What distinguishes a turnaround document from other output report types?

- A) It is always printed and never displayed electronically
- B) It contains only summary-level information for management review
- C) It serves as both an output from the system and an input when returned by the user
- D) It is generated automatically on a batch schedule without any user query

**✅ Correct Answer: C**

> **Explanation:** A turnaround document "turns around" — it is output that becomes input when the customer fills it in and returns it (e.g., a utility bill with a payment stub). A) is not a specified constraint. B) describes a summary report. D) describes scheduled batch processing, not the defining characteristic of turnaround documents.

---

**Q8.** According to UI design principles, why should pop-up menu functionality always be duplicated in another menu type?

- A) Pop-up menus consume too much screen real estate
- B) Pop-up menus are often missed by novice users who may not discover them
- C) Pop-up menus violate the three-click rule
- D) Pop-up menus cannot display icons, reducing content awareness

**✅ Correct Answer: B**

> **Explanation:** The slides explicitly state pop-up menus "are often overlooked by novice users, so usually they should duplicate functionality provided in other menus." A), C), and D) are not stated constraints relating to pop-up menus in the source material.

---

**Q9.** A designer places "Save File" as a two-word item in the menu bar. Which UI design guideline does this violate?

- A) Content awareness — field labels must be single words
- B) Menu bar design — items should always be one word, never two
- C) Consistency — the same word must appear in all menus across the system
- D) Minimal user effort — multi-word items require more reading time

**✅ Correct Answer: B**

> **Explanation:** The slides state explicitly that menu bar items "are always one word, never two." Multi-word items belong in drop-down menus, not the menu bar. A), C), and D) are not the specific rule being violated here.

---

**Q10.** Which of the following correctly identifies all five elements of interface standards?

- A) Programming languages, frameworks, APIs, databases, and deployment environments
- B) Hardware devices, screen resolutions, operating systems, browsers, and network protocols
- C) Metaphors, interface objects, interface actions, icons, and templates
- D) WND structure, state transitions, stereotypes, use scenarios, and real use cases

**✅ Correct Answer: C**

> **Explanation:** Interface standards encompass exactly five elements: metaphors (real-world concepts), objects (domain entity names), actions (command names), icons (visual symbols), and templates (standard layout). A) describes the technical architecture stack. B) describes operational/nonfunctional requirements. D) describes navigation design artifacts.

---

## 6. One-Page Cheat Sheet Summary

---

### UI Design Principles — LCA UCM

| Principle | Key Rule |
|---|---|
| **Layout** | Top=nav, Middle=content/forms, Bottom=status; match user reading flow |
| **Content Awareness** | Titles on all screens; menus show current location; date+version on reports |
| **Aesthetics** | Minimalist; whitespace=separation; novice <50%; expert >50%; sans-serif for screens; serif for print body |
| **User Experience** | Ease of *learning* (novices) vs. ease of *use* (experts) — can complement OR conflict |
| **Consistency** | Same navigation + same terminology everywhere → enables prediction |
| **Minimal User Effort** | ≤ 3 clicks from main menu; minimize words on screen |

---

### UI Design Process (Iterative)

```
Use Scenario → Structure (WND) → Standards → Prototyping → Evaluation → repeat
```

---

### WND Key Facts
- Analogous to **behavioral state machine** for UI
- Single arrow = no return | Double arrow = return required
- Boxes = components | Arrows = transitions | `<<stereotypes>>` = interface type

---

### Interface Standards = 5 Elements
**Metaphor | Objects | Actions | Icons | Templates**

---

### Prototyping Fidelity (low → high)
```
Storyboard → Windows Layout Diagram → HTML Prototype → Language Prototype
(no interactive eval)                  (interactive eval)   (formal usability)
```

---

### Evaluation Methods
| Method | Needs |
|---|---|
| Heuristic | Expert + checklist |
| Walkthrough | Any prototype |
| Interactive | HTML prototype minimum |
| Formal Usability | Language prototype + lab |

---

### Navigation Controls
- Hardware: keyboard, mouse, touch, microphone
- Software: Command language | Natural language | Menus | Direct manipulation | Voice

### Menu Bar Rule
> **ONE WORD ONLY** — leads to other menus, never directly performs actions

### Pop-up Rule
> Expert shortcuts ONLY — must be **duplicated** in other menus (novices miss them)

---

### Message Types — Quick Reference

| Message | Trigger | Use Frequency |
|---|---|---|
| Error | Not permitted action | Always — explain + fix |
| Confirmation | Dangerous action | When needed |
| Acknowledgment | Task completed | **Seldom/Never** |
| Delay | > **7 seconds** | When needed — allow cancel + show duration |
| Help | Anytime | All systems |

### Error Message Order
```
Problem → Specific Detail → Corrective Action → Polite Tone → Message Number
```

---

### Input Validation Order (cheapest → most expensive)
```
Completeness → Format → Range → Check Digit → Consistency → DATABASE (last)
```

---

### Input Control Selection
- Multiple selections → **Check box**
- Mutually exclusive → **Radio button**
- Fixed list → **List box / Drop-down**
- Continuous scale → **Slider**
- List OR custom → **Combo box**

---

### Output Report Types
| Type | Key Feature |
|---|---|
| Detail | Full info on specific items |
| Summary | Compare many items; **sort order critical** |
| Turnaround | Both **output AND input** (customer returns it) |
| Graph | 2D > 3D; bar for comparison; line for trends; **Y-axis must start at 0** |

---

### Essential vs. Real Use Cases
- **Essential** = platform-independent (analysis artifact)
- **Real** = names actual buttons/forms/links (design artifact); **one per platform**

---

### Nonfunctional Requirements → HCI Impact
| NFR | Impact |
|---|---|
| Operational | Platform/technology constraints on available UI controls |
| Performance | Mobile/web add latency; affects feedback design |
| Security | Login controls, session management, encryption |
| Political/Cultural | Date formats, currencies, color meanings, reading direction |

---

### Critical Numbers
| Rule | Value |
|---|---|
| Three-click rule | ≤ **3 clicks** from main menu |
| Delay message threshold | > **7 seconds** |
| Novice information density | < **50%** |
| Expert information density | > **50%** |
| Acknowledgment usage | **Seldom / Never** |
| Menu bar item word count | **1 word maximum** |

---

*Study guide based on: Dennis, Wixom & Tegarden — Systems Analysis and Design with UML, 5th Edition, Chapter 10.*
