# Summary Protocols

- HTTP is built on top of TCP
- TCP ensures reliable connection
- UDP ensures fast connection
- HTTP is a way to format requests/reponses into a json object with Method + Content
- Rest API: the default way to organize apis
  - APIs are organized into verbs + URLs
  - A path operates operation on resource
  - Instead of a function calling for certain operation

```
CREATE => /resources/

# with id
GET
UPDATE
PATCH
DELETE
```

- GraphQL is a solution to REST in the sense that:
  - REST can over- or underfetch
  - REST is less flexible than GraphQL and more "backend" oriented, since we need the APIs
  - Graphql is great for flexibility and for "client-decided" requests. The client decides what data it needs
