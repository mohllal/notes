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