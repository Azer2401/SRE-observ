Observability is the ability to understand the internal state of a system by examining the outputs it produces—such as logs, metrics, and traces. It enables teams to detect, investigate, and resolve issues in complex, distributed systems.

## Core pillars of observability
![three-pillars-of-observability](../assets/three-pillars-of-observability.png)

The three pillars of observability (metrics, logs, and traces) are the core telemetry data types that give you visibility into the internal state of your systems. When combined, they enable a deep understanding of system behavior, facilitate root cause analysis, and support proactive operations.

**Metrics**  
Time-series data collected from various sources, such as infrastructure, platform and application software. Metrics are the original telemetry signals of monitoring and can be categorized into several types:  

- **Server**: Monitor the underlying physical metrics.  
- **Network & Security**: Track the availability, throughput, and security of networking components.  
- **Storage**: Observe the performance and capacity of persistent storage systems.  
- **Kubernetes**: Expose the health and behavior of Kubernetes clusters.  
- **Virtual Cluster**: Track observability data for logical groupings of resources (e.g., tenant clusters in multi-tenantsystems).  
- **Virtual Machine**: Similar to server metrics but often gathered from hypervisors (e.g., KubeVirt).  
- **Application**: Expose business and application logic performance.  

These metrics are necessary for setting alerting, warning, and error condition thresholds. They enable teams to surveil system and network performance overall and identify issues when they arise. In this way, metrics inform a reactive stance in monitoring.

**Logs**  
Timestamped, unstructured or semi-structured text records describing discrete events. Logs provide a comprehensive record of all events and errors that take place during the life cycle of software resources.

Use cases:

- Debugging application errors.
- Auditing user activity.
- Capturing system events (e.g., authentication failure).


**Traces**  
Distributed traces show the flow of a request across services, capturing timing and contextual metadata at each step and used to isolate the root cause of an infrastructure or code problem.

Use cases:

- Identify bottlenecks in microservices (e.g., service portal).
- Visualize service dependencies.
- Analyze high-latency transactions.

### Why All Three Matter
Using only one or two pillars limits your ability to answer critical questions. For example:

| Question                           | Best Tool(s)         |
|------------------------------------|----------------------|
| Is it broken?                      | 📈 Metrics          |
| Why did it break?                  | 📝 Logs, 📡 Traces  |
| Where in the system did it fail?   | 📡 Traces            |
| Has this happened before?          | 📝 Logs, 📈 Metrics |
| How often does it happen?          | 📈 Metrics           |
| What was the user impact?          | 📝 Logs, 📡 Traces  |