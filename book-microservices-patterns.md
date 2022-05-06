# Microservices Patterns

By: Chris Richardson.
[Book Link](https://www.amazon.com/Microservices-Patterns-examples-Chris-Richardson/dp/1617294543).

Date Started: Friday, April 1, 2022
Date Finished: TBD

## Chapter 1: Escaping Monolithic Hell

- Benefits of the monolithic architecture:
  - Simple to develop.
  - Easy to make radical changes to the application.
  - Straightforward to test.
  - Straightforward to deploy.
  - Easy to scale.
- Monolithic hell
  - Complexity intimidates developers
  - Slow of development.
  - Path from commit to deployment is long and arduous.
  - Scaling is difficult.
  - Reliability is hard to maintain.
  - Locked into obsolete technology stack.
- Adrian Cockcroft, formerly of Netflix, defines a Microservice architecture as a service-oriented architecture composed of loosely coupled elements that have bounded contexts.
- The 3D scale cube:
  - x-axis scaling load balances requests across multiple instances a.k.a **horizontal duplication**.
  - y-axis scaling decomposes an application into services a.k.a **functional decomposition**.
  - z-axis scaling runs multiple instances of the monolith application, but unlike X-axis scaling, each instance is responsible for only a subset of the data a.k.a **data partitioning**.
- A key characteristic of the Microservice architecture is that the services are loosely coupled and communicate only via APIs. One way to achieve loose coupling is by *each service having its own datastore*. Each one can be independently developed, tested, deployed, and scaled. Also, this architecture does a good job of preserving modularity. A developer can’t bypass a service’s API and access its internal components.
- SOA applications typically use heavyweight technologies such as SOAP and other WS* standards. They often use an ESB, a smart pipe that contains business and message-processing logic to integrate the services.
- Benefits of the Microservice architecture:
  - Enables the continuous delivery and deployment of large, complex applications.
    - It has the testability required by continuous delivery/deployment.
    - It has the deployability required by continuous delivery/deployment.
    - It enables development teams to be autonomous and loosely coupled.
  - Each service is small and easily maintained.
  - Each service is independently scalable.
  - Better fault isolation
  - Easily experimenting and adoption of new technologies.
  - Enables teams to be autonomous.
- Drawbacks of the Microservice architecture:
  - Finding the right services is challenging.
  - Distributed systems are complex
  - Deploying features spanning multiple services needs careful coordination.
- Pattern language is a collection of related patterns that solve problems within a particular domain. A pattern structure includes three especially valuable sections:
  - **Forces**: The issues that you must address when solving a problem in a given context. Forces can conflict, so it might not be possible to solve all of them.
  - **Resulting context**: The consequences of applying the pattern. It consists of three parts:
    - Benefits: The benefits of the pattern, including the forces that have been resolved.
    - Drawbacks: The drawbacks of the pattern, including the unresolved forces.
    - Issues: The new problems that have been introduced by applying the pattern.
  - **Related patterns**: The related patterns section of a pattern describes the relationship between the pattern and other patterns. There are five types of relationships between patterns:
    - Predecessor: A predecessor pattern is a pattern that motivates the need for this pattern.
    - Successor: A pattern that solves an issue that has been introduced by this pattern.
    - Alternative: A pattern that provides an alternative solution to this pattern.
    - Generalization: A pattern that is a general solution to a problem.
    - Specialization: A specialized form of a particular pattern.
- Patterns for decomposing an application into services:
  - **Decompose by business capability** which organizes services around business capabilities, rather than the individual components.
  - **Decompose by subdomain** which organizes services around domain-driven design (DDD) subdomains.
- Communication patterns:
  - **Communication style**: What kind of IPC mechanism should you use?
  - **Discovery**: How does a client of a service determine the IP address of a service instance so that, for example, it makes an HTTP request?
  - **Reliability**: How can you ensure that communication between services is reliable even though services can be unavailable?
  - **Transactional messaging**: How should you integrate the sending of messages and publishing of events with database transactions that update business data?
  - **External API**: How do clients of your application communicate with the services?
- Observability patterns:
  - **Health check API**: Expose an endpoint that returns the health of the service.
  - **Log aggregation**: Log service activity and write logs into a centralized logging server, which provides searching and alerting.
  - **Distributed tracing**: Assign each external request a unique ID and trace requests as they flow between services.
  - **Exception tracking: Report exceptions to an exception tracking service, which deduplicates exceptions, alerts developers, and tracks the resolution of each exception**.
  - **Metrics**: Collect and report metrics about service activity.
  - **Audit logging**: Log user actions.

> Continuous Delivery is the ability to get changes of all types—including new features, configuration changes, bug fixes and experiments—into production, or into the hands of users, safely and quickly in a sustainable way.

## Chapter 2: Decomposition Strategies

> The software architecture of a computing system is the set of structures needed to reason about the system, which comprise software elements, relations among them, and properties of both.

- The **4+1 view** model of software architecture:
  - *Logical* view: The software elements that are created by developers. In object-oriented languages, these elements are classes and packages. The relations between them are the relationships between classes and packages, including inheritance, associations, and depends-on.
  - *Implementation* view: The output of the build system. This view consists of modules, which represent packaged code, and components, which are executable or deployable units consisting of one or more modules.
  - *Process* view: The components at runtime. Each element is a process, and the relations between processes represent interprocess communication.
  - *Deployment* view: How the processes are mapped to machines. The elements in this view consist of (physical or virtual) machines and the processes. The relations between machines represent networking.
  - Additionally, there are the *scenarios*—the +1 in the 4+1 model— that animate views. Each scenario describes how the various architectural components within a particular view collaborate in order to handle a request.
- An application has two categories of requirements:
  - **Functional requirements**: Which define what the application must do. They’re usually in the form of use cases or user stories.
  - **Quality of service requirements a.k.a non-functional requirements**: Which define the runtime qualities such as scalability and reliability. They also define development time qualities including maintainability, testability, and deployability.

> An architectural style, then, defines a family of such systems in terms of a pattern of structural organization. More specifically, an architectural style determines the vocabulary of components and connectors that can be used in instances of that style, together with a set of constraints on how they can be combined.

- A **layered architecture** organizes software elements into layers. Each layer has a well-defined set of responsibilities. A layered architecture also constraints the dependencies between the layers. *A layer can only depend on either the layer immediately below it (if strict layering) or any of the layers below it*.
- Three-tier architecture is the layered architecture applied to the logical view. It organizes the application’s classes into the following tiers or layers:
  - **Presentation layer**: Contains code that implements the user interface or external APIs.
  - **Business logic layer**: Contains the business logic.
  - **Persistence layer**: Implements the logic of interacting with the database.
- Drawbacks of the layered architecture style:
  - **Single presentation layer**: It doesn’t represent the fact that an application is likely to be invoked by more than just a single system.
  - **Single persistence layer**: It doesn’t represent the fact that an application is likely to interact with more than just a single database.
  - **Defines the business logic layer as depending on the persistence layer**: In theory, this dependency prevents testing the business logic without the database.
- The hexagonal architecture style organizes the logical view in a way that *places the business logic at the center*.
  - **Inbound adapters**: Instead of the presentation layer, the application has one or more inbound adapters that handle requests from the outside by invoking the business logic.
  - **Outbound adapters**: Instead of a data persistence tier, the application has one or more outbound adapters that are invoked by the business logic and invoke external applications.
  - A key characteristic and benefit of this architecture is that the business logic doesn’t depend on the adapters. Instead, they depend upon it.
  - **The business logic has one or more ports**. A port defines a set of operations and is how the business logic interacts with what’s outside of it.
- The microservice architecture is an architectural style. It structures the implementation view as a set of multiple components: executables or WAR files. The components are services, and the connectors are the communication protocols that enable those services to collaborate. Each service has its own logical view architecture, which is typically a hexagonal architecture.
- A service is a standalone, independently deployable software component that implements some useful functionality. A service’s API encapsulates its internal implementation. Unlike in a monolith, a developer can’t write code that bypasses its API. As a result, the microservice architecture enforces the application’s modularity.
- A system operation is an abstraction of a request that the application must handle. It’s either a command, which updates data, or a query, which retrieves data. The behavior of each command is defined in terms of an abstract domain model, which is also derived from the requirements. The system operations become the architectural scenarios that illustrate how the services collaborate.
- Decomposition obstacles:
  - Network latency.
  - Synchronous communication between services reduces availability.
  - Maintain data consistency across services.
  - Obtaining a consistent view of the data.
  - God classes.
- A three-step process for defining an application’s microservice architecture:
  - **Determine system operations**.
    - Sketch a high-level domain model for the application.
    - Identify the requests that the application must handle. There are two types of system operations:
      - Commands: System operations that create, update, and delete data.
      - Queries: System operations that read (query) data.
  - **Determine the decomposition into services**.
    - Defining services by applying the Decompose by **business capability pattern**
    - Defining services by applying the Decompose by **sub-domain pattern**
  - **Determine each service’s APIs**.
- Responsibility principle (SRP): A class should have only one reason to change.
- Common closure principle (CCP): The classes in a package should be closed together against the same kinds of changes. A change that affects a package affects all the classes in that package.
- The starting point for defining the service APIs is to map each system operation to a service. After that, we decide whether a service needs to collaborate with others to implement a system operation. If collaboration is required, we then determine what APIs those other services must provide in order to support the collaboration.
- Defining service APIs:
  - Assigning system operations to services.
  - Determine the APIs required to support collaboration between services.

## Chapter 3: Interprocess Communication

- Services can use **synchronous request/response-based communication** mechanisms, such as HTTP-based REST or gRPC. Alternatively, they can use **asynchronous, message-based communication** mechanisms such as AMQP or STOMP. There are also a variety of different messages formats. Services can use human-readable, text-based formats such as JSON or XML. Alternatively, they could use a more efficient binary format such as Avro or Protocol Buffers.
- There are a variety of client-service interaction styles:
  - The first dimension is whether the interaction is one-to-one or one-to-many
    - One-to-one: Each client request is processed by exactly one service.
      - Request/response: A service client makes a request to a service and waits for a response.
      - Asynchronous request/response: A service client sends a request to a service, which replies asynchronously.
      - One-way notifications: A service client sends a request to a service, but no reply is expected or sent.
    - One-to-many: Each request is processed by multiple services.
      - Publish/subscribe: A client publishes a notification message, which is consumed by zero or more interested services.
      - Publish/async responses: A client publishes a request message and then waits for a certain amount of time for responses from interested services.
  - The second dimension is whether the interaction is synchronous or asynchronous:
    - Synchronous: The client waits for the service to respond before it can send another request.
    - Asynchronous: The client sends requests to the service as soon as it receives them.
- The Semantic Versioning specification (Semvers) requires a version number to consist of three parts: `MAJOR.MINOR.PATCH`:
  - MAJOR: When you make an incompatible change to the API.
  - MINOR: When you make backward-compatible enhancements to the API.
  - PATCH: When you make a backward-compatible bug fix.
- The text-based formats such as JSON and XML have an advantage that not only are they human readable, they’re self describing. A JSON message is a collection of named properties. Similarly, an XML message is effectively a collection of named elements and values. This format enables a consumer of a message to pick out the values of interest and ignore the rest. Consequently, many changes to the message schema can easily be backward-compatible.
- The binary formats like the Protocol Buffers and Avro. Both formats provide a typed IDL for defining the structure of your messages. A compiler then generates the code that serializes and deserializes the messages.

> REST provides a set of architectural constraints that, when applied as a whole, emphasizes scalability of component interactions, generality of interfaces, independent deployment of components, and intermediary components to reduce interaction latency, enforce security, and encapsulate legacy systems.

- REST APIs challenges:
  - Fetching multiple resources in a single request.
  - Mapping operations to HTTP verbs.
- gRPC is a binary message-based protocol, and this means that you’re forced to take an API-first approach to service design. You define your gRPC APIs using a Protocol Buffers-based IDL, which is Google’s language neutral mechanism for serializing structured data. You use the Protocol Buffer compiler to generate client-side stubs and server-side skeletons.
- Circuit breaker pattern is an RPI proxy that immediately rejects invocations for a timeout period after the number of consecutive failures exceeds a specified threshold.
- Robust RPI proxies:
  - **Network timeouts**: Never block indefinitely and always use timeouts when wait- ing for a response. Using timeouts ensures that resources are never tied up indefinitely.
  - **Limiting the number of outstanding requests from a client to a service**: Impose an upper bound on the number of outstanding requests that a client can make to a particular service. If the limit has been reached, it’s probably pointless to make additional requests, and those attempts should fail immediately.
  - **Circuit breaker pattern**: Track the number of successful and failed requests, and if the error rate exceeds some threshold, trip the circuit breaker so that further attempts fail immediately. A large number of requests failing suggests that the service is unavailable and that sending more requests is pointless. After a timeout period, the client should try again, and, if successful, close the circuit breaker.
- The service discovery mechanism updates the service registry when service instances start and stop. When a client invokes a service, the service discovery mechanism queries the service registry to obtain a list of available service instances and routes the request to one of them.
There are two main ways to implement service discovery:
  - The services and their clients interact directly with the service registry.
  - The deployment infrastructure handles service discovery.
- **3rd party registration**: Service instances are automatically registered with the service registry by a third party.
- **Server-side discovery**: A client makes a request to a router, which is responsible for service discovery.
- Messages types:
  - **Document**: A generic message that contains only data. The receiver decides how to interpret it. The reply to a command is an example of a document message.
  - **Command**: A message that’s the equivalent of an RPC request. It specifies the operation to invoke and its parameters.
  - **Event**: A message indicating that something notable has occurred in the sender. An event is often a domain event, which represents a state change of a domain object.
- Messaging channels types:
  - A **point-to-point** channel delivers a message to exactly one of the consumers that is reading from the channel. Services use point-to-point channels for the one-to-one interaction styles.
  - A **publish-subscribe** channel delivers each message to all of the attached consumers. Services use publish-subscribe channels for the one-to-many interaction styles.
- The broker-less messaging architecture benefits:
  - Allows lighter network traffic and better latency, because messages go directly from the sender to the receiver, instead of having to go from the sender to the message broker and from there to the receiver
  - Eliminates the possibility of the message broker being a performance bottleneck or a single point of failure.
  - Features less operational complexity, because there is no message broker to set up and maintain.
- The broker-less messaging architecture challenges:
  - Services need to know about each other’s locations and must therefore use one of the discovery mechanisms describer.
  - It offers reduced availability, because both the sender and receiver of a message must be available while the message is being exchanged.
  - Implementing mechanisms, such as guaranteed delivery, is more challenging.
- The broker-based messaging architecture benefits:
  - Loose coupling: A client makes a request by simply sending a message to the appropriate channel. The client is completely unaware of the service instances. 
  - Message buffering: The message broker buffers messages until they can be processed.
  - Flexible communication: Messaging supports all the interaction styles.
  - Explicit interprocess communication: RPC-based mechanism attempts to make invoking a remote service look the same as calling a local service.
- The broker-based messaging architecture challenges:
  - Potential performance bottleneck—There is a risk that the message broker could be a performance bottleneck. Fortunately, many modern message brokers are designed to be highly scalable.
  - Potential single point of failure—It’s essential that the message broker is highly available—otherwise, system reliability will be impacted. Fortunately, most modern brokers have been designed to be highly available.
  - Additional operational complexity—The messaging system is yet another system component that must be installed, configured, and operated.
- Handling at-least-once messages delivery:
  - Write idempotent message handlers: If the application logic that processes messages is idempotent, then duplicate messages are harmless. Application logic is idempotent if calling it multiple times with the same input values has no additional effect. For instance, cancelling an already-cancelled order is an idempotent operation
  - Track messages and discard duplicates: Track the messages that it has processed using the message id and discard any duplicates.
- Transactional outbox pattern: Uses a database table as a temporary message queue by inserting it into an *OUTBOX* table as part of the transaction that updates the database. The Message Relay reads the *OUTBOX* table and publishes the messages to a message broker. The *OUTBOX* table acts a temporary message queue. The *MessageRelay* is a component that reads the *OUTBOX* table and publishes the messages to a message broker.
- Transaction log tailing: Tails the database transaction log and publish each message/event inserted into the *OUTBOX* to the message broker.
