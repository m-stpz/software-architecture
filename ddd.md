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

```
// model 1
Cargo -> Stop
        unload()
        load()
```

> The first acceptable good idea is usually the last one. This isn't good.

Example: When buying a car, you might have some requirements (color, model, ac, etc)

- When you go to the dealership, even if the dealer shows you the car that attends the exact requirements, you still:
  - test-drive
  - look for other alternatives in and out of the shop

- What's the difference between buying a car and designing a piece of sotware?

1. Possibilities for sofware are broader and less understood
2. Most of the time software development is more expensive than buying a car

Given this, it'd make sense to spend more effort on designing a software than buying a car. What we don't necessarily do

```
// model 2
Cargo -> Stop
        unload()
        load()

Cargo -> Leg
        unload()
        load()
```

> Usually finding 3 design decisions is a great sweet spot
> Paying attention not just to the meaning, but to the words themselves, it's a very effective way to model software
