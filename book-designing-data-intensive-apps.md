# Designing Data Intensive Applications

By: Martin Kleppmann. [Purchase the book](https://www.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/)!

Date Started: Saturday, March 27, 2021

Date Finished: TBD

## Chapter 1: Reliable, Scalable, and Maintainable Applications

- **Reliability**: The system should continue to work correctly (performing the correct function at the desired level of performance) even in the face of adversity (hardware or soft‐ ware faults, and even human error).
- **Scalability**: As the system grows (in data volume, traffic volume, or complexity), there should be reasonable ways of dealing with that growth.
- **Maintainability**: Over time, many different people will work on the system (engineering and operations, both maintaining current behavior and adapting the system to new use cases), and they should all be able to work on it productively.
- Reliability is **“continuing to work correctly, even when things go wrong.”**.
- *A **fault** is usually defined as one component of the system deviating from its spec, whereas a **failure** is when the system as a whole stops providing the required service to the user*. It is impossible to reduce the probability of a fault to zero; therefore it is usually best to design fault-tolerance mechanisms that prevent faults from causing failures.
- [Chaos Monkey](https://github.com/Netflix/chaosmonkey).
- How do we make our systems reliable, in spite of unreliable humans? The best systems combine several approaches:
  - Design systems in a way that minimizes opportunities for error.
  - Decouple the places where people make the most mistakes from the places where they can cause failures.
  - Test thoroughly at all levels, from unit tests to whole-system integration tests and manual tests.
  - Set up detailed and clear monitoring, such as performance metrics and error rates. In other engineering disciplines this is referred to as *telemetry*. (Once a rocket has left the ground, telemetry is essential for tracking what is happening, and for understanding failures).
  - Implement good management practices and training.
- **Scalability** is the term we use to describe a system’s ability to cope with increased load. Load can be described with a few numbers which we call **load parameters**. It may be *requests per second* to a web server, the *ratio of reads to writes* in a database, the *number of simultaneously active users* in a chat room, the *hit rate on a cache*, or something else.
- **Fan-out**: A term borrowed from electronic engineering, where it describes the number of logic gate inputs that are attached to another gate’s output. The output needs to supply enough current to drive all the attached inputs. **In transaction processing systems, we use it to describe the number of requests to other services that we need to make in order to serve one incoming request**.
- **Throughput**: the number of records we can process per second, or the total time it takes to run a job on a dataset of a certain size.
- **Response time**: the time between a client sending a request and receiving a response. The response time is what the client sees: besides the actual *time to process the request* (the service time), it includes *network delays* and *queueing delays*.
- **Latency**: the duration that a request is waiting to be handled—during which it is latent, awaiting service.
- Response time measures:
  - **Arithmetic mean** (given n values, add up all the values, and divide by n) *is not a very good metric to know the “typical” response time*, because it doesn’t tell how many users actually experienced that delay.
  - **Median(p50)** is *the halfway point* after taking the list of response times and sort it from fastest to slowest. *The median is also known as the 50th percentile, and sometimes abbreviated as p50*. Note that the median refers to a single request; if the user makes several requests (over the course of a session, or because several resources are included in a single page), the probability that at least one of them is slower than the median is much greater than 50%.
  - The **95th, 99th, and 99.9th percentiles** are common (abbreviated p95, p99, and p999). *They are the response time thresholds at which 95%, 99%, or 99.9% of requests are faster than that particular threshold*. High percentiles of response times, also known as **tail latencies**, are important because they directly affect users’ experience of the service.
- **Queueing delays** often account for a large part of the response time at high percentiles. As a server can only process a small number of things in parallel (limited, for example, by its number of CPU cores), it only takes a small number of slow requests to hold up the processing of subsequent requests—an effect sometimes known as **head-of-line blocking**. Even if those subsequent requests are fast to process on the server, the client will see a slow overall response time due to the time waiting for the prior request to complete. Due to this effect, it is important to measure response times on the client side.

> It takes just one slow call to make the entire end-user request slow.

- The naïve implementation to calculate response time percentiles is to *keep a list of response times for all requests within the time window and to sort that list every minute*. If that is too inefficient for you, there are algorithms that can calculate a good approximation of percentiles at minimal CPU and memory cost, such as **forward decay**, **t-digest**, or **HdrHistogram**. Beware that averaging percentiles, e.g., to reduce the time resolution or to combine data from several machines, is mathematically meaningless—the right way of aggregating response time data is to add the histograms.
- People often talk of a dichotomy between **scaling up** (**vertical scaling**, *moving to a more powerful machine*) and **scaling out** (**horizontal scaling**, *distributing the load across multiple smaller machines*). Distributing load across multiple machines is also known as a *shared-nothing* architecture. A system that can run on a single machine is often simpler, but high-end machines can become very expensive, so very intensive workloads often can’t avoid scaling out. In reality, *good architectures usually involve a pragmatic mixture of approaches*.
- **Elastic** systems can automatically add computing resources when they detect a load increase, whereas other systems are scaled manually (a human analyzes the capacity and decides to add more machines to the system).

> The architecture of systems that operate at large scale is usually highly specific to the application - there is no such thing as a generic, one-size-fits-all scalable architecture (informally known as *magic scaling sauce*). The problem may be the volume of reads, the volume of writes, the volume of data to store, the complexity of the data, the response time requirements, the access patterns, or (usually) some mixture of all of these plus many more issues.

- Maintainability principles
  - **Operability**: Make it easy for operations teams to keep the system running smoothly.
  - **Simplicity**: Make it easy for new engineers to understand the system, by removing as much complexity as possible from the system.
  - **Evolvability**: Make it easy for engineers to make changes to the system in the future, adapting it for unanticipated use cases as requirements change. Also known as *extensibility*, *modifiability*, or *plasticity*.
- Data systems can do various things to make routine tasks easy, including:
  - Providing **visibility** into the runtime behavior and internals of the system, with good monitoring.
  - Providing good support for **automation and integration** with standard tools.
  - Avoiding dependency on individual machines (allowing machines to be taken down for maintenance while the system as a whole continues running uninterrupted).
  - Providing good **documentation** and an easy-to-understand operational model (“If I do X, Y will happen”).
  - Providing good default behavior, but also giving administrators the freedom to override defaults when needed.
  - **Self-healing** where appropriate, but also giving administrators manual control over the system state when needed.
  - Exhibiting predictable behavior, minimizing surprises.
- There are various possible symptoms of complexity:
  - Explosion of the state space
  - Tight coupling of modules
  - Tangled dependencies
  - Inconsistent naming and terminology
  - Hacks aimed at solving performance problems
  - Special-casing to work around issues elsewhere
