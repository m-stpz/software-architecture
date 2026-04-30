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

### Internet Protocol

- IP: Giving "names" to nodes in the network and allow routing
  - IPv4: 4 bytes
    - normally used externally
  - IPv6: 16 bytes
    - more used internally

- They come in two flavors:
  - Private: protected from the world
    - Used in: microservices, internal config
  - Public: known to the world
    - assigned by a "central body"
    - routers are aware of them
    - Used in: API gateways, load balancers [externally facing components of your design]
