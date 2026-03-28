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

## The role

- The role is about vision, communication, and risk management
- Bridges the gap between high-level business goals and low-level technical implementation

| Category         | Responsibility                                                              |
| ---------------- | --------------------------------------------------------------------------- |
| technical vision | define the "north start" for: tech stack, frameworks, and patterns          |
| risk mitigation  | identifying "hidden" dangers early (e.g, security gaps, scaling limits)     |
| standardization  | ensuring all teams follow the same patterns, so the system remains cohesive |
| bridge building  | translating business requirements into technical specs for devs             |

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
