# Exercise II:  Lagom

##### Note:  This document is largely a summary of the [Lagom Documentation]().  Any mistakes are mine.
## Background

### What is Lagom? 

Lagom is a Swedish word meaning “just the right amount”.

Microservices arent about size, they are about alignment with business capabilites and requirements to support isolation and resiliency. A system of right-sized microservices will help us achieve scalability and resiliency.


### What happened to all of the Akka talk?

When we started the reactive launch, we operated under the assumption that we wouldn't want to be restricted by a framework.  We wanted to take a toolkit (Akka) and use it as the middleware to support microservices.

What we found was that we were burning time building infrastructure and were spending less time solving problems in the business domain.  We quickly found ourselves trying to generalize problems. We recognized that we could go down the slippery path of writing a microservices framework if we werent careful.

There were a number of things that we wanted:
- Separation of reads from writes in our services.
- Easy ability to deploy in the cloud (Kubernetes/Docker support)
- Enhanced productivity for developers
- Support for streaming data as well as traditional rest requests
- Telemetry - Ability to log how the system performs and how it is used

### How is Akka related to Lagom?

Lagom is a framework that uses Akka and Play under the covers.  Think of it as an opinionated approach to building microservices which is based entirely on Akka.  Its the tools that developers would build themselves to use Akka effectively in an eventsourcing/CQRS design. 

## Why we chose Lagom for the reactive launch:

We chose Lagom because it includes libraries and a development environment that increases productivity and lets us focus on solving business problems

- A single command builds our project and starts all of our services
- When developing, Lagom spins up Kafka (messaging) and Cassandra (persistence) for us -- we have to do nothing
- Service location, communication protocols, and other issues are handled by transparently, maximizing convenience and productivity.  
- We can use Java or Scala, or both.  
- Lagom supports Event sourcing and CQRS (Command Query Responsibility Segregation) for persistence.
- Lagom creates service clients for you (based on service descriptors), making it extremely easy to invoke and test endpoints.
- Cloud Ready.  Kubernetes support.  Kubernetes is the leading an open-source solution for container orchestration.


## Resources

Videos from Jonas Boner

#### Microservices Blah, Blah, Blah
[Longer Version of the Above](https://www.youtube.com/watch?v=DRK7WYNh6AA)



Lagom Promotional Videos:

- [Meet Lagom](https://youtu.be/d1jT0UOVx9U)
  - Marketing piece for Lagom, but does a good job at highlighting the benefits of using a more opinionated framework to increase productivity
  - Runtime:  2 MIN

- [Introduction to Lagom](https://youtu.be/D9y2Ex3NN34)
  - Services are asynchronous. Intra-service communication is managed for you. Streaming is out of the box. Your microservices are resilient by nature.
  - This is a walkthough of a twitter-clone application written in JAVA using Lagom
  - Runtime: 10 MIN
  
- [Developer Setup](https://youtu.be/1cOqYMe-Zm0)
  - Walkthough of setting up your IDE for Lagom
  - Runtime: 11 MIN

- [Managing Data Persistence](https://youtu.be/yj581pSRflQ)
  - Systems are distributed. Akka Cluster and Akka Persistence are used under the hood to a create distributed system. CQRS and event sourcing patterns are fully supported. 
   - Runtime: 16 MINS
