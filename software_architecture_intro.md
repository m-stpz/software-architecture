# Software architecture in the real world: The basics

https://www.youtube.com/watch?v=iWX5-LDbRNs

video: 11:21

## Why software architecture?

- Informed decisions
- Maintanable systems
- Coding is about how a feature works, architecture is about why the system stays standing
- Managing complexity
- As a codebase grows, the "mental overhead" required to understand it increases exponentially
  - Architecture provides a map
  - Allows us to reason about a massive system by breaking it into smaller, decoupled modules
- Juniors solve tasks
- Seniors solve features
- Staff/Architects solve systems
  - High-level design docs (RFCs)
  - mentor others
  - ensure company isn't building technical dbt that will bankrupt the engineering team later

- What defines the value of a software?
  - Meaning, what are the characteristics that make a valuable software?

### What it does

1. Designing "non-functional" requirements

- While devs focus on features (what the app does), the architect focuses on the "-ilities"
  - scalability: can it handle increased traffic?
  - reliability: can it handle server fail?
  - maintainability: can a hire understand this in a week?
  - security: is the data protected at every layer?

2. Evaluating "build vs. buy": should we build it or use a 3rd-party solution?
3. Defining boundaries: how different portions talk to each other

- Rest or GraphQL?
- Monolith or microservices?

### How do they spend their time?

- 30% technical design & documentation: Writing RFCs and ADRs
- 25% meetings & stakeholder management: talking to product managers to understand future needs and talking with executives to explain why a "complete rewrite" might (not) be necessary
- 20% code review & mentoring: might not write much production code, but they review critical PRs to ensure the architecture is being followed
  - Act as a "level up" for senior developers
- 15% Prototyping (PoCs): builds a small "toy" version of the feature
- 10% firefighting

## The value of software

### Functional value: what it does for the user right now

- The features to achieve the what the business wants
  - what the business wants? [for more in_depth business needs, check `business_needs_questions`]
    - Important: each software has a "different" need according to the context it's inserted
  - achieving parameters in relation to quality
  - respecting restrictions

- if you're not Netflix, creating a software similar to netflix, likely is a bad idea
- In 2018, Netflix would attent 1M requests per second. If your software doesn't have this, why would you create something following netflix architecture?
  - Their architecture work like that due to the demands they have

### Architectural value: how easily it can change tomorrow

- In the book clean architecture, Robert C. Martin argues that the "architecture" (the ability to change) is more valuable than the "features" (the current functions)
  - If a program works perfectly, but it can't be changes, it becomes useless the moment the world changes

> The software responsing the requests within a given time span within a given percentage expresses its quality

- When taking a decision, you must know what's the cost you're paying
  - What are the consequences of my decision?

- Important: for a business it's important, "I need this feature on prod until this day" (window of opportunity)

> The life of a real software begins, when it enters production

- Software becomes more challeging to be changed as time passes by
- Software has value when it achieves business need at a specified quality time and also when it can be maintained/expanded without reducing this established quality
- Software engineering takes care of software development throughout time
  - software architecture is a discipline of software engineering, it's part of the effort to maintain the software through time
  - software architecture is fundamental so the software evolves/maintains its value throughout time

> We've long believed that when the rate of change inside an institution becomes slower than the change outside, the end is in sight. The only question is when. Jack Welch

- To change is to keep the business afloat
- When an organization loses the capacity to change as fast as its surrounding market, it rots

### Technical debt

- When we take "less" optimal solutions in order to achieve certain deadlines, however, debt generates interest
- If you don't know to explain why your suggestion solves a real problem, likely it doesn't solve any problem
  - Then, it's only your opinion
  - Propose solutions to real-problems, otherwise, keep quiet

### Lehman Laws

#### 1st Continuous change = keeping software relevant

- A software system must continuously change to keep it relevant
- Software that doesn't change is software that is irrelevant

#### 2nd Expanding complexity is bounded my explicit effort

- New functionality/features increase the software complexity, except when there are explicit effort to reduce this complexity

> What is legacy software? When a team's necessity to have technical debt is bigger than they available capacity to solve old debts

- My software needs to be set up in a way that introducing new changes is cheap

### Why software architecture is important?

1. Increase the value of a software by making it easier to adapt
2. Combating complexity, applying agility

- By applying good software architecture practices, we enable/facilitate its agility

### Architecture vs. Design

> Architecture is always about design, however, design isn't always architectural

- Distinction: significance and scope

#### Architecture: hard to change and impact everything else

