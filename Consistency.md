### C — Consistency
> **Do all users see the same data at the same time?**

Consistency ensures that every read receives the most recent write.

| Type | Description | Use Case |
|------|-------------|----------|
| **Strong Consistency** | All nodes see same data instantly | Banking transactions |
| **Eventual Consistency** | Data syncs eventually, not instantly | Social media likes count |



|  |Strong Consistency|Eventual Consistency|
|-------------|----------|----------|
|**Definition:** | Every read returns the most recent successful write; all nodes agree on the data state immediately.| After writes stop, all replicas will eventually converge to the same value, but temporary inconsistencies are possible.|
|**Pros:** | High data integrity, no stale data.| High availability, low latency, better scalability (e.g., NoSQL databases).|
|**Cons:** | Higher latency, reduced availability during failures (must wait for replication).| Can display outdated data temporarily, requires conflict resolution.|
|**Use Cases:** | Banking, inventory management, critical healthcare records.| Social media feeds, user profiles, recommendation engines. |

___

**Interview Question:** *"Should your system prioritize consistency or availability?"*
