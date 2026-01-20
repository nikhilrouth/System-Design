### A — Availability
> **Is the system always there when you need it?**

Focuses on building systems that stay operational and accessible, minimizing downtime through strategies like **redundancy, load balancing, and failover** to ensure continuous service.

Availability is measured in "nines":

| Availability | Downtime/Year | Example |
|--------------|---------------|---------|
| 99% (two nines) | 3.65 days | Personal blog |
| 99.9% (three nines) | 8.76 hours | Business apps |
| 99.99% (four nines) | 52.6 minutes | E-commerce |
| 99.999% (five nines) | 5.26 minutes | Banking, Healthcare |

## Key Design Strategies for Reliability & High Availability

Modern distributed systems must expect failures and recover quickly. The following strategies are foundational to building resilient systems.

---

### 1. Redundancy
**Definition:**  
Duplicating critical components (servers, databases, services) so a backup can take over if one fails.

**Why it matters:**  
Eliminates single points of failure and enables high availability.

---

### 2. Deployment Models

#### Hot–Hot
- All instances are active and handle traffic simultaneously
- Requires strong synchronization and conflict resolution

**Pros:** Maximum availability  
**Cons:** High complexity and cost

#### Hot–Warm
- Primary instance handles writes
- Secondary instance handles reads or stands by for failover
- Some data replication lag is possible

**Pros:** Balanced cost and availability  
**Cons:** Possible stale reads

#### Hot–Cold
- Primary instance is active
- Backup remains idle until failure
- Often requires manual or slower automated failover

**Pros:** Cost-effective  
**Cons:** Higher recovery time (RTO)

---

### 3. Load Balancing
**Definition:**  
Distributing incoming traffic across multiple servers.

**Benefits:**
- Prevents overload
- Improves performance
- Enables seamless failover when a node becomes unhealthy

Common types:
- Layer 4 (TCP/UDP) Transport
- Layer 7 (HTTP-aware) Application

---

### 4. Failover
**Definition:**  
Automatically switching traffic to a healthy component when the primary fails.

**Key aspects:**
- Fast detection via health checks
- Minimal service disruption
- Often integrated with load balancers or DNS

---

### 5. Data Replication
**Definition:**  
Maintaining multiple copies of data across nodes or regions.

**Goals:**
- Fault tolerance
- Faster reads
- Disaster recovery

**Trade-offs:**
- Strong consistency vs eventual consistency
- Replication lag

---

### 6. Monitoring & Health Checks
**Definition:**  
Continuous observation of system health and performance.

**Monitors typically include:**
- Latency (p95/p99)
- Error rates
- CPU, memory, disk usage
- Service availability

**Why it matters:**  
You can’t fix what you can’t see.

---

### 7. Geo-Redundancy & Disaster Recovery
**Definition:**  
Deploying systems across multiple geographic regions.

**Protects against:**
- Data center outages
- Natural disasters
- Regional network failures

**Key metrics:**
- RPO (Recovery Point Objective)
- RTO (Recovery Time Objective)

---

## Final Principle

> **Assume everything will fail — design so users never notice.**
> **Every strategy aims to reduce the periods the system is inaccessible.**

---
---
### Interview Question

**Interview Question:** *"What's your system's availability target and how will you achieve it?"*

For a Senior Engineer role, a safe and professional target to propose is "Three Nines" (99.9%) for standard services or "Four Nines" (99.99%) for mission-critical ones.

