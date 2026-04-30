# Load balancer with nginx

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
