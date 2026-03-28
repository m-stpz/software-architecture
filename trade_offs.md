# Trade-offs

- Trade-offs mean that **every architectural decision opmitimizes for something at the cost of something else**

- What you increase availability (horizontal scale), you also increase the area of surface for attacks (reduced security)
  - increased avalability = reduced security

> Good architects don't find the right answer, the find the right trade-off for the current context

## Core principles

### 1. Name both sides explicitly

- Never say, "we should use X"
- Say, "X gives us Y, but costs us Z"
- If you can't name the cost, you haven't thought it enough

### 2. CAP theorem

- In any distrubuted system, you can only have 2 out of 3: Consistency, Availability, Partition tolerance
  - Consistency: every read returns the most recent write
    - All nodes see the same data at the same time
    - No stale reads
  - Availability: every request gets a response (not an error), even if some nodes are down
    - System keeps serving, always
  - Partition tolerance: system keeps working even when network communication between nodes break (messages are dropped or delayed)

Important: network partitions will happen in any distributed system, so we must choose between C or A when things go wrong

CP: a bank -> when a partition occurs, refuse requests rather than serve stale data
AP: a shopping cart -> when a partition occurs, keep serving even if data is out of sync

#### Partition definition

- Partition: when 2 >= nodes can't communicate with each other
  - the network link between them is broken, slow, or dropping messages

### 3. Optimize for what changes, not what exists

### 4. Complexity is always a cost

### 5. Context invalidates best practices

### 6. Trade-offs compound

### 7. The reversibility test
