# Designing Data Intensive Applications

By: Martin Kleppmann.
[Book Link](https://www.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/).

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

## Chapter 3: Storage and Retrieval

- An *index* is an additional structure that is derived from the primary data. Many databases allow you to add and remove indexes, and this doesn’t affect the contents of the database; it only affects the performance of queries. Maintaining additional structures incurs overhead, especially on writes. For writes, it’s hard to beat the performance of simply appending to a file, because that’s the simplest possible write operation. *Any kind of index usually slows down writes, because the index also needs to be updated every time data is written*.

- Key-value stores are quite similar to the **dictionary type** that can be found in most programming languages, and which is usually implemented as a hash map (hash table). Then the simplest possible indexing strategy is this: **keep an in-memory hash map where every key is mapped to a byte offset in the data file the location at which the value can be found**.
- To solve the running out of disk space for the log file:
  - **Break the log into segments of a certain size by closing a segment file when it reaches a certain size, and making subsequent writes to a new segment file**.
  - **Perform compaction on the segments files. Compaction means throwing away duplicate keys in the log, and keeping only the most recent update for each key**.
- **Moreover, since compaction often makes segments much smaller (assuming that a key is overwritten several times on average within one segment), we can also merge several segments together at the same time as performing the compaction**. Segments are never modified after they have been written, so the merged segment is written to a new file. *The merging and compaction of frozen segments can be done in a background thread, and while it is going on, we can still continue to serve read and write requests as normal, using the old segment files*. After the merging process is complete, we switch read requests to using the new merged segment instead of the old segments—and then the old segment files can simply be deleted.
- Hash Indexes Implementation Issues:
  - **File format**: CSV is not the best format for a log. It’s faster and simpler to use a binary format that first encodes the length of a string in bytes, followed by the raw string (without need for escaping).
  - **Deleting records**: If we want to delete a key and its associated value, we have to *append a special deletion record to the data file (sometimes called a tombstone)*. When log segments are merged, the tombstone tells the merging process to discard any previous values for the deleted key.
  - **Crash recovery**: If the database is restarted, the in-memory hash maps are lost. *In principle, we can restore each segment’s hash map by reading the entire segment file from beginning to end and noting the offset of the most recent value for every key as we go along*.
  - **Partially written records**: The database may crash at any time, including halfway through appending a record to the log. Bitcask files include checksums, allowing such corrupted parts of the log to be detected and ignored.
  - **Concurrency control**: As writes are appended to the log in a strictly sequential order, a common implementation choice is to have only one writer thread. *Data file segments are append-only and otherwise immutable, so they can be read concurrently by multiple threads*.
- An append-only design turns out to be good for several reasons:
  - **Appending and segment merging are sequential write operations, which are generally much faster than random writes**, especially on magnetic spinning-disk hard drives. To some extent sequential writes are also preferable on flash-based solid state drives (SSDs).
  - **Concurrency and crash recovery are much simpler if segment files are append-only or immutable**.
  - **Merging old segments avoids the problem of data files getting fragmented over time**.
- However, the hash table index also has limitations:
  - **The hash table must fit in memory**, so if we have a very large number of keys, we’re out of luck. In principle, we could maintain a hash map on disk, but unfortunately it is difficult to make an on-disk hash map perform well. It requires a lot of random access I/O, it is expensive to grow when it becomes full, and hash collisions require fiddly logic.
  - Range queries are not efficient.
- Sequence of key-value pairs is sorted by key: **Sorted String Table**, or **SSTable** where an in-memory index to store the offsets for some of the keys, but it can be sparse: one key for every few kilobytes of segment file is sufficient, because a few kilobytes can be scanned very quickly.
- Log-Structured Merge-Tree (or LSM-Tree) storage engine work as follows:
  - When a write comes in, add it to an in-memory balanced tree data structure (for example, a red-black tree). This in-memory tree is sometimes called a **memtable**.
  - When the **memtable** gets bigger than some threshold—typically a few megabytes —write it out to disk as an **SSTable** file. This can be done efficiently because the tree already maintains the key-value pairs sorted by key. The new SSTable file becomes the most recent segment of the database. While the SSTable is being written out to disk, writes can continue to a new memtable instance.
  - In order to serve a read request, first try to find the key in the memtable, then in the most recent on-disk segment, then in the next-older segment, etc.
  - From time to time, run a merging and compaction process in the background to combine segment files and to discard overwritten or deleted values.
- **The LSM-tree algorithm can be slow when looking up keys that do not exist in the database**: you have to check the memtable, then the segments all the way back to the oldest (possibly having to read from disk for each one) before you can be sure that the key does not exist. In order to optimize this kind of access, storage engines often use additional **Bloom filters**. (A Bloom filter is a memory-efficient data structure for approximating the contents of a set. It can tell you if a key does not appear in the database, and thus saves many unnecessary disk reads for nonexistent keys.)
- **B-trees** break the database down into *fixed-size blocks or pages*, traditionally 4 KB in size (sometimes bigger), and read or write one page at a time. This design corresponds more closely to the underlying hardware, as disks are also arranged in fixed-size blocks. Each page can be identified using an address or location, which allows one page to refer to another—similar to a pointer, but on disk instead of in memory.
- A B-tree with n keys always has a depth of `O(log n)`. Most databases can fit into a B-tree that is *three or four levels deep*, so you don’t need to follow many page references to find the page you are look‐ ing for. (A four-level tree of 4 KB pages with a branching factor of 500 can store up to 256 `TB`.).
- In order to make the B-Tree based database resilient to crashes, it is common for B-tree implementations to include an additional data structure on disk: **a write-ahead log (WAL, also known as a redo log**). *This is an append-only file to which every B-tree modification must be written before it can be applied to the pages of the tree itself*. When the database comes back up after a crash, this log is used to restore the B-tree back to a consistent state.
- The most common type of multi-column index is called a **concatenated index**, which simply combines several fields into one key by appending one column to another (the index definition specifies in which order the fields are concatenated).
- Some in-memory key-value stores, such as Memcached, are intended for caching use only, where it’s acceptable for data to be lost if a machine is restarted. But other in- memory databases aim for durability, which can be achieved with special hardware (such as battery-powered RAM), by writing a log of changes to disk, by writing peri‐ odic snapshots to disk, or by replicating the in-memory state to other machines.
- The so-called **anti-caching** approach works by evicting the least recently used data from memory to disk when there is not enough memory, and loading it back into memory when it is accessed again in the future.
- A data warehouse, by contrast, is a separate database that analysts can query to their hearts’ content, without affecting OLTP operations. The data warehouse contains a read-only copy of the data in all the various OLTP systems in the company. The process of getting data into the warehouse is known as **Extract–Transform–Load (ETL)**.
- Many data warehouses are used in a fairly formulaic style, known as a **star schema** (also known as dimensional modeling). At the center of the schema is a so-called **fact table**. The name “star schema” comes from the fact that when the table relationships are visualized, the fact table is in the middle, surrounded by its dimension tables; the connections to these tables are like the rays of a star.
- The idea behind **column-oriented storage** is simple: don’t store all the values from one row together, but store all the values from each column together instead. *If each column is stored in a separate file, a query only needs to read and parse those columns that are used in that query, which can save a lot of work*.
- **Materialized view**: In a relational data model, it is often defined like a *standard (virtual) view*: a table-like object whose contents are the results of some query. The difference is that a materialized view is an actual copy of the query results, written to disk, whereas a virtual view is just a shortcut for writ‐ ing queries. When you read from a virtual view, the SQL engine expands it into the view’s underlying query on the fly and then processes the expanded query.
- A common special case of a materialized view is known as a **data cube** or **OLAP cube**. It is a grid of aggregates grouped by different dimensions. The advantage of a materialized data cube is that certain queries become very fast because they have effectively been precomputed.

## Chapter 4: Encoding and Evolution

- The translation from the in-memory representation to a byte sequence is called **encoding** (also known as *serialization* or *marshalling*) and the reverse is called **decoding** (*parsing*, *deserialization*, *unmarshalling*).
- JSON, XML, and CSV are textual formats, and thus somewhat human-readable (although the syntax is a popular topic of debate). Besides the superficial syntactic issues, they also have some subtle problems:
  - There is a lot of ambiguity around the encoding of numbers. In XML and CSV, you cannot distinguish between a number and a string that happens to consist of digits (except by referring to an external schema). JSON distinguishes strings and numbers, but it doesn’t distinguish integers and floating-point numbers, and it doesn’t specify a precision.
  - JSON and XML have good support for Unicode character strings (i.e., human- readable text), but they don’t support binary strings (sequences of bytes without a character encoding).
  - There is optional schema support for both XML and JSON. These schema languages are quite powerful, and thus quite complicated to learn and implement.
  - CSV does not have any schema, so it is up to the application to define the mean‐ ing of each row and column.
- Binary encoding formats:
  - Thrift (Facebook).
    - BinaryProtocol - 53B of 81B JSON.
    - CompactProtocol - 34B of 81B JSON.
    - DenseProtocol
  - Protocol Buffers (Google) - 33B of 81B JSON.
  - Avro (Apache) - 32B of 81B JSON.
- Binary encodings based on schemas are viable:
  - They can be much more compact than the various “binary JSON” variants, since they can omit field names from the encoded data.
  - The schema is a valuable form of documentation, and because the schema is required for decoding, you can be sure that it is up to date (whereas manually maintained documentation may easily diverge from reality).
  - Keeping a database of schemas allows you to check forward and backward compatibility of schema changes, before anything is deployed.
  - For users of statically typed programming languages, the ability to generate code from the schema is useful, since it enables type checking at compile time.
- The RPC model tries to make a request to a remote net‐ work service look the same as calling a function or method in your programming language, within the same process (this abstraction is called location transparency).
- The backward and forward compatibility properties of an RPC scheme are inherited from whatever encoding it uses:
  - Thrift, gRPC (Protocol Buffers), and Avro RPC can be evolved according to the compatibility rules of the respective encoding format.
  - In SOAP, requests and responses are specified with XML schemas. These can be evolved, but there are some subtle pitfalls.
  - RESTful APIs most commonly use JSON (without a formally specified schema) for responses, and JSON or URI-encoded/form-encoded request parameters for requests. Adding optional request parameters and adding new fields to response objects are usually considered changes that maintain compatibility.
- For RESTful APIs, common approaches are to use a version number in the URL or in the HTTP Accept header. For services that use API keys to identify a particular client, another option is to store a client’s requested API version on the server and to allow this version selection to be updated through a separate administrative interface.
- Using a message broker has several advantages compared to direct RPC:
  - It can act as a buffer if the recipient is unavailable or overloaded, and thus improve system reliability.
  - It can automatically redeliver messages to a process that has crashed, and thus prevent messages from being lost.
  - It avoids the sender needing to know the IP address and port number of the recipient (which is particularly useful in a cloud deployment where virtual machines often come and go).
  - It allows one message to be sent to several recipients.
  - It logically decouples the sender from the recipient (the sender just publishes messages and doesn’t care who consumes them).
- Message brokers typically don’t enforce any particular data model—a message is just a sequence of bytes with some metadata, so you can use any encoding format. If the encoding is backward and forward compatible, you have the greatest flexibility to change publishers and consumers independently and deploy them in any order.

## Chapter 5: Replication

- Scaling to higher load:
  - Shared-memory architecture.
  - Shared-disk architecture.
  - Shared-nothing architecture.
- There are two common ways data is distributed across multiple nodes:
  - **Replication**: Keeping a copy of the same data on several different nodes, potentially in differ‐ ent locations. Replication provides redundancy: if some nodes are unavailable, the data can still be served from the remaining nodes.
  - **Partitioning**: Splitting a big database into smaller subsets called partitions so that different partitions can be assigned to different nodes (also known as sharding).
- Replication means keeping a copy of the same data on multiple machines that are connected via a network:
  - To keep data geographically close to your users (and thus reduce latency).
  - To allow the system to continue working even if some of its parts have failed (and thus increase availability).
  - To scale out the number of machines that can serve read queries (and thus increase read throughput).
- Replication algorithms:
  - Leader based replication
    - Single-leader (active/passive or master/slave)
    - Multi-leader (active/active or master/master)
  - Leaderless based replication (coordinated replication)
- Single leader-based replication strategies:
  - Asynchronous replication
  - Semi-synchronous replication
  - Synchronous replication
- How do you achieve high availability with single leader-based replication?
  - Follower failure: Catch-up recovery
  - Leader failure: Failover
- How does single leader-based replication work under the hood?
  - Statement-based replication
  - Write-ahead log (WAL) shipping
  - Logical (row-based) log replication
  - Trigger-based replication
- *Eventual consistency* is inconsistency in the database caused by the replication lag between the leader nodes and its followers when read queries are issued to one of the followers nodes.
- Replication lag problems:
  - Read-after-write consistency
  - Monotonic reads
  - Consistent prefix reads
- Use cases for multi-leader replication
  - Multi-datacenter operation
  - Clients with offline operation
  - Collaborative editing
- Multi-leader replication problems:
  - Handling write conflicts
  - Converging toward a consistent state
- Multi-leader replication topologies:
  - All-to-all (A-A) topology
  - Star (S) topology
  - Circular (C) topology
- Quorums for reading and writing: if there are `n` replicas, every write must be confirmed by `w` nodes to be considered successful, and we must query at least `r` nodes for each read. As long as `w + r > n`, we expect to get an up-to-date value when reading, because at least one of the `r` nodes we’re reading from must be up to date. Reads and writes that obey these `r` and `w` values are called quorum reads and writes.
- Last write wins (discarding concurrent writes)
- The “happens-before” relationship and concurrency
- Version vectors: Each replica increments its own version number when processing a write, and also keeps track of the version numbers it has seen from each of the other replicas. This information indicates which values to overwrite and which values to keep as siblings.

## Chapter 6: Partitioning

- Partitioning of Key-Value Data:
  - Partitioning by Key Range.
  - Partitioning by Hash of Key.
- Partitioning and Secondary Indexes:
  - Partitioning Secondary Indexes by Document: sending the query to all partitions, and combine all the results you get back is sometimes known as *scatter/ gather*, and it can make read queries on secondary indexes quite expensive.
  - Partitioning Secondary Indexes by Term: constructing a global index that covers data in all partitions.
- Rebalancing Partitions:
  - After rebalancing, the load (data storage, read and write requests) should be shared fairly between the nodes in the cluster.
  - While rebalancing is happening, the database should continue accepting reads and writes.
  - No more data than necessary should be moved between nodes, to make rebalanc‐ ing fast and to minimize the network and disk I/O load.
- Strategies for Rebalancing:
  - hash mod N.
  - Fixed number of partitions.
  - Dynamic partitioning.
- On a high level, there are a few different approaches to the request re-routing problem:
  - Allow clients to contact any node (e.g., via a round-robin load balancer). If that node coincidentally owns the partition to which the request applies, it can handle the request directly; otherwise, it forwards the request to the appropriate node, receives the reply, and passes the reply along to the client.
  - Send all requests from clients to a routing tier first, which determines the node that should handle each request and forwards it accordingly. This routing tier does not itself handle any requests; it only acts as a partition-aware load balancer.
  - Require that clients be aware of the partitioning and the assignment of partitions to nodes. In this case, a client can connect directly to the appropriate node, without any intermediary.
- Many distributed data systems rely on a separate coordination service such as ZooKeeper to keep track of this cluster metadata Each node registers itself in ZooKeeper, and ZooKeeper maintains the authoritative mapping of partitions to nodes. Other actors, such as the routing tier or the partitioning-aware client, can subscribe to this information in ZooKeeper. Whenever a partition changes ownership, or a node is added or removed, ZooKeeper notifies the routing tier so that it can keep its routing information up to date.
