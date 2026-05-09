# Network essentials

https://www.youtube.com/watch?v=SHkbPm1Wrno

## OSI Model

- Think of network as a layered cake

| layers | name         | description                                                                 | Example                           |
| ------ | ------------ | --------------------------------------------------------------------------- | --------------------------------- |
| l7 [*] | application  | Interface where user interacts with the network                             | HTTP, DNS, SMTP, FTP              |
| l6     | presentation | Handles data encryption, compression, and formatting                        | SSL/TLS, JPEG, GIF                |
| l5     | session      | Manages the "conversation" (opening/closing/restarting) between two devices | NetBIOS, RPC, Sockets             |
| l4 [*] | transport    | Handles the delivery and error-checking of data packets                     | TCP, UDP                          |
| l3 [*] | network      | Determines the best physical path for data to travel                        | IP, Routers, ICMP                 |
| l2     | data link    | Transfer data between connected nodes on the network                        | Ethernet, Mac addreses, switchers |
| l1     | physical     | Hardware, cables, electrical signals                                        | Fiber, Wifi                       |

![alt text](image.png)

- There's a lot of back and forth between layers

### Layer 3: Network Layer (Internet Protocol)

- IP: Giving "names" to nodes in the network and allow routing
  - IPv4: 4 bytes
    - normally used externally
  - IPv6: 16 bytes
    - more used internally

- They come in two flavors:
  - Private: protected from the world
    - Used in: microservices, internal config/hosts
    - If we're going to load balance them, we need to keep track of their existence
  - Public: known to the world
    - assigned by a "central body"
    - routers are aware of them
    - Used in: API gateways, load balancers [externally facing components of your design]

### Layer 4: Transport (Protocols TCP, UDP)

- TCP: default, reliable | snipper
  - slower, but safer
  - guarantee delivery + ordering
  - connection-oriented
  - higher latency
  - we care about individual packages
  - best for: most of content online
- UDP: faster, non-reliable | machine gun
  - faster, but less reliable
  - no delivery guarantee
  - we don't care that much about individual packages
  - where latency is the most important
  - not supported by browsers by default
  - best for: streaming, video conferencing, multiplayer games

### Layer 7: Application layer (HTTP - REST, GraphQL, gRPC)

#### HTTP

- Formatted requests and responses

Request

```json
GET /posts/1 HTTP/1.1 // method/verb
//headers
Host: thedestination.com
Accept: application/json
User-Agent: the device making the request
```

Response

```json
HTTP/1.1 200 OK // status code
// response headers
Date:
Content-Type:
Content-Length:

// response body
{
  "userId": 1,
  "title": "Some content"
  //...
}
```

#### REST Api

- The most common way to build APIs on top of HTTP
  - a way to organize API around verbs and URLs
  - thinking in terms of resource update
- We use the:
  - HTTP methods/verbs: to describe what we're doing
  - Resources: URLs associated with it
- The default in building APIs

```bash
# method + resource
GET /users/{id} -> User
  # returns a json with the information of the given user

POST /users -> User
# body of request
{
  "username":"example",
  "email":"johndoe@email.com",
}
```

- Some developers find this organization counterintuivie because they're used to thinking in terms of function-calls
  - operational thinking

#### GraphQL

- REST might need multiple calls to the server to load a given resource, e.g, a web page. This can present some overhead
- REST can cause over/underfetching depending on the context
- GraphQL basically allows the frontend to describe the shape of data it needs
  - The backend then decides how to it can grab that data
  - Instead of focusing on "resources" (REST), we describe exactly the shape I need for given context
- GraphQL is useful when:
  - frontend requirements change a lot
  - multiple backend/frontend teams
    - this way, the frontend becomes less dependent on an API implementation of the backend

- In system design interviews, usually REST is the best choice. Opt for GraphQL if:
  - change of requirements is mentioned explicitly
  - you want to design a system that explicitly supports this arbitrary queries

#### gRPC = Protobufs + Services

