## Chapter 18: Microservices Architecture
_(Chapter 17 in the 1st Edition)_

## Introduction

There is no secret group of architects who decide what the next big movement 
will be. Rather, it turns out that manyarchitects end up making common 
decisions.

Microservices differ in this regard - it was popularized by a famous blog entry 
by Martin Fowled and James Lewis. 
Microservices favor highly decoupled, single-purpose services that includes all 
necessary parts and can operate independently.

## Topology
Microservices is a _distributed_ architecture style that is heavily inspired by 
the ideas in domain driven design (DDD) - bounded context, decidedly inspired 
microservices. Within a bounded context, the internal parts (code, data schemas) 
are coupled together to produce work, but they are never coupled to anything 
outside the bounded context.

Performance is often the negative side effect of the distributed nature of 
microservices. Network calls take much longer than method calls. It is advised
to avoid transactions across service boundaries, making determining the 
granularity the key to success in this architecture.

It is hard to define the right granularity for services in microservices. 
If there are too many services, a lot of communication will be required to 
perform work. The purpose of service boundaries is to capture a domain or 
workflow.

## Style Specifics

### Bounded Context
Every microservice models a particular function, subdomain, or workflow. Each 
bounded condtext includes everything needed for said microservice (code, 
databases, schemas, etc.). 

### Granularity
_"The term microservice is a label, not a description." - Martin Fowler_
Effective use of microservices requires the architect to determine what is the 
"correct level of granularity". Often microservices can be _too_ small,
so it is important to specify the **purpose**, **transactions**, and
 **choreography** of each microservice.
- Purpose - a domain, one significant behaviour on behalf of the overall 
application
- Transactions - often the entities that need to cooperate in a transaction 
show a good service boundary
- Choreography - if excellent domain isolation require extensive communication, 
you may consider merging services back into larger service to avoid
communication overhead.

### Data Isolation
Microservices Architecture tries to avoid all kinds of coupling - 
including shared schemas and databases used as integration points. 
This means using a single database or common data source should also be avoided.
**Avoid coupling wherever possible.**

### API Layer
The API layer (or gateway) handles request routing, either using user 
interfaces, calls from other systems, or other services within the greater 
system. API layers should contain **no** business logic and simply be used for 
request routing and cross-cutting concerns such as security, monitoring, 
logging, etc.

### Operational Reuse
Once a team has built several microservices, they realize that each has common 
elements that benefit from similarity. The shared sidecar can be either owned by
individual teams or a shared infrastructure team. Once teams know that each
service includes a common sidecar, they can build a _service mesh_ - 
allowing unified control across the infrastructure concerns like logging and 
monitoring.

## Frontends
- Monolithic User Interface - a single UI that calls through the API layer to 
satisfy user request
- Micro-Frontends - each service emits the UI for that service, which the 
frontend coordinates with the other emitted UI components

## Communication
Microservices architectures typically utilize 
_protocol-aware heterogeneous interoperability_:

- Protocol-Aware - each service should know how to call other services
- Heterogeneous - each service may be written in a different technology stack, 
heterogeneous means that microservices fully support polyglot environments
- Interoperability - describes services calling one another, while architects 
in microservices try to discourage transactional calls, services commonly call 
other services via the network to collaborate

For asynchronous communication, architects often use events and messages 
(internally utilizing an event-driven architecture).

## Choreogrpahy and Orchestration

The broker and mediator patterns manifest as choreography and orchestration:

- **Choreography** - no central coordinator exists in this architecture
- **Orchestration** - coordinating calls across several services

## Transactions and Sagas

Do not design transactions across service boundaries, as this violates the core 
decoupling principle of the microservices architecture.

> Don't do transactions in microservices - fix granularity instead.

Exceptions always exist (e.g. 2 different services need vastly different 
architecture characteristics -> different
boundaries), in such situations - patterns exist to handle transaction 
orchestration (with serious trade-offs).

**SAGA** - the mediator calls each part of the transaction, records 
success/failure, and coordinates results. In case of an error, the mediator 
must ensure that no part of the transaction succeeds if one part fails 
(e.g. send a request to undo - usually very complex). 
Typically, implemented by having each request in a `pending` state.

> A few transactions across services is sometimes necessary; if it is the 
> dominant feature of the architecture, mistakes were made!

## Data Topologies

### Monolithic Database Issues
A single shared database across multiple services can create bottlenecks for 
schema changes, breaks bounded context, limits scalability & elasticity, and 
also can introduce risk for the system if the database goes down.

### Database-per-Service Pattern
- Each service owns its own database or schema.
- Maintains integrity of bounded context and simplifies change control.
- Improves scalability, elasticity, and fault tolerance.
  - i.e. a pool of databases results in a down database not causing the whole 
  system to crash.
#### _Limited Sharing within a Context_
If multiple services form a broader bounded context, a handful of services 
can share a database. However, this should be limited to no more than 
5 or 6 services. This can reduce the amount of seperate stores while still 
preserving domain boundaries. This introduces some trade-offs from 
monolithic databases such as coordination for schema changes.

## Cloud Considerations
The Microservices Architecture is very well suited for cloud-based deployments 
which often use orchestration platforms such as kubernetes and cloud foundry. 
Note, while _serverless_ deployments are often viewed as a architectural style,
this book makes the argument that is rather a deployment model.

## Common Risks
- Performance is often an issue in microservices.
  - if too fine-grained there can be too many network calls, which has high 
  performance overhead and is less maintainable. 
- Excessive inter-service communication increases dynamic coupling.
- Overuse of data sharing reduces agility and fault tolerance.
- Sharing libraries can break context boundaries and complicate governance.

## Governance
Governance needs to be applied by the architect to monitor and control the 
amount of static and dynamic coupling between services. 
- Static Coupling - Driven by things such as software build of materials,
 deployment scripts, dependency-management tools, etc.
- Dynamic Coupling - Much more complicated to collect metrics on, but can be 
captured using consistent logging or registry entries to map inter-service calls. 
Use _fitness function_ to detect and limit excessive runtime dependencies. 

## Team Topology Considerations
- Stream-Aligned Teams - Best when team organization marches bounded contexts.
- Enabling Teams - Provide shared services and tools without blocking feature 
teams.
- Complicated-Subsystems - Focus on subdomain implementations within dedicated
 microservices.
- Platform Teams - Build and maintain the service mesh and operational sidecars 
to support stream-aligned teams.

## Style Characteristics
Strengths:
- high modularity
- high maintainability & scalability
- excellent testability
- deployability
- elasticity and fault tolerance
Weaknesses:
- network and data latency
  - can be mitigated by caching and choreography.
