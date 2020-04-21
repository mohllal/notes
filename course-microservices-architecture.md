# Microservices Architecture

By: Rag Dhiman. [Purchase the book](https://www.pluralsight.com/courses/microservices-architecture)!

Date Started: Tuesday, April 7, 2020

Date Finished: ongoing

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
