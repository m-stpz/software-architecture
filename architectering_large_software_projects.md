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
- Learn how to think forward
- Avoid implementing good-enough-for-now
  - If you do, hide it behind solid APIs
  - Don't ever implement good-enough-for-now APIs

### Modularize your software

- they should be block boxes
- what they do should be exposed through APIs/protocols
- interface for talking to it
- you shouldn't know what's behind the scenes
- if a module doesn't work, but it has a well-defined API, you can change it without changing the API

> A module can be exchanged without changing its interface

## Examples

### 1. Video editor

#### Modules

Platform layer

- Anything that you don't own [platform], you should not talk to it directly
- Wrap it
- Don't have a bunch of calls directly to code you don't own!
- Write a mininum test application
  - exercise all the API
  - Consolidate functionality
    - all buttons are the same [on controller/keyboard/mouse]
    - all pointers [mouse events and touch] are the same

Drawing layer

Text

UI Tookits
