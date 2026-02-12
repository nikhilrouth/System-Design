### E — Efficiency
> **Does it use resources wisely?**

Efficiency is about maximizing output while minimizing resource usage (compute, storage, network, cost).

it focuses on maximizing resource utilization (CPU, memory, network, energy) to achieve high performance, low latency, and cost-effectiveness

| Metric | What It Measures |
|--------|------------------|
| **Throughput** | Requests processed per second |
| **Resource Utilization** | CPU, Memory, Network usage |
| **Cost per Request** | Infrastructure cost per operation |

---
**Interview Question:** *"How would you optimize this system to reduce costs by 50%?"*  

To reduce costs by 50% without sacrificing performance, you have to move from a "provisioned" mindset (paying for what you might need) to a "utilization" mindset (paying only for what you actually use).

### 1. Right sizing and Autoscaling

**Aggresive Horizontal Scaling:** Instead of using 10 huge nodes that run 24/7, use medium or small nodes that scale up and down based on load.

**Spot Instances:** For batch processes, logs, and background processing, using less expensive instances such as AWS Spot instances can save up to 90% of the cost compared to on-demand instances.

**Virtical Scaling (Gridlock Princple):** Use auto scaling; for example, the JVM needs 3GB for a 6GB pod that shrinks to 3GB, saving pods money.

### 2. Move to Serverless & Manged Services

If a service is only used occasionally (like your HR Chatbot during non-business hours), you shouldn't pay for it to stay "awake."

**Scale to Zero:** Use AWS Lambda or Google Cloud Run for specific microservices. When no one is using the chatbot, the cost is literally $0.

**Serverless Databases:** Move from a fixed RDS instance to Amazon Aurora Serverless, which scales its capacity (and cost) up and down based on the number of queries.

### 3. Application level Efficency

Java can be "heavy," but it can be optimized to run on smaller, cheaper hardware.

**ARM Based chips:** Switch Infra to ARM based chips (AWS Graviton) from x86 chips. it typically offers 40% better price-performance for Java workloads. 

**Virtual threads:** Switch to virtual threads instead of the massive thread pools it performaces same for lessser cost

**Native Compilation:** Convert your Spring Boot apps into native binaries. This reduces startup time from seconds to milliseconds and cuts memory usage by up to 5x, allowing you to fit more apps on a single cheap server.

### 4. Data and Storgae Optimization

**Storage Tiering:** Move old logs or historical GenAI outputs from expensive "Standard S3" to S3 Intelligent-Tiering or Glacier.

**Data Lifecycle Policies:** Automatically delete temporary files or "old" cache entries after 30 days.

**Reduced Data Transfer:** Ensure your microservices are talking to each other within the same region/availability zone to avoid "Data Transfer Out" fees, which are a hidden cost killer.


To cut costs by 50%, I would implement a three-pronged strategy: first, I’d transition our compute to ARM-based Graviton instances and Spot instances for background tasks. Second, I’d optimize our Java apps using GraalVM native images to reduce memory footprints. Finally, I’d implement Scale-to-Zero architectures for low-traffic services and move historical data to lower-cost storage tiers
