# Architecting large software projects

https://www.youtube.com/watch?v=sSpULGNHyoI

## What we want to optimize for

- Dependability: last forever and doesn't break
- Extendability: you want to be able to expand it
- Team scalability: few people working on sections of the problem
  - you want a lot of small projects instead of one big project
- Development velocity: get shit done
  - by default, the longer you work on it, the slower it gets
  - you want to keep the velocity constant

> Reduce risk!

> Many small chances of failure == a big chance of failure

- A lot of little things that might be easy to fix, if you have enough of them and they happen frequently enough, you have a big problem

## Some takes

- It's faster to write five lines of code today, than to write one today, and have to edit it in the future
  - Be very explicit
  - Leave code when it's done
- Modulerize your software
  - they should be block boxes
  - what they do should be exposed through APIs/protocols
  - interface for talking to it
  - you shouldn't know what's behind the scenes
