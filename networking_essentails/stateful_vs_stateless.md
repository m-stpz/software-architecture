# Stateful vs. Stateless

- State: any data that a system needs to remember about a user/process to function

## Stateless: Vending Machine / Cashier | Cares only about the current transaction

- Stateless system: doesn't store any information about the previous interaction
- Every request from the client must contain all the information necessary for the server to understand/perform it
- Scaling:
  - Easy. Any server can handle any request
  - Add more servers behind a load balancer

## Stateful: Private Lessons / Psychologist / Personal trainer | Remembers your progress

- Stateless system: keeps track of the state of the interaction
- Remembers who you are and what you did
- Scaling:
  - Difficult. Each server might have a "portion" of given state
    - If one server is down, the other might miss this info
    - Data needs to be synced between servers

| Feature      | Stateless                                     | Stateful                                       |
| ------------ | --------------------------------------------- | ---------------------------------------------- |
| Data storage | client sends all data with every request      | server stores data about the session           |
| Dependency   | requests are independent                      | requests depend on previous ones               |
| Scalability  | high (horizontal scaling is simple)           | low (requires "sticky sessions") or synced DBs |
| Complexity   | simpler server logic / heavier client payload | complex server logic to manage memory/state    |
| Examples     | REST APIs, HTTP, Serverless (lambda)          | WebSockets, FTP, Online Graming Servers        |

- In modern web dev, we aim for:
  - stateless servers
  - stateful dbs

- Server "fetches" state from a fast, external db (Redis, for instance)
  - fetches, processes, then forgets it
