# DB Replicaiton: Master Slave

- Write operations go to master [INSERT/UPDATE/DELETE]
  - Master: source of truth
  - broadcasts the changes to the slaves
- Read operations go to slaves [SELECT]
  - replicas
  - read-only
  - most web apps are "read-heavy"

## Considerations

1. Replication lag: it takes a few ms for data to travel from master to slave

2. Failover: if the master dies, one of the slaves must be "promoted" to be the new master

3. Horizontal scaling for dbs: reads are much easier to scale than writes. For reads, we just add more slaves.
   - For writes, it's harder, it usually requires "sharding"

```js
const { Pool } = require("pg");

// 1. Connection to the Primary (Master) - Read/Write
const primaryPool = new Pool({
  host: "primary-db.example.com",
  user: "postgres",
  database: "myapp",
});

// 2. Connection to the Replica (Slave) - Read-Only
const replicaPool = new Pool({
  host: "replica-db.example.com", // In a real setup, this might be a Load Balancer IP
  user: "postgres",
  database: "myapp",
});

async function handleRequest(userId, newData) {
  try {
    // WRITE: Always use the primaryPool
    await primaryPool.query("UPDATE users SET bio = $1 WHERE id = $2", [
      newData,
      userId,
    ]);
    console.log("Write successful on Primary");

    // READ: Use the replicaPool to save resources on the Primary
    const result = await replicaPool.query(
      "SELECT * FROM users WHERE id = $1",
      [userId],
    );
    console.log("Read successful from Replica");

    return result.rows[0];
  } catch (err) {
    console.error(err);
  }
}
```
