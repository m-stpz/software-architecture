# Domain-Driven Design

- When we design software, we can have two perspectives

1. Software engineer perspective
   - how to make software clean/changeable
   - are there side effects?
   - is the system coupled?
2. Domain perspective
   - someone who's an expert in the field, but not on the software

> Creative collaboration of software experts and domain experts

Software + Domain:

- what concepts are we missing?
  - language is very important in DDD
    - sometimes the specific domain explanation of the functionality doesn't match the technical, software implementation of it
    - DDD tries to solve this

> The first acceptable good idea is usually the last one. This isn't good.

Example: When buying a car, you might have some requirements (color, model, ac, etc)

- When you go to the dealership, even if the dealer shows you the car that attends the exact requirements, you still:
  - test-drive
  - look for other alternatives in and out of the shop

- What's the difference between buying a car and designing a piece of sotware?

1. Possibilities for sofware are broader and less understood
2. Most of the time software development is more expensive than buying a car

Given this, it'd make sense to spend more effort on designing a software than buying a car. What we don't necessarily do

> Usually finding 3 design decisions is a great sweet spot
> Paying attention not just to the meaning, but to the words themselves, it's a very effective way to model software

```
// model 1
Cargo -> Stop
        unload()
        load()

// model 2
Cargo -> Leg
        unload()
        load()

// model 3
Cargo -> Itinerary -> Stop
                    unload()
                    load()
```

- We want quantity, not depth of analysis. Get initially broad.
  - You don't know if you have explored the space of possible solutions if you never come up with a bad idea

## Steps

### 1. Generate variation: create

- initially: quantity > quality
- generate ideas, but also know when to go to action
- multiple, small sessions
  - what do you document from this?
    - diagram
    - main words

### 2. Choosing the model: select | edit

- project manager are useful here
- what is a model?
  - UML diagram
  - concept behind the diagram
    - map:
      - a model of the world
      - as the mercator projection, a map/model might have distortions
        - even though the mercator projection is rather distorted, it's useful for one thing: **preserves the direction between two points**
          - if you make a map based on this projection, you can draw a line between two points and the angle of the line corresponds to a compass direction
        - mercator project (this model): makes...
          - direction simpler
          - comparing size difficult
    - behind a model, you've got:
      - abstraction
        - earth is a spehere
        - spherical coordinates: latitude/longitude
      - data selection
        - points of interest: coastlines, rivers, ports
      - established conventions/formalism
      - assertion

- In software, we:
  - take a dataset that is imperfect and incomplete
  - we apply transformation to that data that allows us to make meaningful decisions

## Definitions

1. domain: sphere of knowledge/activity
2. model: system of abstractions representing selected aspects of a domain
   - based on knowledge + assumptions about the domain
   - some models are good for some things, other for other things
   - models need a narrow focus
     - completeness leads us out of the track
     - multiple models for multiple problems within a larger domain
     - model should be focused on a specific, difficult, important problem

```
                complex domain

model 1             model 2     model 3
- abstraction 1         ...         ...
- abstraction 2
```

> Realism is a distraction. It doesn't give us models that we can use

- Instead of asking, "which model is better?", we should ask, "which model is more useful?"
  - but, then, useful to what?
    - we need scenarios!

3. Ubiquitous language: language structured around the domain model
   - used by all team to connect the activities of the team with the software

```
team --> language --> software
            |
    structured around domain -> both software and business
```

> Focus on how talking about the problem is fundamental to finding a good solution

## Concrete scenarios

- Specific stories about the business
- Some model will be more useful to given problem
  - simpler
  - clearer
  - less steps
