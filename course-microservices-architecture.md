# Microservices Architecture

By: Rag Dhiman. [Purchase the course](https://www.pluralsight.com/courses/microservices-architecture)!

Date Started: Tuesday, April 7, 2020

Date Finished: TBD

## Section 1: Introduction

- ***Microservices architecture is basically Service-Oriented architecture done well***.
- Microservices architecture introduced a new set of additional design principles which on how to design and size a service correctly because there was no guidance  in how to size a service and what to include in a service.
- Traditional SOA *resulted in monolithic large services* and because of the size of the services, these services become unable to scale up and change in a reliable way.
- Microservices architecture *uses lightweight communication mechanism* between clients to service and service to service.
- ***Microservices*** architecture has these key principles:
  - Independent data storage.
  - Independently changeable.
  - Independently deployable.
  - Distributed transactions
  - Centralized tooling for monitoring and management.
  - Flexible technology stack.
- ***Monolithic*** architecture has these key characteristics:
  - No restriction on size.
  - Large code base which results to longer development time.
  - Challenging deployment.
  - Fixed technology stack.
  - High level of coupling.
  - Single point of failure.
- ***Microservices key benefits***:
  - Shorter development times.
  - Reliable and faster deployments.
  - Frequent updates and changes.
  - Security.
  - Increased uptime and availability.
  - Fast issue resolution.
  - High scalability and better performance.
  - Better ownership and specialized technology stacks.
- ***Microservice design principles***:
  - High cohesion.
  - Autonomous.
  - Business domain centric.
  - Resilience.
  - Observable.
  - Automation.
- Microservice must have a single focus and single responsibility, ***S***OLID principles.
- Microservice must be loosely coupled. It should honor the *contract and interfaces to other services and other clients*.
- Microservice must be always ***backward compatible***.
- Microservice is bounded context from ***DDD*** (Domain Driven Design) and it aims for high cohesion.
- Microservice must embrace failure when it happens, it might be in the form of another service isn't responding to a service, ***by degrading the functionality or use the default functionality***.
- Resilience can be implemented using multiple instances of the same microservice that *register on start-up and de-register on failure*.
- Observability ensures that we have a ***centric monitoring and logging systems***.
- ***CI/CD*** and ***testing automation*** tools are one of the fundamental requirements for Microservices architecture.

## Section 2: Microservices Design

- To make the Microservices architecture more decoupled, we could use the asynchronous communication model; Service publish events and these events are then handled by a message broker so other services which are listening out for these events can carry out the tasks.
- Open communication protocols which are technology antagonistic APIs like ***HTTP REST***.
- Microservices that talk to each other should have contracts between the services.
- ***Shared model*** of data is unlikely to change even if the internal representation is changed in the microservice itself.
- ***Avoid chatty exchanges*** between services.
- ***Avoid using shared libraries*** within microservices because a bug fix in a shared library might mean that we have to redeploy two microservices because they carry the same bug.
- ***Versioning strategy*** is important when designing a Microservices architecture application.
- In ***semantic versioning*** a version number is made of three numbers ***Major.Minor.Patch***. Incrementing the major number when *the new version is not backward compatible*. Incrementing the minor number when *the new version is backward compatible*. Incrementing the patch number when *the only change is a defect fix*.
- We have to ensure that when we introduce a new version of a microservice, it doesn't break the existing contract and the existing microservices that consume our new microservice.
- Integration tests basically test the changed microservice for input, outputs and shared models.
- CI/CD practices must be followed when designing a Microservice architecture application.
- During the design stage of our Microservices application, we need to design all microservices for all unknown failures by:
  - Degrade the functionality on failure detection.
  - Default functionality on failure detection.
- Using timeout between connected systems or services to configure the system to fail fast and recover fast.
- It's important that we consider system health in a transparent way and one way of doing this is to have a centralized monitoring that produces:
  - Monitoring data in real-time.
  - Monitoring health of the host in terms of CPU usages, memory usages, etc..
  - Expose metrics within the services like response time, timeout, etc..
  - Incident alerting.
- It's also important to have a centralized logging that help in troubleshooting code paths milestones.
- Traceable distributed transactions can be achieved by putting the correlation id which is passed from service to service.

## Section 4: Technology for Microservices

- When we ***synchronous*** communication between microservices we basically make requests and then wait for responses.
- The ***remote procedure call (RPC)*** can be used for synchronous communication. Libraries basically shield all the detail regarding the network protocols and the communication protocols. It appears like you're making a call to a local functional method but in fact you're actually calling a remote method on a remote service.
- Another technology that can be used for request-response synchronous communication is ***HTTP***. HTTP is firewall friendly therefore firewalls can be configured quite easily to let HTTP traffic through.
- ***REST*** is an open communication protocol that can be used over HTTP to ***CRUD*** the resources entities using HTTP verbs. REST also provides natural decoupling because the data returned is always in *JSON* or *XML* format which is normally different to the internal representation of that entity.
- ***HATEOS*** is basically a technique to include links to related resources in responses.
- Asynchronous communication in Microservices architecture context basically means ***event based communication***. When a service needs another service to carry out a task, instead of connecting directly to that service, the service trigger an event and services that needs to carry out that task will automatically pick that event up.
- Event based communication *mitigates the need of client and service availability*. It normally uses ***messages queuing protocols*** where the events created by services are seen as ***messages*** are stored in a ***message broker*** and the service create the messages is seen as ***publisher*** and the service that carries out tasks in form of response to these messages is seen as ***subscriber***.
- There are many vendors that provide message queuing solutions such as ***Microsoft Message Queuing (MSMQ)***, ***RabbitMQ***, and ***ATOM*** (HTTP to propagate events), etc..
- Asynchronous communication challenges:
  - Complication.
  - Reliance on message broker.
  - Visibility of transactions.
  - Managing the message queuing.
- ***Containers are another type of Virtualization***. However unlike virtual machines, they don't run an operating system within the container.
- *Containers are tend to run faster than VMs*. And they tend to boot up a lot faster than VMs because they are more lightweight.
- Cloud hosting services can make the implementation of a Microservices application a lot simpler because we can control the whole application via a portal and there is no need for physical machines or specialized stuff members in order to curry out a specific tasks.
- ***Service registry system enable our microservices to register themselves on start-up and when any microservice stop responding the service registry system de-register*** that instance of the microservice so that new client requests will not connect to that microservice that experience problems.
- There are two types of service discovery:
  - ***Client side discovery***: When the client connects directly to the service register system in order to find a microservice location to connect to.
  - ***Server side discovery***: When the client connects to a gateway service which is connected to the service registry system on behalf of the client.
- There are number of central monitoring tools out there:
  - Nagios
  - PRTG
  - New Relic
- In Microservices architecture ***the network monitoring is a key component*** specially when most transactions are distributed.
- Scaling a microservice can be done as one of two types:
  - ***Creating multiple instances*** of that service (***scaling out***).
  - ***Increase the resources*** to existing service (***scaling up***).
- Caching is a way of basically detecting that multiple calls are asking for the same thing and instead of honoring each request, the service honor one request and then cache the data in order to use the same data to satisfy all other requests.
- ***The ideal place to implement caching within a Microservices architectured system is that the API Gateway level*** as it will not only reduce the number of calls to our services and databases but it will also reduce the amount of traffic within our network.
- API gateways are ***basically the central entry point*** into our system for client application and therefore they can be used to improve performance by having load-balancing and caching functionalities.
- *API gateways can also be used to improve security* for the overall system by providing authentication and authorization.
- It is considered a good practice to have a separate code repository for each microservice with its own CI/CD build, this ways two microservices are not accidentally changed at the same time.

## Section 3: Moving Forward with Microservices

1. The first key step in migrating to Microservices architecture from Monolithic system ***is identifying seems in the system*** which is done by identifying separation within the code that reflect the domains in the application business.
    - By grouping the code that relates to a specific business domain or functionality we are basically identifying ***bounded contexts*** and having a clear boundary with clear interfaces between each module/group.
2. ***Covert bounded contexts into separate microservices*** one by one with incremental approach.
    - How to prioritize what to split?
      - By risk
      - By technology
      - By dependencies
3. *Split database by identifying seems that related to code modules*. Providing API calls that can fetch data for a relationship between different microservices.
    - we still have to worry about ***data referential integrity*** by having microservices talk and instruct each other.
4. Implement different ***strategies for handling distributed transactions failures*** and this can be done by using a transaction manager software which ***uses two-phase commit*** methodology.
5. Microservices complicate data reporting because data is split across different microservices thus we have to accept the fact that the ***data reporting will be slower***.
    - We can have a ***dedicated reporting microservice*** which basically calls other microservices to collect and consolidate the data.
    - We also use ***data pumps*** that pumps the data to a central database.

- It's important in a greenfield situation to *understand and collect the requirements* and not focus to much on what microservices are required to address the overall problem.
- ***It's best to start off with a Monolithic design*** so we start off with a high level design and we allow the seems within our system to evolve.
- Because of distributed nature in a Microservices application we have to invest more in extra testing resources in order to test latency, performance, and resilience.