- Protobufs:
  - Declare an object/schema, and this object is transformed/serialized into binary representations

```js
// declaring protobuf, the schema of the request
message User {
  string id = 1;
  string name = 2;
}

// 40 bytes
{
  "id": "123",
  "name":"John Doe",
}

// 15 bytes = proto buffer representation
0A 03 32 33 12 08 ...
```

- Makes the serialization and deserialization of data simpler
- gRPC services can be up to 10x faster and more efficient than REST

gRPC service

```js

message GetUserRequest {
  string id = 1;
}

message GetUserResponse {
  User user = 1;
}

service UserService {
  rpc GetUser (GetUserRequest) return (GetUserResponse);
}
```

- gRPC is useful for:
  - operating microservices at scale
    - client-side load balancing
    - streaming
    - rudimentary authentication

- gRPC is how google builds services internally
  - gRPC is great for building internal services
  - on interviews, bring it up only if you have the need to clearly optimize for performance

##### Problems with gRPC

- clients don't support it natively
  - browsers don't support it
- operating with binary in between services is really efficient for servers, not for humans though
  - harder debugging

#### Server Sent Events (SSE)

- Previously we were talking mainly about requests/responses
  - vast majority of applications
- However, some applications we might want to push data to the user
  - notifications
  - sensitive updates
    - stock change
  - we could use polling, however:
    - data is stale until polling is called
    - sometimes there might not be updates, and we're polling

- SSE (Server Sent Events): it's basically HTTP, with formmating differences
  - SSE we include headers in the response (chunked encoding, timestamps)
  - SSE uses new lines for parsing

- It's built on top of HTTP, and as default of HTTP, the connection is closed after a while. So, SSE "reconnects" on this period
  - Responses are periodically disconnected and then re-enabled

- Allows longer running requests, however, it's unidirectional
  - Server is sending requests to the client continuously
  - What if we need it to be bidirectional?
    - clients send constantly and servers send constantly?
    - then, it enters web sockets

#### Web Sockets

- High frequency updates and bidirectional communication
- Powerful, but require a lot of infra
  - Before resourcing to web sockets, verify whether a polling or sse solution wouldn't solve it
- How do they work?
  - "Simulate" a gRPC connection, but making it accessible for browsers/other clients
  - offer bidirectional, low-latency streaming

```
wss:// /tickers

received:
{
  msgType:"tickerUpdate",
  ticker: "abc",
  valueInCents: 10,
}

sent:
{
  action: "subscribe",
  ticker: "abc"
}

{
  action: "unsubscribe",
  ticker: "abc"
}
```

- When designing it, we usually talk about API in terms of messages that we're sending
  - receive / send
- Usually in system design, we want to minimize statefulness, but websockets are inherently stateful

#### WebRTC

- Runs on UDP, not on TCP
- Used for: collaborative editors or audio/video connections between clients
- peer-to-peer connection
- on a design, you use if you're doing video/audio calling
  - or collaborative editors
  - CRDT (Conflict-free Replicate Data Types)

##### How it works

1. Clients connect to `signaling server` (centralized knowledge of connected clients)
2. Clients connection to a STUN server (allows clients to connect to each other)
3. Once the clients are connected, they can exchange data through UDP

### Summary OSI Model

- websockets and sse: streaming applications
- grpc: high-performance backend
- graphql: flexible-frontend
- Rest APIs: jack of all trades

## Scaling

- How can you design a system that works with a lot of users/data?
  - vertical: bigger
    - try vertical before horizontal
    - servers have terabytes of memory nowadays
  - horizontal: more servers
    - this is usually expected to talk about
    - load balancing
      - spreads the load
      - increases high availability

### Load balancing

They come in two flavors:

- Client-side
  - The client is aware of all the servers they can connect to
  - They query a registry with the existence of all servers
  - We use it when:
    - we don't have many clients (internal facing microservices)
      - gRPC supports it natively
    - many clients, but can tolerate update delays (DNS)
- Dedicated tool
  - external clients and need to be updated quickly

