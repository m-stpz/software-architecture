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

#### gRPC = Protobufs + Servies
