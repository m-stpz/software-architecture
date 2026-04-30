# Scaling software architecture

- A common discussion scenario is an application that started having the need to deal with a increased load of users and this caused the application to be slow or have downtime during peak time

- The objective is not only to write code, but know how to deal with an application in the real-world

## Initial architecturel

- Client
  - Web browser
  - Mobile app

- client : DNS
  - api.mysite.com [client -> dns]
  - 12.123.12.123 [dns -> client]

- client : server (the db is deployed here as well)
  - 12.123.12.123 [client -> server]
  - JSON response [server -> client]

## Improvements

### 1. Separate the application server from the db server

- A server for the API
- A server for the DB

- Server -> db [read/write/update]
- Db -> server [return data]
