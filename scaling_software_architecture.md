# Scaling software architecture

- A common discussion scenario is an application that started having the need to deal with a increased load of users and this caused the application to be slow or have downtime during peak time

- The objective is not only to write code, but know how to deal with an application in the real-world

## Initial architecture

- Client
  - Web browser
  - Mobile app

- client : DNS
  - api.mysite.com [client -> dns]
  - 12.123.12.123 [dns -> client]

- client : server (the db is deployed here as well)
  - 12.123.12.123 [client -> server]
  - JSON response [server -> client]

- We'll present the solutions below, then "add" on top of each other, not necessarily replace one another. We go to the next level according to the need of that given application. On each solution, we'll see what it does, what it solves and issues that it introduces

### Solution 1: Separate the application server from the db server

- A server for the API
- A server for the DB

- Server -> db [read/write/update]
- Db -> server [return data]

#### Issues with this implementation

- Single point of failure: either from api server or from db
- Solution: scalability
  - Vertical: not much to talk about, you just increase computing power
    - CPU, RAM, Disk, etc
    - It's not bad, however, if we want to achieve 1M users, it's not enough
  - Horizontal: where the magic is. Adding more servers
    - Load balancer

- Failover: if a server fails, we've got one to substitute it
  - Having redudance

## Solution 2: Horizontal scaling

- Needs load balancer: it's the tool that allows switching servers
  - server a
  - server b: a replica of server a

```
        load balancer // receives the requests from the frontend
        /           \ // private ip: not exposed to the internet
    server a        server b
```

### Load balancer

- It's the load balancer that receives the frontend requests and distributes to the server
- With a load balancer, we don't expose our servers to the internet any longer, instead, we expose the load balancer
- What's accessible through the internet is the load balancer, not the servers anymore

#### Example with nginx

- Nginx: a massive switchboard operator

```conf
http {
    # define the group of servers to balance across
    upstream my_app_servers {
        # least_conn; # optional: sends traffic to the server with fewest active connections
        server 127.0.0.1:3001;
        server 127.0.0.1:3002;
        server 127.0.0.1:3003;
        # ...
    }

    # when a request comes on this port/domain, follow the instructions
    server {
        listen 80;

        location / {
            # pass the trafic to the upstream group defined above
            proxy_pass http://my_app_servers; // same name as above

            # standard headers so the app knows the original user info
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
    }

}
```

### Issues with this implementation

- If we scale the api servers horizontally, we have replicas for our application servers, however, our db server is is one server. Then, we still have the single point of failure issue
- The issue then is that we'll have multiple api server instances connecting to the same db
- If something happens with this single db server, we're screwed

## Solution 3: Database replication

### Master / slave

- The web servers communicate with different db servers
- Web servers -> write to -> master db
- Web servers -> read from -> slave db1, db2...

- Writing [create/update/delete] is redirected to the master db
- Reading [select] happens in the slave dbs
- In most applications, the volume of reading is much bigger than writing

## To sum up: Scaling an application

1. separating application log [api] from db
2. horizontal scaling
   - having replicas
   - these replicas are accessed through a load balancer
   - resolves:
   - single point of failure
     - we have redundance | we add failover
