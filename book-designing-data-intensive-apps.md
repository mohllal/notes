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

## Chapter 2: Data Models and Query Languages

- In **relational** data model: data is organized into relations (called tables in SQL), where each relation is an unordered collection of tuples (rows in SQL).
- There are several driving forces behind the adoption of NoSQL databases, including:
  - A need for *greater scalability* than relational databases can easily achieve, including very large datasets or very high write throughput.
  - A widespread preference for *free and open source* software over commercial database products.
  - *Specialized query operations* that are not well supported by the relational model.
  - Frustration with the *restrictiveness of relational schemas*, and a desire for a more dynamic and expressive data model.
- **"The Object-Relational Mismatch"** can be described as the disconnection between the data models which is stored in relational tables and the application code object-oriented model, thus an awkward *translation layer is required between the objects in the application code and the database model of tables, rows, and columns*.
- Removing text duplication and using IDs instead for data representation is the key idea behind **normalization** in databases. Unfortunately, *normalizing data requires many-to-one relationships* (many people live in one particular region, many people work in one particular industry), which don’t fit nicely into the document model. *In relational databases, it’s normal to refer to rows in other tables by ID, because joins are easy. In document databases, joins are not needed for one-to-many tree structures, and support for joins is often weak and the work of making the join is shifted from the database to the application code*.
- The network data model, **CODASYL model**, was a generalization of the hierarchical model. In the tree structure of the hierarchical model, every record has exactly one parent; *in the network model, a record could have multiple parents*. **The links between records in the network model were not foreign keys, but more like pointers in a programming language (while still being stored on disk)**. The only way of accessing a record was to follow a path from a root record along these chains of links. This was called an *access path*.
- **A query in CODASYL was performed by moving a cursor through the database by iterating over lists of records and following access paths**. If a record had multiple parents (i.e., multiple incoming pointers from other records), the application code had to keep track of all the various relationships. Even CODASYL committee members admitted that this was like navigating around an n-dimensional data space, so the problem is that *the code for querying and updating the database become complicated and inflexible*.
- What the relational model did, by contrast, was to lay out all the data in the open: a relation (table) is simply a collection of tuples (rows), and that’s it. There are no labyrinthine nested structures, no complicated access paths to follow.
- Query optimizers for relational databases are complicated beasts, and they have consumed many years of research and development effort. But a key insight of the relational model was this: you only need to build a query optimizer once, and then all applications that use the database can benefit from it.
- The main arguments in favor of the document data model are **schema flexibility**, **better performance due to locality**, and that for some applications it is **closer to the data structures used by the application**. The relational model counters by providing **better support for joins**, and **many-to-one** and **many-to-many** relationships.
- In document model, it’s possible to **reduce the need for joins by denormalizing**, but then the application code needs to do additional work to keep the denormalized data consistent. Joins can be emulated in application code by making multiple requests to the database, but that also moves complexity into the application and is usually slower than a join performed by specialized code inside the database.
- Document databases are sometimes called **schemaless**, but that’s misleading, as the code that reads the data usually assumes some kind of structure i.e., there is an implicit schema, but it is not enforced by the database. A more accurate term is **schema-on-read** (the structure of the data is implicit, and only interpreted when the data is read), in contrast with **schema-on-write** (the traditional approach of relational databases, where the schema is explicit and the database ensures all written data conforms to it).
- **A document is usually stored as a single continuous string**, encoded as JSON, XML, or a binary variant thereof (such as MongoDB’s BSON). **The locality advantage only applies if you need large parts of the document at the same time**. The database typically needs to load the entire document, even if you access only a small portion of it, which can be wasteful on large documents. **On updates to a document, the entire document usually needs to be rewritten—only modifications that don’t change the encoded size of a document can easily be performed in place**. For these reasons, it is generally recommended that you keep documents fairly small and avoid writes that increase the size of a document. These performance limitations significantly reduce the set of situations in which document databases are useful.
- An **imperative language** tells the computer to perform certain operations in a certain order. In a **declarative query** language, like SQL or relational algebra, you just specify the pattern of the data you want—what conditions the results must meet, and how you want the data to be transformed (e.g., sorted, grouped, and aggregated)—but not how to achieve that goal.
- **MapReduce is a programming model for processing large amounts of data in bulk across many machines**, popularized by Google. A limited form of MapReduce is supported by some NoSQL datastores, including MongoDB and CouchDB, as a mechanism for performing read-only queries across many documents. MapReduce is neither a declarative query language nor a fully imperative query API, but somewhere in between: *the logic of the query is expressed with snippets of code, which are called repeatedly by the processing framework*. It is based on the **map** (also known as *collect*) and **reduce** (also known as *fold* or *inject*) functions that exist in many functional programming languages.
- MongoDB 2.2 added support for a *declarative query language* called the **aggregation pipeline**. The aggregation pipeline language is similar in expressiveness to a subset of SQL, but it uses a JSON-based syntax rather than SQL’s English-sentence-style syntax; the difference is perhaps a matter of taste.
- There are several different, but related, ways of structuring and querying data in graphs e.g. the **property graph model** (implemented by Neo4j, Titan, and InfiniteGraph) and the **triple-store model** (implemented by Datomic, AllegroGraph, and others). And there are three declarative query languages for graphs: *Cypher*, *SPARQL*, and *Datalog*. Besides these, there are also imperative graph query languages such as *Gremlin* and graph processing frameworks like *Pregel*.
- In the property graph model, each *vertex* consists of:
  - A unique identifier
  - A set of outgoing edges
  - A set of incoming edges
  - A collection of properties (key-value pairs)
- In the property graph model, each *edge* consists of:
  - A unique identifier
  - The vertex at which the edge starts (the tail vertex)
  - The vertex at which the edge ends (the head vertex)
  - A label to describe the kind of relationship between the two vertices
  - A collection of properties (key-value pairs)
- **Cypher** is a declarative query language for property graphs, created for the Neo4j graph database.

```
CREATE
  (NAmerica:Location {name:'North America', type:'continent'}),
  (USA:Location      {name:'United States', type:'country'  }),
  (Idaho:Location    {name:'Idaho',         type:'state'    }),
  (Lucy:Person       {name:'Lucy' }),
  (Idaho) -[:WITHIN]->  (USA)  -[:WITHIN]-> (NAmerica),
  (Lucy)  -[:BORN_IN]-> (Idaho)
```

- The triple-store model is mostly equivalent to the property graph model, using differ‐ ent words to describe the same ideas. In a triple-store, all information is stored in the form of very simple three-part statements: (**subject, predicate, object)**.
- The **semantic web** is fundamentally a simple and reasonable idea: websites already publish information as text and pictures for humans to read, so why don’t they also publish information as machine-readable data for computers to read? The **Resource Description Framework (RDF)** was intended as a mechanism for different websites to publish data in a consistent format, allowing data from different websites to be automatically combined into a web of data—a kind of internet-wide *“database of everything”*.
- Datalog’s data model is similar to the triple-store model, generalized a bit. Instead of writing a triple as (subject, predicate, object), we write it as predicate(subject, object).
