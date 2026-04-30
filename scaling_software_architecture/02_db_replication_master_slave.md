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
