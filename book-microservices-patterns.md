# Microservices Patterns

By: Chris Richardson.
[Book Link](https://www.amazon.com/Microservices-Patterns-examples-Chris-Richardson/dp/1617294543).

Date Started: Friday, April 1, 2022
Date Finished: TBD

## Chapter 1: Escaping Monolithic Hell

- Bnefits of the monolithic architecture:
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
  - Locked into osbolete technology stack.
- Adrian Cockcroft, formerly of Netflix, defines a microservice architecture as a service-oriented architecture composed of loosely coupled elements that have bounded contexts.
- The 3D scale cube:
  - x-axis scaling load balances requests accross multiple instances a.k.a **horizontal duplication**.
  - y-axis scaling decomposes an application into services a.k.a **functional decomposion**.
  - z-axis scaling runs multiple instances of the monolith application, but unlike X-axis scaling, each instance is responsible for only a subset of the data a.k.a **data partitioning**.
- A key characteristic of the microservice architecture is that the services are loosely coupled and communicate only via APIs. One way to achieve loose coupling is by *each service having its own datastore*. Each one can be independently developed, tested, deployed, and scaled. Also, this architecture does a good job of preserving modularity. A developer can’t bypass a service’s API and access its internal components.
- SOA applications typically use heavyweight technologies such as SOAP and other WS* stan- dards. They often use an ESB, a smart pipe that contains business and message-processing logic to integrate the services.
- Benefits of the microservice architecture:
  - Enables the continuous delivery and deployment of large, complex applications.
    - It has the testability required by continuous delivery/deployment.
    - It has the deployability required by continuous delivery/deployment.
    - It enables development teams to be autonomous and loosely coupled.
  - Each service is small and easily maintained.
  - Each service is independently scalable.
  - Better fault isolation
  - Easily experimenting and adoption of new technologies.
  - Enables teams to be autonomous.
- Drawbacks of the microservice architecture:
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
