# Network essentials

https://www.youtube.com/watch?v=SHkbPm1Wrno

## OSI Model

- Think of network as a layered cake

| layers | name         | description                                                                 | Example                           |
| ------ | ------------ | --------------------------------------------------------------------------- | --------------------------------- |
| l7     | application  | Interface where user interacts with the network                             | HTTP, DNS, SMTP, FTP              |
| l6     | presentation | Handles data encryption, compression, and formatting                        | SSL/TLS, JPEG, GIF                |
| l5     | session      | Manages the "conversation" (opening/closing/restarting) between two devices | NetBIOS, RPC, Sockets             |
| l4     | transport    | Handles the delivery and error-checking of data packets                     | TCP, UDP                          |
| l3     | network      | Determines the best physical path for data to travel                        | IP, Routers, ICMP                 |
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
