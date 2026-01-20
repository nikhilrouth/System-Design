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

I target 99.99% availability for core services. I achieve this by ensuring no single point of failure exists in our infrastructure through Multi-AZ redundancy, using Resilience4j to prevent cascading failures in our microservices, and utilizing automated canary rollbacks to minimize the impact of bad code. Our goal is to keep our Mean Time To Recovery (MTTR) under 5 minutes through proactive alerting and automated self-healing."

**How to Achieve It (The "Three Pillars" Strategy)**
To hit 99.99%, you must move from "reactive" fixing to "proactive" prevention across three domains:

## I. Infrastructure: Eliminate Single Points of Failure (SPOF)
**Multi-AZ Deployment:** Never run in a single data center. Use at least 3 Availability Zones (AZs). If an entire AZ goes dark due to a power failure, your Load Balancer (ALB) automatically shifts traffic to the healthy zones.

**Database High Availability:** Use a "Multi-AZ" database setup with a synchronous standby. If the primary database fails, the system triggers an automatic failover to the standby in seconds.

**Stateless Services:** Ensure your application servers hold no "session memory." If a server dies, a new one can take its place immediately without the user losing their progress.

## II. Application: Resilience Patterns
**Circuit Breakers (e.g., Resilience4j):** If a third-party service (like a shipping API) is slow, don't let it "hang" your system. The circuit breaker "trips," giving your user a cached or "limited" response instead of a spinning wheel.

**Retries with Exponential Backoff:** If a request fails, don't hammer the server again instantly. Wait 100ms, then 200ms, then 400ms. This prevents a "thundering herd" effect that could crash a recovering system.

**Health Checks:** Implement deep health checks. If a service can't talk to its database, it should report itself as "Unhealthy" so the orchestrator (like Kubernetes) can kill and restart it.

## III. Operations: Reducing MTTR (Mean Time To Recovery)
Availability isn't just about not breaking; it's about fixing things fast.

**Automated Monitoring & Alerting:** Use tools like Prometheus/Grafana or Datadog. You should have an automated alert for "high error rates" that pages an engineer before the total downtime begins.

**Canary Deployments:** Use Blue-Green or Canary deployments. If a new code change causes a spike in 5xx errors, the system should automatically roll back to the previous stable version in seconds.

**Chaos Engineering:** Occasionally "kill" a server in production on purpose (like Netflix's Chaos Monkey) to prove your system can actually handle a real failure without human intervention.


