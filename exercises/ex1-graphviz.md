# Exercise One: Domain Driven Design and Event Sourcing
- Technology Used: GraphViz
- Time needed to complete: 1hr
- Exercise best done with a partner/group

## Overview
Consider this a one-hour crash course on Domain Driven Design, CQRS and Eventsourcing.  There is no way one exercise can give you all of what you need to know.  My hope here is to share with you a few notes to help accelerate your learning and perhaps give you some incentive to do further research.  More importantly, we are moving more rapidly toward this approach as we evolve our software and these topics are becoming increasingly more relevant to us in our journey to build a cloud-ready service offerings.

## Things to do first:

1. Read [this article on Domain Driven Design and EventStorming](https://blog.redelastic.com/corporate-arts-crafts-modelling-reactive-systems-with-event-storming-73c6236f5dd7)

2.  Make sure GraphViz is installed.  Instructions can be found [here](doc/GraphViz).


## Start to learn the terminology

### Domain Driven Design (DDD) (Evans, Vernon)
Domain-driven design (DDD) is an approach to developing software for complex needs by deeply connecting the implementation to an evolving model of the core business concepts.
 
Its premise is:
- Place the project’s primary focus on the core domain and domain logic. 
- Base complex designs on a model
- Initiate a creative collaboration between technical and domain experts to iteratively cut ever closer to the conceptual heart of the problem.

The premise is simple, but pulling it off in the messy real world is hard. It calls for new skills and discipline, and a systematic approach.

Domain-driven design is not a technology or a methodology. DDD provides a structure of practices andterminology for making design decisions that focus and accelerate software projects dealing with complicated domains.
###### [Source: DDDCommunity](http://dddcommunity.org/learning-ddd/what_is_ddd/)

### Utility of a Model in Domain Driven Design (Evans)

1. **The model and the heart of the design shape each other.**
   
   It is the intimate link between the model and the implementation that makes the model relevant and ensures that the analysis that went into it applies to the final product, a running program. This binding of model and implementation also helps during maintenance and continuing development,because the code can be interpreted based on understanding the model.

2. **The model is the backbone of a language used by all team members. It uses the practitioners words. It is opinionated.** 

   Because of the binding of model and implementation, developers can talk about the program in this language. They can communicate with domain experts without translation. And because the language is based on the model, our natural linguistic abilities can be turned to refining the model itself.

3. **The model is distilled knowledge.**
   
   The model is the team's agreed-upon way of structuring domain knowledge and distinguishing the elements of most interest. A model captures how we choose to think about the domain as we select terms, break down concepts, and relate them. The shared language allows developers and domain experts to collaborate effectively as they wrestle information into this form. The binding of model and implementation makes experience with early versions of the software applicable as feedback into the modeling process.

###### [Source: Eric Evans, "Domain-Driven Design: Tackling Complexity in the Heart of Software](https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)

### Ingredients of Effective Modeling  (take-aways from Eric Evan's book on DDD)

1. Develop a reference implementation early -- binding the model and the implementation. Even a crude prototype can forge the essential links early,  They may even be maintained through all subsequent iterations. Fail fast and move on, progress is better than perfection. 

2. Build a common language based on your model. A ubiquitous language adds clarity to conversations. 

    > #### What is a Ubiquitous Language?

    >It is the idea that the design of the software should come from the shared language on a team. "One Team, One Language". For political reasons it may not be possible for everyone in an enterprise to speak the same ubiquitous language. However, within a bounded context (explanation later) it is necessary. Technical people should not shield business experts from the domain model. Some technical components of the design may not concern the domain experts, but the core of the model had better interest them. The technical staff should take their modeling queues from the business experts. Developers should explain their design decisions to business experts. That way, developers can tell from expressions and feedback if the concepts will be adopted into the ubiquitous language. If not, even if the design is elegant, it should be redesigned to correspond with the language the entire team actually uses.

3. Develop a knowledge-rich model.
   
   The objects have behavior and enforce rules. The model isn't just a data schema; it was is integral to solving a complex problem. It captures knowledge of various kinds.


4. Distill the model.
   
   Important concepts get added to the model as it becomes more complete, but equally important, concepts are dropped when they don't prove useful or central. When an unneeded concept is tied to one that was needed, find a new model that distinguishes the essential concept so that the other can be dropped.

5. Brainstorming and experimenting. 
   
   The language, combined with sketches and a brainstorming attitude, turn discussions into laboratories of the model.  We then can conduct hundreds of experimental variations that can be exercised, tried, and judged. 

### What is CQRS?
CQRS stands for Command Query Responsibility Segregation. Greg Young is known as the originator of this term. At its heart is the notion that you can use a different model to update information than the model you use to read information. 

The change that CQRS introduces is to split that conceptual model into separate models for update and display, which it refers to as Command and Query respectively following the vocabulary of CommandQuerySeparation. The rationale is that for many problems, particularly in more complicated domains, having the same conceptual model for commands and queries leads to a more complex model that does neither well.

By separate models we most commonly mean different object models, probably running in different logical processes, perhaps on separate hardware. A web example would see a user looking at a web page that's rendered using the query model. If they initiate a change that change is routed to the separate command model for processing, the resulting change is communicated to the query model to render the updated state.

CQRS naturally fits with some other architectural patterns.

- As we move away from a single representation that we interact with via CRUD, we can easily move to a task-based UI.
- CQRS fits well with event-based programming models. It's common to see CQRS system split into separate services communicating with Event Collaboration. This allows these services to easily take advantage of Event Sourcing.
- Having separate models raises questions about how hard to keep those models consistent, which raises the likelihood of using eventual consistency.
- For many domains, much of the logic is needed when you're updating, so it may make sense to use EagerReadDerivation to simplify your query-side models.
- If the write model generates events for all updates, you can structure read models as EventPosters, allowing them to be MemoryImages and thus avoiding a lot of database interactions.
- CQRS is suited to complex domains, the kind that also benefit from Domain-Driven Design.

###### [Source: Martin Fowler](https://martinfowler.com/bliki/CQRS.html)

### Aggregates
Aggregate is a pattern in Domain-Driven Design. A DDD aggregate is a cluster of domain objects that can be treated as a single unit. An example may be an order and its line-items, these will be separate objects, but it's useful to treat the order (together with its line items) as a single aggregate.

An aggregate will have one of its component objects be the aggregate root. Any references from outside the aggregate should only go to the aggregate root. The root can thus ensure the integrity of the aggregate as a whole.

> In Lagom, we would  call this a Persistent Entity (or a Persistent Actor in Akka terms)

Aggregates are the basic element of transfer of data storage - you request to load or save whole aggregates. Transactions should not cross aggregate boundaries.

> IMPORTANT NOTE 
**<p>DDD Aggregates are sometimes confused with collection classes (lists, maps, etc). DDD aggregates are domain concepts (order, clinic visit, playlist), while collections are generic. An aggregate will often contain mutliple collections, together with simple fields. The term "aggregate" is a common one, and is used in various different contexts (e.g. UML), in which case it does not refer to the same concept as a DDD aggregate.</p>**

###### [Source: Martin Fowler](https://martinfowler.com/bliki/CQRS.html)

### What is Eventsourcing?

The traditional way to persist an entity is to save its current state. Event sourcing uses a radically different, event-centric approach to persistence. A business object is persisted by storing a sequence of state changing events. Whenever an object’s state changes, a new event is appended to the sequence of events. Since that is one operation it is inherently atomic. A entity’s current state is reconstructed by replaying its events.

There are two features of aggregates that are relevant to events and event sourcing:
- Aggregates define consistency boundaries for groups of related entities; therefore, you can use an event raised by an aggregate to notify interested parties that a transaction (consistent set of updates) has taken place on that group of entities.
- Every aggregate has a unique ID; therefore, you can use that ID to record which aggregate in the system was the source of a particular event.

Event sourcing is a way of persisting your application's state by storing the history that determines the current state of your application. For example, a conference management system needs to track the number of completed bookings for a conference so it can check whether there are still seats available when someone tries to make a new booking. The system could store the total number of bookings for a conference in two ways:

- It could store the total number of bookings for a particular conference and adjust this number whenever someone makes or cancels a booking. You can think of the number of bookings as being an integer value stored in a specific column of a table that has a row for each conference in the system.
- It could store all the booking and cancellation events for each conference and then calculate the current number of bookings by replaying the events associated with the conference for which you wanted to check the current total number of bookings.

### Events:  Understanding the Verbs
Events are the things that happened -- past tense -- they are immutable facts. They represent a trail of crumbs that are left behind a process.  In a system that uses [eventsourcing](link), events are appended to a write-only journal. These events (past tense verbs) capture state changes in the entities (the nouns).  

### The Journal as an Event Ledger:  Internal Accounting of Events
This journal is a ledger of sorts.  It records everything that happened (that the particular entity cares about).  We represent all state changes as events and those immutable facts are appended to this ledger, just as an accountant makes journal entries. To recreate the current state of an entity when it is started we replay these events.

### The Nouns:  Aggregates and Peristent Entities

The nouns in our system are the domain objects.  Many of us start with these guys when we want to build a system.  We'll likely create some ERD diagram, develop a schema and then translate that into a database design using an ORM.

### Events, Commands and Changes in State

A persistent entity (aggregate root) has a stable identifier and for a given id there will only be one instance of the entity. These entities can make up the "nouns" in your service. If you know the identifier you can send messages, so called **commands**, to the entity.

A common type of command is a query command.  This is a **read-side request** to the Service that is consumed by a persistent entity.  When the persistent entity takes the command, it will validate it and then decide how to respond to the query.  Inside the entity, the external command/query is transformed into an internal command that drives behavior of the service.

### What is a bounded context?

Bounded contexts are used to create resiliency in complex systems. Like bulkheads in a ship hull that prevents a breach from sinking the entire ship, a bulkhead in a system prevents unnecessary complexity from leaking outside the contextual boundary.

I like to think of a bounded context as the "boundary" for my microservice.  Microservices should share nothing with the outside world other than the events they emit and the payloads they provide when queried directly.  These boundaries give us the isolation we which facilitate a loosely coupled design where services can be deployed anywhere (location transparency).

---
## Background for Exercise

Take a moment a consider the events that happen behind the scenes in a system that uses taylor series approximation to provide near real time valuation of a portfolio of interest rate swaps.  In this exercise, we will use domain driven design to begin to design a system that can support ticking valuations of this portfolio.  Along the way, we will learn the language of Domain Driven Design.

In this exercise, you will create a context map that demonstrates communication between Bounded Contexts in the above example.  We'll develop this map using GraphViz and it will serve as a way to identify the services that we would need in our system to support the above use case.

### Exercise
**STEP ONE:** 

On a piece of paper, write a timeline starting at 0ms.  List the events that occur in the system along this timeline.  What you should find is that you have clumps of events.  If you think about it, that makes sense.  These "clumps" of events likely occur in the same context -- the bounded context that will define our services.

**STEP TWO:**

The next thing we'll do is take the events we listed in Step One and 
determine how the system needs to react to these events.  What are the entities that care about the events?  When they react to the event what happens? Do they change state?  Do they emit new events?

The reactions to these events are represented are then converted inside the entity (aggregate) into internal commands. These commands in turn, potentially trigger state changes, which in turn create more events.

At this point, we should have a high level view of the required services to support the use case. 

**STEP THREE:**

Pick one bounded context and create a context diagram that shows the events that the bounded context (think microservice) will need to subscribe to, as well as the commands and queries that the service can accept.

**STEP FOUR:**

Using graphviz, take your work from STEP THREE AND create a context diagram that shows the events that occur in a system that uses taylor series approximation to provide near real time valuation of a portfolio of interest rate swaps.

Sample Events to Consider:

- yieldCurveUpdated

- valuationChanged

- tradeExecuted

- quoteTicked

When considering the above events, think about the information that the event should carry with it.  What do the consumers of the event need to do their job(s)?

#### Note on Context Maps
> The context map is responsible for defining an explicit boundary between bounded contexts and makes sure they have a right contact point. A context map should be simple enough to be understood by the domain expert and the technical team. The more important issue you should care about is that the context map should not show the detail of a model; instead, they must demonstrate the integration points and the flow of data between bounded contexts.

> Anyone can do a context map.  The context map is independent of the technology that you use to satisfy the requirement.  The map is a valuable tool to show the relationship between different parts of the system. A context map ensures enabling technical teams to have the best possible chance of overcoming issues early and to avoid accidentally weakening the usefulness of the models by violating their integrity.

> In the end, we will have several context maps.  Some context maps will cover the interactions inside the bounded context and others will show the relationships between different bounded contexts.


