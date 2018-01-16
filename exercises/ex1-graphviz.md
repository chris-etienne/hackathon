# Exercise One: Domain Driven Design and Event Sourcing
##### Technology Used: GraphViz

Take a moment a consider the events that happen behind the scenes in a system that uses taylor series approximation to provide near real time valuation of a portfolio of interest rate swaps.

#### Events:  Understanding the Verbs
Events are the things that happened -- past tense -- they are immutable facts. They represent a trail of crumbs that are left behind a process.  In a system that uses [eventsourcing](link), events are appended to a write-only journal. These events (past tense verbs) capture state changes in the entities (the nouns).  

#### The Journal as an Event Ledger:  Internal Accounting of Events
This journal is a ledger of sorts.  It records everything that happened (that the particular entity cares about).  We represent all state changes as events and those immutable facts are appended to this ledger, just as an accountant makes journal entries. To recreate the current state of an entity when it is started we replay these events.

#### The Nouns:  Aggregates and Peristent Entities

The nouns in our system are the domain objects.  Many of us start with these guys when we want to build a system.  We'll likely create some ERD diagram, develop a schema and then translate that into a database design using an ORM.

When we use eventsourcing and CQRS

##### Excepts from From Martin Fowler:
Aggregate is a pattern in Domain-Driven Design. A DDD aggregate is a cluster of domain objects that can be treated as a single unit. An example may be an order and its line-items, these will be separate objects, but it's useful to treat the order (together with its line items) as a single aggregate.

An aggregate will have one of its component objects be the aggregate root. Any references from outside the aggregate should only go to the aggregate root. The root can thus ensure the integrity of the aggregate as a whole.

> In Lagom, we would  call this a Persistent Entity (or a Persistent Actor in Akka terms)

Aggregates are the basic element of transfer of data storage - you request to load or save whole aggregates. Transactions should not cross aggregate boundaries.

> IMPORTANT NOTE 
**<p>DDD Aggregates are sometimes confused with collection classes (lists, maps, etc). DDD aggregates are domain concepts (order, clinic visit, playlist), while collections are generic. An aggregate will often contain mutliple collections, together with simple fields. The term "aggregate" is a common one, and is used in various different contexts (e.g. UML), in which case it does not refer to the same concept as a DDD aggregate.</p>**

#### Events, Commands and Changes in State

A persistent entity (aggregate root) has a stable identifier and for a given id there will only be one instance of the entity. These entities can make up the "nouns" in your service. If you know the identifier you can send messages, so called **commands**, to the entity.

A common type of command is a query command.  This is a **read-side request** to the Service that is consumed by a persistent entity.  When the persistent entity takes the command, it will validate it and then decide how to respond to the query.  Inside the entity, the external command/query is transformed into an internal command that drives behavior of the service.



### Exercise
**STEP ONE:** On a piece of paper, write a timeline starting at 0ms.  List the events that occur in the system along this timeline.

What you should find is that you have clumps of events.  If you think about it, that makes sense.  These "clumps" of events likely occur in the same context -- more about why this matters in a moment.



### Eventsourcing
The traditional way to persist an entity is to save its current state. Event sourcing uses a radically different, event-centric approach to persistence. A business object is persisted by storing a sequence of state changing events. Whenever an object’s state changes, a new event is appended to the sequence of events. Since that is one operation it is inherently atomic. A entity’s current state is reconstructed by replaying its events.

### Exercise

#### Step One: Eventstorming




Using graphviz, create a diagram that shows the events that occur in a system that uses taylor series approximation to provide near real time valuation of a portfolio of interest rate swaps.

Sample Events to Consider

yieldCurveUpdated
valuationChanged
tradeExecuted
quoteTicked