```
                        / server 1
client -> load balancer - server 2
                        \ server 3
              ^
    - decides which server to route to
```

- Load balancers perform health checks on servers connected to it
  - If the load balancer returns healthy (check can be shallow or deep), the load balancer will route traffic to it
  - If the load balancer returns unhealthy, the load balancer will stop sending traffic to it

#### Algos for load balancing

1. Round robin

- requests are passed to servers in sequential order
- Assign a weight to the servers based on their capacity
- A server with weigth 10 gets 5x more traffic than one with load 2

Cons:

- Doesn't take into account server load

2. Least connections

- load balancer keeps track of how many active connections each server has and sends a new request to the one with the fewest
- great for "long-lived" requests (streaming/heavy db queries)

Cons:

- Load balancer needs to be stateful (who's doing what) which uses more CPU/memory on the balancer

3. IP Hash

- Load balancer takes the IP address and runs a hash function to map it to a specific server
- The same user will usually hit the same server

Cons:

- If server goes down and all sessions are stored on that server for given users, they lose their session
- Also, if many users come from the same IP (e.g., a company) the server might get jammed

4. Least response time

- A more advanced version of least connection
- Picks server with: fewest active connections + least average response time
- Most efficient for user experience

Cons:

- Computationally expensive for a load balancer to track all those metrics in real-time

5. Random (and power of two choices)

- Balancer picks a/two server(s) at random. Then sends the request to the better of the two
- Avoids the scenario where the "least busy" server gets loads of requests from load balancers at the same time

Cons

- Can lead to uneven distributions at small samples

#### What levels do load balancers operate at?

- Layer 4 load balancer: operates at TCP level
  - High-performance
  - More simple
  - Web sockets and stateful connections interact well with a l4 load balancer

- Layer 7 load balancer: operates at HTTP level
  - More expensive
  - More "complete"
  - The default

## Regionalization

- A system that works across the globe
  - Physics must be taken into account
  - Light an travel at a max speed in a fiber optic cable
  - London -> NY (80ms of latency, no matter what I'm doing, simply by the laws of physics)

- Uber
  - a global system with global traffic
  - however, drivers and users are in the same "region" (if you're in your city, likely you won't be ordering an uber from another continent/country)

- Companies will normally have a data center in region of where they're operating
  - regional db

### Important principles

1.  collocating your data: the core of data and processing as close as possible
    - web server + db: need to talk quite often to send a response. This way, need to be close by
2.  services should be as close to the users as possible

## Handling failures/faults | timeouts, backoff, retries

- When clients are talking to servers, we likely want them to eventually timeout
  - timeout.duration > server.process_data() && timeout.duration < user.waiting_too_long
- Retry: if a request fail, try again after some time
  - This time shouldn't be specific (each 3-5s) since if different clients hit an error, they will all wait the "same" time, increasing the load on that specific moment
  - How to avoid this?
    1. backoff: the time between retries increases
    - spreads the load
    - keeps the sync of requests though
    2. jitter
    - randomness: for each client, add some randomness to the time for the next retry
    - avoids syncronization

> When handling failures, you want to: have timeouts and retries with exponential back off with jitter

## Cascading failures

- More on senior and staff level
- Problems in one part of the system can create problems in other parts of the system

## Summary

- l3: IP (public and private addresses)
  - public: internet facing
  - private: internal, microservices

- l4: tcp/udp
  - tcp: default
  - udp: useful when performance is the most important, even with some data loss

- l7: http/rest/gRPC/websockets/webRTC
  - http: backbone for API protocols
  - rest: default for API design
    - url paths + verbs
  - gRPC: performance
  - graphql: flexibility
  - websockets: enable high-frequency bidirectional communication
  - webRTC: peer-to-peer protocol used normally for audio/video calling (can also be used for collaborative editors)

- load balancing
  - client-side load balancing: internal microservices
    - offers great performance for a limited number of clients
  - server-side load balancing
    - external load balacing
      - more clients