Architecture = decisions that are hard to change & shape everything else

- Structural choices that constrain all the design below them
- Microservices vs. monolith
- Sync vs. event-driven communication
- Data storage strategy
- Defining system boundaries and how modules interact

- Every architectural decision is a design decision: you're choosing how to structure something

> For every problem, there's a single, elegant solution, which is completely mistaken

- There's no easy answer. You need to develop your repertoire
- The most skilled devs aren't the ones that necessarily provide all the answers, but make the correct questions

#### Design: local, easy to change, and don't ripple across the system

- Naming a variable
- Choosing a loop vs. a map
- Deciding whether a helper func is private or public
- One UI layout over another

|                               | Architectural                  | Non-architectural design        |
| ----------------------------- | ------------------------------ | ------------------------------- |
| Scope                         | system-wide                    | local: module/class             |
| Cost to change                | high: significant rework       | low: quick refactor             |
| Affect other teams/components | usually yes                    | usually no                      |
| Example                       | "we use REST between services" | "This function returns a tuple" |

- Software architecture deals with the design aspects that ensure:
  - achieval of objectives
  - restrictions respect
  - achieval of quality attributes throughout time
  - reducing risk and cost

- Selecting the technology isn't as architectural as thinking about the criteria we're using to select the technology

```
Start
  ⬇️
Orchestrate the elaboration of an architectural design proposal
  ⬇️
Evaluate the proposal         <-  Clarity what can be improved
  ⬇️                                     ⬆️ No
- Does it satisfy business demands?
  ⬇️ Yes                                 ⬆️ No
- Does it comply with constraints?
  ⬇️ Yes                                 ⬆️ No
- Does it meet the quality attributes?
  ⬇️ Yes                                 ⬆️ No
- Is there a cheaper or less risky option?
  ⬇️ No                                  ⬆️ Yes
Prepare implementation
  ⬇️
  End
```

> Organizations that develop software tend to produce systems that are a copy of the communication structures of these companies. Melvin Conway

- The way teams are structured influence on how software is structured

## What's the role of a software architect?

- The role is about vision, communication, and risk management
- Bridges the gap between high-level business goals and low-level technical implementation

| Category         | Responsibility                                                              |
| ---------------- | --------------------------------------------------------------------------- |
| technical vision | define the "north start" for: tech stack, frameworks, and patterns          |
| risk mitigation  | identifying "hidden" dangers early (e.g, security gaps, scaling limits)     |
| standardization  | ensuring all teams follow the same patterns, so the system remains cohesive |
| bridge building  | translating business requirements into technical specs for devs             |

- In some teams, the architect, creates the plan, but doesn't stay with the team. This creates more problems than solutions. Ideally, the architect is close to the development

- An architect is, before everything, an orchestrator
  - Talk with the team
    - Satisfies business demands?
    - Complies with constraints?
    - Meets quality attributes?
  - Beyond software skills, it's important people skill
- The architect ensures decisions are:
  - made
  - justified
  - communicated & repeated
  - remembered
  - respected

## Some career tips

- Decide fast
  - And keep on the path
- Right execution
  - It's important to develop repertoire
- Good selling
  - Selling your idea is always important

## Software architecture vs. System Design

### Software architecture

Focuses on the internal structure of a software system

- how code is organized into components, modules, and layers and how they interact
- it deals with:
  - design patterns (mvc, hexagonal, clean architecture)
  - module boundaries and dependencies
  - quality attributes (-ities: maintainability, testability, security, extensibility)
  - technology-agnostic structural decisions

#### Books

- Clean architecture: principles of component design and dependency management
- Fundamentals of software architecture: Broad survey of architecture styles and the architect's role
- Designing data-intensive applications: briges architecture and system design [arguably the single most important book]
- Patterns of enterprise application architecture: classic catalog of architecture patterns
- A philosophy of software design: deep vs shallow modules, complexity management
- Software architecture: the hard parts: tradeoff analysis for distributed architectures
- Domain-driven design: aligning architecture with business domains

### System design

Focuses on the infrastructure and distributed components that make the system work at scale

- it deals with:
  - load balancers, caches, dbs, message queues
  - horizontal scaling, replication, sharding
  - network protocols and API design

#### Books

- Desining data-intensive applications: again, it's that important
- System design interview: practical walkthroughs of real-world systems
- Building microservices: distributed system patterns and pitfalls

Architecture = how you structure your code
System design = how you structure your infra and distributed services
